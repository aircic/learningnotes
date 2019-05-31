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
传感器模型
推进器模型
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
* oceanState：配置海洋参数，定义水的特性，通过osgOcean实现从平静的蓝色海洋到波涛汹涌的大浪。
> * windx,windy: X and Y wind force.
> * windSpeed: Wind speed. Will affect to the amount and height of waves.
> * depth: Sets the ocean's meters height.
> * reflectionDamping: Amount of water reflexed light.
> * waveScale: This parameter decides the wave height, 1e-7 is a reasonable value.
> * isNotChoppy: Set the waves to be choppy.
> * choppyFactor: How choppy the waves are.
> * crestFoamHeight: How high the waves need to be before foam forms on the crest.
> * oceanSurfaceHeight: Z position of the ocean surface in world coordinates.
> * fog: Determines underwater visibility.
>   * * density: Underwater fog density, modifies particle number and visibility.
>   * * color: Underwater fog color (RGB parameters).
> * color: Set the water color (RGB parameters).
> * attenuation: Changes underwater light behaviour.
* simParams：调整仿真器的设置，例如着色、分辨率或物理参数等。这部分中所有标签都可选的，除了showTrajectory，如果标签没有给定值，将采用默认值。
> * **disableShaders**: Disable osgOcean shaders, visual performance may increase but visually quality will get drastically worse. Fixes segmentation fault when graphic card is not able to use shaders.
> * **resw,resh**: Initial window resolution of the scene.
> * **offsetp,offsetr**: World coordinates (position and rotation) offset. It moves the virtual zero pose of the scene. Requires (x,y,z) parameters.
> * **enablePhysics**: Activates or deactivates bullet physics.
> * **gravity**: Sets force and direction of gravity in the scene. Requires (x,y,z) parameters.
> * **physicsFrequency**: simulation rate (default is 60, as in Bullet).
> * **physicsSubSteps**: In case the framerate is lower than 60, a max number of physics calculation steps to be done. 0 (default) for automatic number of substeps.
> * **physicsSolver**: Solver used in bullet engine, to know: Dantzig (default), SolveProjectedGauss, SequentialImpulse.
> * **showTrajectory** Draws the trajectory of a vehicle, object or vehicle part in the simulator. Pressing 't' will show/hide trajectory, although it will keep drawing even in hidden state.
> * * **target** Name of the vehicle, object or vehicle part to draw trajectory. Allows routes using '/' for deambiguation: for example girona500/part4 will draw the trajectory of the base_link present in the girona500 vehicle.
> * * **color** Sets the color of the trajectory. Requires (r,g,b) parameter.
> * * **lineStyle** Allows diferent line patterns configuration. To know:
1: continuous line --------
2: dashed line - - - - - - -
3: dashed line --  --  --  -- 
4: dashed line -   -   -   -
> * **lightRate**: Sets the light ratio on the scene from 0 total darkness to infinity, default value 1.0.

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


