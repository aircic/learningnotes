## 建立Gazebo ROS仿真步骤
参考
[Gazebo ROS入门](https://www.cnblogs.com/shhu1993/p/5067749.html)
### Ubuntu16.04 ros-kinetic上实践过程
#### 1. 创建ROS工作目录
``` shell
mkdir -p ~/catkin_ws/src
cd ~ /catkin_ws/src
catkin_init_workspace
cd ..
catkin_make
source ~/catkin_ws/devel/setup.bash
```
#### 2. 创建仿真机器人项目
``` shell
cd ~/catkin_ws/src
catkin_create_pkg myusv_gazebo gazebo_ros
catkin_create_pkg myusv_description
catkin_create_pkg myusv_control
```
#### 3. 创建Gagebo环境模型
``` xml
    <include file="$(find gazebo_ros)/launch/empty_world.launch">
        <arg name="use_sim_time" value="true" />
        <arg name="debug" value="false" />
        <arg name="gui" value="true" />
        <arg name="paused" value="true"/>
        <arg name="verbose" value="true"/>
        <arg name="world_name" value="$(find myusv_gazebo)/worlds/myusv.world"/>
    </include>
```
上面程序继承了Gazebo的empty_world.launch的环境模型，但需要将模型参数world_name变换成自己的环境模型myusv.world。在myusv.world中，使用浮力插件libfreefloating_gazebo_fluid.so。

#### 4. 创建机器人模型
船体模型
* gazebo:
  simpleHull3/base_link.dae  
  simpleHull3/centerRight.dae  
  simpleHull3/backLeft.dae  
  simpleHull3/backRight.dae  
  simpleHull3/frontLeft.dae  
  simpleHull3/frontRight.dae  
  simpleHull3/thruster.dae  
* uwsim:
  simpleHull3/base_link.obj  
  simpleHull3/centerRight.obj  
  simpleHull3/backLeft.obj  
  simpleHull3/backRight.obj  
  simpleHull3/frontLeft.obj  
  simpleHull3/frontRight.obj  
  simpleHull3/thruster.obj  
CFD模型参数：
* $\mathbf{M}$ 惯性矩阵(inertia matrix)
* $\mathbf{G(v)}$ 科氏力(Coriolis)和离心力(centrifugal)
* $\mathbf{d(v)}$ 水动力阻尼力(hydrodynamic damping force)
* $\mathbf{\tau_g}$ 重力、浮力和诱导力矩(induced torque)
* $\mathbf{\tau_u}$ 推力
dampingXYZ
angularDampingXYZ
extraPayload
front_body_mass
middle_body_mass
back_body_mass
front_water_displaced_mass
middle_water_displaced_mass
back_water_displaced_mass
frontal_area
lateral_area
lateral_length


传感器模型
  simpleHull3/box.obj  


#### 5. 建立机器人模型与ROS的连接
#### 6. 使用teleop节点控制机器人
#### 7. 机器人增加传感器（摄像头）
#### 8. 使用Rviz可视化机器人信息

## [UWSim](http://www.irs.uji.es/uwsim/wiki/index.php?title=Main_Page)
### [UWSIM主要特性](http://www.irs.uji.es/uwsim/wiki/index.php?title=Main_characteristics_of_UWSim)
#### 可配置的环境
可以使用建模软件如Blender、3D Studio Max等配置3D仿真场景，所有OSG可读的文件格式(.ive, .3ds, .wrl等)都可以加载到仿真器中，构建实际场景。全部场景可以通过XML或xacro文件描述，详见[配置和创建场景](http://www.irs.uji.es/uwsim/wiki/index.php?title=Configuring_and_creating_scenes)。
#### 支持多机器人
默认情况下，机器人对象由3D模型组成，放置在6自由度场景中仿真，模型采用URDF格式描述，包括运动学，动力学和可视化信息。
#### 仿真传感器
UWSIM的1.4版本除了机器人和机械臂关节的位置、速度传感器之外还有12种传感器可以使用。默认的机器人的位置和速度传感器提供6DOF场景中的位姿(x, y, z, r, p, y)，其他传感器如下：
* Camera： 提供虚拟图像，可以用来开发视觉算法。
* Range Camera: 深度图像
* Range Sensor：沿着预定义的方向测量距离最近的障碍物距离
  We have added virtual range sensors to the trunk version of UWSim. Virtual range sensors measure the distance to the nearest obstacle along the X axis of their local frame. They can be attached to vehicles, in a similar way to virtual cameras. This is an example of how to include it into your scene XML file:    
``` xml
    <rangeSensor>
      <name>sonar</name>
      <relativeTo>part0</relativeTo>
      <range>10</range>
      <visible>1</visible>
      <position>
        <x>-0.3</x>
        <y>0</y>
        <z>0</z>
      </position> 
      <orientation>
        <r>0</r>
        <p>1.57</p>
        <y>0</y>
      </orientation>
    </rangeSensor>
```
  The example above adds a virtual range sensor at the given position and orientation with respect to part0, with a maximum range of 10m. The visible tag indicates whether to render the beam in the UWSim scene or not. The measured distance can be published in ROS by adding the following lines inside the rosInterfaces block of your XML:
``` xml
    <RangeSensorToROSRange>
      <name>sonar</name>
      <topic> /g500/range </topic>
      <rate>10</rate>
    </RangeSensorToROSRange>
```
* Object Picker: 当到目标的距离小于预设距离时，假设抓到目标。
* Pressure： 提供压力测量。
* DVL：估计机器人运行的线速度。
* IMU：估计机器人在world坐标系中的姿态。
* GPS：当机器人接近水面时，提供机器人在world坐标系下的位置。
* Multibean：仿真一个距离传感器阵列，提供在固定角度平面上距离最近障碍的距离
  This new sensor available for UWSim vehicles measures multiple distances to nearest obstacles along the -Z axis (osgcamera like) of their local frame varying X rotation from initial angle to final angle in configured angle increments. This sensor is useful to create many virtual range sensor at once in a efficient manner as it uses z-buffer to do its inner calculations.

  As underwater floating particles were causing huge noise in this kind of sensors, multibeam sensors will not be disturbed by them unless "underwaterParticles" attribute is set to true on XML multibeam configuration. This is an example of how to include it into your scene XML file:
``` xml
  <multibeamSensor underwaterParticles="false">
    <name>multibeam</name>
    <relativeTo>part0</relativeTo>
    <position>
      <x>-0.2</x>
      <y> 0.1 </y>
      <z> 0 </z>
    </position>
    <orientation>
      <r>3.14</r>
      <p>0</p>
      <y>3.14 </y>
    </orientation>
    <initAngle>-60</initAngle>
    <finalAngle>60</finalAngle>
    <angleIncr>0.1</angleIncr>
    <range>10</range>
  </multibeamSensor>
```
  The example above adds a multibeam sensor at the given position and orientation with respect to part0, with a maximum range of 10m measuring from -60º to 60º in increments of 0.1º. “underwaterParticles” attribute turns on or off floating particles noise and is set to false by default. The measured distances can be published in ROS by adding the following lines inside the rosInterfaces block of your XML:
``` xml
  <multibeamSensorToLaserScan>
    <name>multibeam</name>
    <topic>g500/multibeam</topic>
  </multibeamSensorToLaserScan>
```
* Force：估计机器人部件的力和扭矩。
  Force sensors are now available on UWSim. It measures the external forces on a part of a vehicle. It can be added to every visual part of a vehicle and it will measure the collision forces the piece is suffering in Newtons force torque.

  This sensor uses Bullet physics engine to create two identical collision shapes one colliding with external objects and one "ghost" that traverses them, at the end of each physics iteration measuring the difference between these two collision shapes allows us to calculate the force suffered by the object. Physical properties of the vehicle part such as weight should be correctly provided on URDF to obtain realistic results. For instance, this sensor has been used to measure collision forces of the girona500 vehicle and send this information to underwater vehicle dynamics module which uses it as an input for external forces, the result is vehicle can no longer traverse objects as we can see in the embedded video. The XML code needed to add one is:
``` xml
  <ForceSensor>
    <name>ForceG500</name>
    <target>base_link</target>
    <offsetp>
      <x>-0.2</x>
      <y>0.75</y>
      <z>0</z>
    </offsetp>
    <offsetr>
      <x>-1.57</x>
      <y>0</y>
      <z>3.14</z>
    </offsetr>
  </ForceSensor>
```
  The example shows that only a target and offset from the part's center is needed to add a force sensor. The measured force can be published in ROS by adding the following lines inside the rosInterfaces block of your XML:
``` xml
  <ForceSensorROS>
    <name>ForceG500</name>
    <topic>g500/ForceSensor</topic>
    <rate>100</rate>
  </ForceSensorROS>
```
* Structured light projector：在场景中投影激光或者普遍光
  The UWSim structured light projector displays user configured textures over the scene along the -Z axis (osgcamera like) of their local frame . This projector is able to simulate arbitrary laser and vehicle lights casting realistic shadows. Here we can see an example of use:
``` xml
  <structuredLightProjector>
    <name>laser_projector</name>
    <relativeTo>sls</relativeTo>
    <fov>21</fov>
    <image_name>data/textures/laser_lines.png</image_name>
    <laser>1</laser>
    <position>
      <x>1</x>
      <y>0</y>
      <z>0</z>
    </position>
    <orientation>
      <r>0</r>
      <p>3.14</p>
      <y>1.57</y>
    </orientation>
  </structuredLightProjector>
```
  As usual the previous XML code adds a structured light projector at the given position and orientation with respect to sls. fov tag configures the field of view of the projector (also known as angle of view). image_name tag provides the image to project, RGBA images are allowed but alpha channel will be used as a binary value to project or not project the texture on every pixel. Finally laser label decides if the projection should be treated as laser or not, main difference is laser light does not suffer attenuation along distance and it does not mix with objects color.
* Dredge：挖掘被埋目标。
  Today we are presenting a new feature recently developed in uwsim: dredging. Now every object may be "buried" in sand to a percent of their height, configurable via XML. Furthermore buried objects may be "unearthed" using a dredge tool. This dredge tools are created in vehicles configured via XML setting its position and create an animation of the dredged particles using osgParticle.
  To configure a buried object just an attributte in the object is needed stating the buried percent:
`  <object buried="0.6">`
  and the dredge tool may be created using a DredgeTool device inside vehicles, for instance:
``` xml
  <DredgeTool>
    <name>dredgeGripper</name>
    <target> end_effector</target>
    <offsetp>
      <x>0</x>
      <y>0</y>
      <z>0</z>
    </offsetp>
    <offsetr>
      <x>0</x>
      <y>0</y>
      <z>0</z>
    </offsetr>
  </DredgeTool>
```
  Although this feature is already working in last git version, some fixes and enhancements are coming. As usual any bug report or comment can be done through the github project.
### [配置和创建场景](http://www.irs.uji.es/uwsim/wiki/index.php?title=Configuring_and_creating_scenes)
UWSim中的场景通过XML文件进行配置并由DTD文件确认，所以创建或调整场景只需要编辑相应的XML文件，推荐使用xacro创建自己的设备、目标、机器人库。
#### UWSim场景
UWSim场景由.xml文件组成，默认存储位置在data/scenes文件夹，该文件夹下有：
* g500ARM5.xml：一个搭载ARM5机械手的G500水下机器人3D模型。
* cirs.xml：Girona大学水下机器人人研究中心的水池模型，水面8*16m，最大深度5m，场景中包含一台配备ARM5机械臂的G500水下机器人，G500搭载两个摄像头、距离传感器、imu、压力传感器、gps、dvl、深度相机、多波束传感器等。...
XML文件描述普通情景和仿真参数，URDF描述机器人。一个场景XML文件可以将机器人的URDF文件加载到场景中。通过编写XML文件来创建自己的场景，然后运行UWSim加载场景，命令如下
`rosrun uwsim uwsim --configfile path/to/yourscene.xml`
#### XML语法
UWSim场景XML文件可以分成几块，每块定义不同方面的场景：
* **oceanState**：配置海洋参数，定义水的特性，通过osgOcean实现从平静的蓝色海洋到波涛汹涌的大浪。
> * **windx,windy**: X and Y wind force.
> * **windSpeed**: Wind speed. Will affect to the amount and height of waves.
> * **depth**: Sets the ocean's meters height.
> * **reflectionDamping**: Amount of water reflexed light.
> * **waveScale**: This parameter decides the wave height, 1e-7 is a reasonable value.
> * **isNotChoppy**: Set the waves to be choppy.
> * **choppyFactor**: How choppy the waves are.
> * **crestFoamHeight**: How high the waves need to be before foam forms on the crest.
> * **oceanSurfaceHeight**: Z position of the ocean surface in world coordinates.
> * **fog**: Determines underwater visibility.
>   * **density**: Underwater fog density, modifies particle number and visibility.
>   * **color**: Underwater fog color (RGB parameters).
> * **color**: Set the water color (RGB parameters).
> * **attenuation**: Changes underwater light behaviour.
* **simParams**：调整仿真器的设置，例如着色、分辨率或物理参数等。这部分中所有标签都可选的，除了**showTrajectory**，如果标签没有给定值，将采用默认值。
> * **disableShaders**: Disable osgOcean shaders, visual performance may increase but visually quality will get drastically worse. Fixes segmentation fault when graphic card is not able to use shaders.
> * **resw,resh**: Initial window resolution of the scene.
> * **offsetp,offsetr**: World coordinates (position and rotation) offset. It moves the virtual zero pose of the scene. Requires (x,y,z) parameters.
> * **enablePhysics**: Activates or deactivates bullet physics.
> * **gravity**: Sets force and direction of gravity in the scene. Requires (x,y,z) parameters.
> * **physicsFrequency**: simulation rate (default is 60, as in Bullet).
> * **physicsSubSteps**: In case the framerate is lower than 60, a max number of physics calculation steps to be done. 0 (default) for automatic number of substeps.
> * **physicsSolver**: Solver used in bullet engine, to know: Dantzig (default), SolveProjectedGauss, SequentialImpulse.
> * **showTrajectory** Draws the trajectory of a vehicle, object or vehicle part in the simulator. Pressing 't' will show/hide trajectory, although it will keep drawing even in hidden state.
>   * **target** Name of the vehicle, object or vehicle part to draw trajectory. Allows routes using '/' for deambiguation: for example girona500/part4 will draw the trajectory of the base_link present in the girona500 vehicle.
>   * **color** Sets the color of the trajectory. Requires (r,g,b) parameter.
>   * **lineStyle** Allows diferent line patterns configuration. To know:
1: continuous line --------
2: dashed line - - - - - - -
3: dashed line --  --  --  -- 
4: dashed line -   -   -   -
> * **lightRate**: Sets the light ratio on the scene from 0 total darkness to infinity, default value 1.0.

* **Camera** block sets the main camera parameters and configurse the initial main view and characteristics of the simulator's scene. Is one of the main blocks when configuring and creating scenes.
>* **freemotion:** This parameter sets if the main view is able to move with trackball or not.
>* **objectToTrack:** Sets the object to track if freemotion is not set. Main view will follow that object.
>* **fov:** Field fo view of the main camera.
>* **aspectRatio:** Proportional relationship between width and height(width/height).
>* **near:** Distance to the near plane, anything nearer this distance will not be showed.
>* **far:** Distance to the far plane, anything further this distance will not be showed.
>* **position:** Initial position of the viewer in the world.
>* **lookAt:** Sets the initial rotation of the viewer.
* **vehicle** tag is used to create and figure underwater robots and the sensors availabe on them.
>* **name:** This parameters sets the vehicle's name that will identify it.
>* **file:** The URDF file where full description of joints, visual mesh, collision mesh...etc may be provide.
> * **postion:** Initial position of the vehicle.
> * **orientation:** Initial rotation of the vehicle, stated in RPY.
> * **scaleFactor:** Scale for the complete vehicle(every link in URDF will be scaled).
> * **[virtualCamera](http://www.irs.uji.es/uwsim/wiki/index.php?title=VirtualCamera):** Describes the characteristics of the camera attached to the vehicle.
> * **[StructuredLightProjector](http://www.irs.uji.es/uwsim/wiki/index.php?title=StructuredLightProjector):** Light projector features, may be used to simulated a laser projector or light.
> * **[rangeSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=RangeSensor):** Measures the distance to the nearest obstacle.
> * **[objectPicker](http://www.irs.uji.es/uwsim/wiki/index.php?title=ObjectPicker):** Simulates a grasp(without physics) attaching the nearest object to the sensor when it reaches a certain distance.
> * **[imu](http://www.irs.uji.es/uwsim/wiki/index.php?title=Imu):** Estimates the vehicle rotation with respect to the world.
> * **[pressureSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=PressureSensor):** Pressure sensor.
> * **[gpsSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=GpsSensor):** Gives the vehicle world position. Note only works when the vehicle is near the surface.
> * **[dvlSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=DvlSensor):** Estimates vehicle linear speed
> * **[virtualRangeImage](http://www.irs.uji.es/uwsim/wiki/index.php?title=VirtualRangeImage):** greyscale distance image, analog to virtualCameras but with range information.
> * **[multibeamSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=MultibeamSensor):** Simulates an array of rangeSensors, returning the distance to the nearest obstacle in the ZX plane.
> * **simulatedDevices:** Used for creating devices using the new interface for them. Deprecated, new simualted devices can be added without using it.
> * **echo:** Example test device for new interface devices.
> * **[ForceSensor](http://www.irs.uji.es/uwsim/wiki/index.php?title=ForceSensor):** Uses bullet physics to simulate a force sensor in a link of the vehicle returning force torque forces for collisions.
> * **[DredgeTool](http://www.irs.uji.es/uwsim/wiki/index.php?title=DredgeTool):** Dredging tool available for unearthing sand buried objects.

* **object**block allows to include other 3D models to the sence to interact with robots.
> * **name:** This parameter sets the object's name that will identify it.
> * **file:** The 3D mesh file that describes the object. About the formats, input is supported through OSG, so it depends on osg plugins, but most common 3d formats such as .3ds, osg, .ive, .stl, .obj are supported. Some others may need additinal OSG libraries such as .dae or .wrl.
> * **position:** Inital position of the object.
> * **orientation:** Initial rotation of the object, stated in RPY.
> * **scaleFactor:** Scale of the object.(XYZ)
> * **offsetp:** Object's position offset, it moves the pivot point
> * **offsetr:** Object's rotation offset.
> * **[physics](http://www.irs.uji.es/uwsim/wiki/index.php?title=Physics):** This tag allows to set some physical properties for bullet physics simulation.

* **rosInterfaces** allow to attach ROS interfaces tro certain objects, robots or sensors, specifying the communnication posibilities.
> * **[ROSOdomToPAT](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSOdomToPAT):** This subsriber reads from a ROS odometry message and moves the target vehicle accordingly.
> * **[PATToROSOdom](http://www.irs.uji.es/uwsim/wiki/index.php?title=PATToROSOdom):** This interface publishes the target vehicle pose and velocity in a ROS odometry message.
> * **[WorldToROSTF](http://www.irs.uji.es/uwsim/wiki/index.php?title=WorldToROSTF):** TF ROS interface that publishes the pose of every object, vehicle and device present in the scene.
> * **[ArmToROSJointState](http://www.irs.uji.es/uwsim/wiki/index.php?title=ArmToROSJointState):** Robotic arm interface that publishes ROS joint state messages with the information of non-fixed joints.
> * **[ROSJointStateTOArm](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSJointStateToArm):** This subscriber read ROS joint state messages and move the arm accordingly.
> * **[VirtualCameraToROSImage](http://www.irs.uji.es/uwsim/wiki/index.php?title=VirtualCameraToROSImage):** Creates a ROS image publisher with the desired camera
> * **[ROSImageToHUD](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSImageToHUD):** Subscribes to ROS image publisher to add a camera inside UWSim(for instance real input).
> * **[ROSTwistToPAT](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSTwistToPAT):** This subscriber reads ROS Twist messages to set the velocity to the target vehicle.
> * **[RangeSensorToROSRange](http://www.irs.uji.es/uwsim/wiki/index.php?title=RangeSensorToROSRange:** Range devices publisher. Publishes the distance read by the virtual sensor.
> * **[ROSPoseToPAT](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSPoseToPAT):** Subscriber that reads ROS Pose messages and moves the vehicle to the stated position.
> * **[ImuToROSImu](http://www.irs.uji.es/uwsim/wiki/index.php?title=ImuToROSImu):** This interface publishes an Inertial Measurement Unit readings.
> * **[PressureSensorToROS]():** Publisher for pressure sensors.
> * **[GPSSensorToROS]():** Reads the information from GPS sensors and publish it.
> * **[DVLSensorToROS]():** Creates a publisher for DVL devices.
> * **[RangeImageSensorToROSImage]():** Uses the ROS Image publisher to send depth images from a range image sensor.
> * **[multibeamSensorToLaserScan]():** Transforms the multibeam sensor readings in a LaserScan message and publishes it.
> * **SimulatedDeviceROS:** : Used for creating ROS interfaces inside the new plugin architecture. Deprecated, no longer needed.
> * **[contactSensorToROS](http://www.irs.uji.es/uwsim/wiki/index.php?title=ContactSensorToROS):** Publishes a binary output depending on the vehicle collision state.
> * **echoROS:** Device plugin example.
> * **[ForceSensorROS](http://www.irs.uji.es/uwsim/wiki/index.php?title=ForceSensorROS):** This interface publishes the information of force sensors in a ROS Wrench stamped message.
> * **[ROSPointCloudLoader](http://www.irs.uji.es/uwsim/wiki/index.php?title=ROSPointCloudLoader):** This subscriber reads from ROS Pointcloud messages and represents them in the 3D UWSim view.
> * **[RangeCameraToPCL](http://www.irs.uji.es/uwsim/wiki/index.php?title=RangeCameraToPCL):** This subscriber uses a virtualRangeImage camera and publishes a Pointcloud as result.

#### 使用xacro创建自己的场景
xacro使用预定义的传感器、目标、机器人等的XML块创建自己的库，一旦XML块在一个库中被定义，我们可以在任意其他文件中调用它，而不必重复写代码。基本的macro已经被创建并可以获得实例库，下面的文件已经被创建，可以在UWSim中的data/scenes目录下找到：
* **[Common](http://www.irs.uji.es/uwsim/wiki/index.php?title=Common):**Basic generic macros for UWSimScenes. This macros create the most common XML blocks such as oceanState, simParams, vehicles, sensors, objects,... . This macros should be used as a starting point to create your own libraries to be used in scenes. If you want to know how parameters work visit the XML description in the corresponding block. This macros are available at common.xacro. Most macros are preceded by an underscore - as xacro does not allow to create a macro with the same name as any defined XML tag.
* **[DeviceLibrary](http://www.irs.uji.es/uwsim/wiki/index.php?title=DeviceLibrary):** is a xacro library example for sensors and actuators available for uwsim vehicles when configuring and creating scenes. This library creates some macros for specific sensors that can be easily added to any vehicle. We recommend to create you own device library with the common devices you use in your simulations such as cameras, multibeams, sonars... etc. So you just have to specify its parameters once.
  * **bowtech1:** This macro creates a bowtech camera, identical to the cirs.xml scene. Requires relativeTo, x, y, z, roll, pitch, yaw.
  * **bowtech_iface:** This macro creates a ROS virtual camera interface for bowtech camera. Doesn't require any argument.
  * **example_multibeam:** Creates a multibeam identical to the one in cirs.xml scene. Requires pose argumentes: relativeTo, x, y, z, roll, pitch, yaw.
  * **example_multibeam_iface:** Creates a multibeam interface for the example multibeam.
* **[ObjectLibrary](http://www.irs.uji.es/uwsim/wiki/index.php?title=ObjectLibrary):** is a xacro library example for objects available for uwsim scenes when configuring and create scenes. This library creates some macros for specfic objects that can be easily added to any scene. We recommend to create your own object library with common objects you use in your simulations such as terrains, rocks, black boxes, ... . So you just have to specfiy its parameters once.
In the example objectLibrary.xacro we have created two object macros. We left pose arguments to be able to attach them anywhere in the scene.

  * **CIRSterrain:** This macro creates CIRS pool that can be seen in cirs.xml scene. Note it already has the offsets and physical parameters, so only pose arguments are required: x, y, z, roll, pitch, yaw.
  * **blackbox:** This macro creates the blackbox present in the cirs.xml scene. As it happens with CIRSterrain, physical parameters are already set so only x, y, z, roll, pitch, yaw parameters must be filled when using it.
* **[VehicleLibrary](http://www.irs.uji.es/uwsim/wiki/index.php?title=VehicleLibrary):** is a xacro library example for vehicle available for uwsim when configuring and creating scenes. This library creates some macros for specific vehicle configuration that can be easily added to any scene. We recommend to create you own vehicle library with the common vehicles you use in your simulations such as the girona500 with the ARM5. So you just have to specify its parameters once.
In the example vehicleLibrary.xacro we have created an girona500 configuration example, and a interfaces example. We left pose arguments to be able to attach it anywhere in the scene, and initial joints to start the intervention in the desired position.

  * **example_g500ARM5:** This macro creates a simplified girona500 with the ARM5 arm a camera and a multibeam (using the deviceLibrary macros). Requires x, y, z, roll, pitch, yaw, **initialJoints.
  * **example_g500ARM5_ifaces:** Creates a set of ROS interfaces for the example girona500ARM5 vehicle. It creates a RosOdomTOPAT to move the vehicle, TF publisher, subscriber and publisher for the arm movements and interfaces for the two previously added sensors using deviceLibrary interface macros.


#### [XML配置实例：CIRS场景](http://www.irs.uji.es/uwsim/wiki/index.php?title=Configuring_and_creating_scenes)
uwsim\data\scenesAPAGAR\cirs.xml







## robots_start.launch
``` xml
<!-- launch gazebo world-->
<include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="$(find usv_sim)/world/empty.world"/>
</include>
<!-- launch uwsim-->
<node name="uwsim" pkg="uwsim" type="uwsim" args="$(arg disableShaders) --dataPath $(find usv_sim) --configfile scenes/robots_start.xml" respawn="false" required="true"/>

<!--robots_start_spawner.launch from spawning robots_start.launch-->
<group>
    <param name="diffboat/robot_description" command="$(find xacro)/xacro --inorder $(find usv_sim)/xacro/diffboat.xacro"/>
    <node name="diffboat_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model diffboat -param diffboat/robot_description -x 0 -y 0 -z 1 -R 0 -P 0 -Y 0"/>
</group>

<!--usv_sim/launch/models/spawn_diffboat.launch-->
<arg name="gui" default="false"/>
<arg name="parse" default="false"/>  
<arg name="namespace" default="diffboat"/>  
<arg name="posX" default="0"/>
<arg name="posY" default="0"/>
<arg name="posZ" default="0"/>
<arg name="spawnGazebo" default="false"/>
<arg name="windType" default="global"/>
<arg name="waterType" default="global"/>
<group ns="$(arg namespace)" unless="$(arg parse)">
    <param name="robot_description" command="$(find xacro)/xacro $(find usv_sim)/xacro/diffboat.xacro waterType:=$(arg waterType) windType:=$(arg windType)"/>
    <node unless="$(arg spawnGazebo)" name="spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model diffboat -param robot_description -x $(arg posX) -y $(arg posY) -z $(arg posZ) -R 0 -P 0 -Y 0"/>

    <!-- Load controller configurations (vehicle and arm) from YAML file to parameter server -->
    <rosparam file="$(find usv_sim)/config/diffboat.yaml" command="load"/>

    <!-- Launch control -->
    <node name="pid_control" pkg="freefloating_gazebo" type="pid_control" output="screen" respawn="true"/>
    <!--<node name="control_vel" pkg="usv_base_ctrl" type="boat_diff_vel_ctrl.py"  unless="$(arg gui)"/>-->
    <node name="heading_control" pkg="usv_base_ctrl" type="diff_control_heading.py" unless="$(arg gui)" output="screen" />
    <node name="odom_base_tf" pkg="usv_tf" type="world_tf_broadcaster.py"/> 
    <node pkg="tf" type="static_transform_publisher" name="laser_base_tf" args="0.5 0 0.2 0 0 0 1 base_link diffboat/base_laser 10" />

    <node name="odom_relay" type="relay" pkg="topic_tools" args="state /odom" />
    <node name="vel_relay" type="relay" pkg="topic_tools" args="/navigation_velocity_smoother/raw_cmd_vel cmd_vel" />


    <!-- GUI interface to control joints -->
        <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" if="$(arg gui)"/>             
    <!-- GUI interface to control thrusters -->
        <group ns="thrusters">
                <param name="robot_description" command="$(find xacro)/xacro --inorder $(find usv_sim)/urdf/diffboat_dummy.urdf"/>                         
                <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" respawn="true" if="$(arg gui)">
                <param name="use_gui" value="True"/>
                <remap from="joint_states" to="/$(arg namespace)/thruster_command" />
                </node>             
        </group>

</group>
```
## diffboat_scenario2.launch
```xml
<!--Launch Gazebo with empty world-->
<include file="$(find gazebo_ros)/launch/empty_world.launch">
  <arg name="world_name" value="$(find usv_sim)/world/empty.world"/>
    <plugin name="freefloating_gazebo_fluid"filename="libfreefloating_gazebo_fluid.so"/>
<!--Launch uwsim-->
<node name="uwsim" pkg="uwsim" type="uwsim" args="$(arg disableShaders) --dataPath $(find usv_sim) --configfile scenes/diffboat_scenario2.xml" respawn="false" required="true"/>
  <vehicle>urdf/diffboat_uwsim.urdf</vehicle>
    <mesh filenames="meshes/simpleHull3/base_link.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/centerRight.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/backLeft.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/backRight.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/frontLeft.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/frontRight.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/box.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/thruster.obj" scale="0.21 0.21 0.21"/>
    <mesh filenames="meshes/simpleHull3/thruster.obj" scale="0.21 0.21 0.21"/>
  <vehicle>urdf/buoy_uwsim.urdf</vehicle>
     <mesh filename="meshes/buoy2.osg" scale="0.5 0.5 0.5"/>
  <object>terrain/difluvio5/novo4.obj<object/>
<!--Launch diffboat and buoy-->
<incldue file="$(find usv_sim)/launch/scenarios_launchs/diffboat_scenario2_spawner.launch"/>
  <param name="diffboat/robot_description" command="$(find xacro)/xacro --inorder $(find usv_sim)/xacro/diffboat.xacro"/>
    <xacro:include filename="$(find usv_sim)/xacro/boat_subdivided_validated.xacro" />
      <mesh filename="package://usv_sim/meshes/simpleHull3/base_link.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/centerRight.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/backLeft.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/backRight.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/frontLeft.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/frontRight.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/thruster.dae" scale="0.21 0.21 0.21"/>
      <mesh filename="package://usv_sim/meshes/simpleHull3/box.dae" scale="0.1 0.1 0.1"/>
      <plugin name="gazebo_ros_head_hokuyo_controller" filename="libgazebo_ros_gpu_laser.so">
  
    <plugin name="freefloating_gazebo_control" filename="libfreefloating_gazebo_control.so"/>
  <node name="diffboat_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model diffboat -param diffboat/robot_description -x 240 -y 100 -z 1 -R 0 -P 0 -Y 0"/>
  <param name="buoy1/robot_description" command="$(find xacro)/xacro --inorder $(find usv_sim)/xacro/buoy.xacro"/>
    <mesh filename="package://usv_sim/meshes/buoy2.dae" scale="0.5 0.5 0.5"/>
  <node name="buoy1_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model buoy1 -param buoy1/robot_description -x 250 -y 100 -z 1 -R 0 -P 0 -Y 0"/>
  <param name="terrain_description" command="$(find xacro)/xacro --inorder $(find usv_sim)/terrain/diluvio5/novo4.sdf"/>
    <mesh <uri>file://$(find usv_sim)/terrain/diluvio5/novo4.dae</uri>/>
  <node name="terrain_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-sdf -model terrain -param terrain_description -x 0 -y 558 -z 0 -R 0 -P 0 -Y 0"/>
<!--navigation node-->
<node name="patrol" pkg="usv_navigation" type="patrol_pid_scene2.py" ns="diffbot" unless="$(arg gui)"/>
<!--control node-->
<include file="$(find usv_sim)/launch/models/spawn_diffboat.launch">
  <rosparam file="$(find usv_sim)/config/diffboat.yaml" command="load"/>
  <node name="pid_control" pkg="freefloating_gazebo" type="pid_control" output="screen" respawn="true"/>
  <node name="heading_control" pkg="usv_base_ctrl" type="diff_control_heading.py" unless="$(arg gui)" output="screen" />
  <node name="odom_base_tf" pkg="usv_tf" type="world_tf_broadcaster.py"/>
  <node pkg="tf" type="static_transform_publisher" name="laser_base_tf" args="0.5 0 0.2 0 0 0 1 base_link diffboat/base_laser 10" />
  <node name="odom_relay" type="relay" pkg="topic_tools" args="state /odom" />
  <node name="vel_relay" type="relay" pkg="topic_tools" args="/navigation_velocity_smoother/raw_cmd_vel cmd_vel" />



```

Node [/uwsim]
Publications: 
 * /buoy1/Surface/link [geometry_msgs/Point]                 => /gazebo
 * /buoy2/Surface/link [geometry_msgs/Point]                 => /gazebo
 * /buoy3/Surface/link [geometry_msgs/Point]                 => /gazebo
 * /diffboat/Imu [sensor_msgs/Imu]                           => 
 * /diffboat/Surface/back_l_link [geometry_msgs/Point]       => /gazebo
 * /diffboat/Surface/back_r_link [geometry_msgs/Point]       => /gazebo
 * /diffboat/Surface/base_link [geometry_msgs/Point]         => /gazebo
 * /diffboat/Surface/center_r_link [geometry_msgs/Point]     => /gazebo
 * /diffboat/Surface/front_l_link [geometry_msgs/Point]      => /gazebo
 * /diffboat/Surface/front_r_link [geometry_msgs/Point]      => /gazebo
 * /diffboat/Surface/fwd_left [geometry_msgs/Point]          => /gazebo
 * /diffboat/Surface/fwd_right [geometry_msgs/Point]         => /gazebo
 * /rosout [rosgraph_msgs/Log]                               => /rosout

Subscriptions: 
 * /buoy1/state [nav_msgs/Odometry]                          <= /gazebo
 * /buoy2/state [nav_msgs/Odometry]                          <= /gazebo
 * /buoy3/state [nav_msgs/Odometry]                          <= /gazebo
 * /clock [rosgraph_msgs/Clock]                              <= /gazebo
 * /diffboat/joint_states [sensor_msgs/JointState]           <= /gazebo
 * /diffboat/state [nav_msgs/Odometry]                       <= /gazebo
 * /tf [tf2_msgs/TFMessage]                                  <= /diffboat/odom_base_tf /diffboat/laser_base_tf /gazebo
 * /tf_static [unknown type]                                 <= /
 * /uwsim_marker/update [unknown type]                       <= /
 * /uwsim_marker/update_full [unknown type]                  <= /

Services: 
 * /SpawnMarker
 * /uwsim/get_loggers
 * /uwsim/set_logger_level

Node [/gazebo]
Publications: 
 * /buoy1/state [nav_msgs/Odometry]                          => /uwsim
 * /buoy2/state [nav_msgs/Odometry]                          => /uwsim
 * /buoy3/state [nav_msgs/Odometry]                          => /uwsim
 * /clock [rosgraph_msgs/Clock]                              => /diffboat/heading_control /diffboat/patrol /diffboat/odom_base_tf /diffboat/pid_control /diffboat/vel_relay /diffboat/laser_base_tf /diffboat/odom_relay
 * /diffboat/joint_states [sensor_msgs/JointState]           => /diffboat/pid_control /uwsim
 * /diffboat/state [nav_msgs/Odometry]                       => /diffboat/odom_relay /diffboat/odom_base_tf /diffboat/odom_base_tf /diffboat/heading_control /uwsim
 * /diffboat/thruster_use [sensor_msgs/JointState]           => None
 * /gazebo/link_states [gazebo_msgs/LinkStates]              => None
 * /gazebo/model_states [gazebo_msgs/ModelStates]            => None
 * /gazebo/parameter_descriptions [dynamic_reconfigure/ConfigDescription]  => None
 * /gazebo/parameter_updates [dynamic_reconfigure/Config]    => None
 * /model/state [nav_msgs/Odometry]                          => None
 * /rosout [rosgraph_msgs/Log]                               => /rosout
 * /scan [sensor_msgs/LaserScan]                             => None
 * /tf [tf2_msgs/TFMessage]                                  => /uwsim

Subscriptions: 
 * /buoy1/Surface/link [geometry_msgs/Point]
 * /buoy2/Surface/link [geometry_msgs/Point]
 * /buoy3/Surface/link [geometry_msgs/Point]
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/Surface/back_l_link [geometry_msgs/Point]
 * /diffboat/Surface/back_r_link [geometry_msgs/Point]
 * /diffboat/Surface/base_link [geometry_msgs/Point]
 * /diffboat/Surface/center_r_link [geometry_msgs/Point]
 * /diffboat/Surface/front_l_link [geometry_msgs/Point]
 * /diffboat/Surface/front_r_link [geometry_msgs/Point]
 * /diffboat/Surface/fwd_left [geometry_msgs/Point]
 * /diffboat/Surface/fwd_right [geometry_msgs/Point]
 * /diffboat/joint_command [sensor_msgs/JointState]          <= /diffboat/pid_control
 * /diffboat/thruster_command [sensor_msgs/JointState]       <= /diffboat/heading_control>
 * /gazebo/current [unknown type]
 * /gazebo/gazebocurrent [unknown type]
 * /gazebo/set_link_state [unknown type]
 * /gazebo/set_model_state [unknown type]

Services: 
 * /gazebo/apply_body_wrench[gazebo_msgs/ApplyBodyWrench]
 * /gazebo/apply_joint_effort[gazebo_msgs/ApplyJointEffort]
 * /gazebo/clear_body_wrenches[gazebo_msgs/ClearBodyWrenches]
 * /gazebo/clear_joint_forces[gazebo_msgs/ClearJointForces]
 * /gazebo/delete_light
 * /gazebo/delete_model[gazebo_msgs/DeleteModel]
 * /gazebo/get_joint_properties[gazebo_msgs/GetJointProperties]
 * /gazebo/get_light_properties
 * /gazebo/get_link_properties[gazebo_msgs/GetLinkProperties]
 * /gazebo/get_link_state[gazebo_msgs/GetLinkState]
 * /gazebo/get_loggers
 * /gazebo/get_model_properties
 * /gazebo/get_model_state
 * /gazebo/get_physics_properties
 * /gazebo/get_world_properties
 * /gazebo/pause_physics[std_srvs/Empty]
 * /gazebo/reset_simulation
 * /gazebo/reset_world
 * /gazebo/set_joint_properties
 * /gazebo/set_light_properties
 * /gazebo/set_link_properties
 * /gazebo/set_link_state
 * /gazebo/set_logger_level
 * /gazebo/set_model_configuration
 * /gazebo/set_model_state
 * /gazebo/set_parameters
 * /gazebo/set_physics_properties
 * /gazebo/spawn_sdf_model
 * /gazebo/spawn_urdf_model
 * /gazebo/unpause_physics[std_srvs/Empty]

 Node [/diffboat/heading_control]
Publications: 
 * /diffboat/move_usv/result [std_msgs/Float64]             => /diffboat/patrol
 * /diffboat/thruster_command [sensor_msgs/JointState]      => /gazebo
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/move_usv/goal [nav_msgs/Odometry]              <= /diffboat/patrol
 * /diffboat/state [nav_msgs/Odometry]                      <= /gazebo

Services: 
 * /diffboat/heading_control/get_loggers
 * /diffboat/heading_control/set_logger_level

Node [/diffboat/laser_base_tf]
Publications: 
 * /rosout [rosgraph_msgs/Log]
 * /tf [tf2_msgs/TFMessage]                                 => /uwsim

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]

Services: 
 * /diffboat/laser_base_tf/get_loggers
 * /diffboat/laser_base_tf/set_logger_level

Node [/diffboat/odom_base_tf]
Publications: 
 * /rosout [rosgraph_msgs/Log]
 * /tf [tf2_msgs/TFMessage]                                => /uwsim

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/state [nav_msgs/Odometry]                     <= /gazebo

Services: 
 * /diffboat/odom_base_tf/get_loggers
 * /diffboat/odom_base_tf/set_logger_level

Node [/diffboat/odom_relay]
Publications: 
 * /odom [nav_msgs/Odometry]
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/state [nav_msgs/Odometry]                     => /gazebo

Services: 
 * /diffboat/odom_relay/get_loggers
 * /diffboat/odom_relay/set_logger_level

Node [/diffboat/patrol]
Publications: 
 * /diffboat/move_usv/goal [nav_msgs/Odometry]             => /diffboat/heading_control
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/move_usv/result [std_msgs/Float64]            <= /diffboat/heading_control

Services: 
 * /diffboat/patrol/get_loggers
 * /diffboat/patrol/set_logger_level


Node [/diffboat/pid_control]
Publications: 
 * /diffboat/joint_command [sensor_msgs/JointState]        => /gazebo
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /diffboat/joint_setpoint [unknown type]
 * /diffboat/joint_states [sensor_msgs/JointState]        <= /gazebo

Services: 
 * /diffboat/controllers/joint_position_control
 * /diffboat/controllers/joint_velocity_control
 * /diffboat/pid_control/get_loggers
 * /diffboat/pid_control/set_logger_level

 Node [/diffboat/thrusters/joint_state_publisher]
Publications: 
 * /diffboat/thruster_command [sensor_msgs/JointState]
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]

Services: 
 * /diffboat/thrusters/joint_state_publisher/get_loggers
 * /diffboat/thrusters/joint_state_publisher/set_logger_level


Node [/diffboat/vel_relay]
Publications: 
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /clock [rosgraph_msgs/Clock]
 * /navigation_velocity_smoother/raw_cmd_vel [unknown type]

Services: 
 * /diffboat/vel_relay/get_loggers
 * /diffboat/vel_relay/set_logger_level
