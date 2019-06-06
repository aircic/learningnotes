
机器人模型(model)和描述(description)相关内容放在ROS包/MYROBOT_description，所有Gazebo使用的.world文件和.launch文件放在ROS包/MYROBOT_gazebo。在实际应用中将MYROBOT替换为自己的机器人名称的小写字母。目录结构如下：
```
.../catkin_ws/src
        /MYROBOT_description
            package.xml
            CMakeLists.txt
            /urdf
                MYROBOT.urdf
            /meshes
                mesh1.dae
                mesh2.dae
                ...
            /materials
            /cad
        /MYROBOT_gazebo
            /launch
                /MYROBOT.launch
            /worlds
                MYROBOT.world
            /models
                world_object1.dae
                world_object2.stl
                world_object3.urdf
            /materials
            /plugins
```
##
[Using a URDF in Gazebo]

## build world in Gazebo
[Digital Elevation Models](http://gazebosim.org/tutorials?tut=dem&cat=build_world) -- [中文](https://blog.csdn.net/qq_31736703/article/details/82424440)
[Ground plane model with satellite images](http://gazebosim.org/tutorials?tut=static_map_plugin&cat=build_world)


## gazebo plugin
[Overview of Gazebo Plugins](http://gazebosim.org/tutorials?tut=plugins_hello_world&cat=write_plugin)
[Using Gazebo plugins with ROS](http://gazebosim.org/tutorials?tut=ros_gzplugins)

###插件类型
World：指向并控制Gazebo中特定的world的物理引擎、光照等属性。
Model：指向并控制Gazebo中特定的model的关节、状态等属性。
Sensor：指向并控制Gazebo中特定的sensor属性，获取传感器信息。
System
Visual
GUI

* Model插件提供获取physics::Model API的方法
* Sensor插件提供获取sensors::Sensor API的方法
* Visual插件提供获取rendering::Visual API的方法

###插件构建过程
以一个world插件进行插件构建过程说明，world插件由一个class和几个成员函数构成。
首先确保安装了Gazebo开发环境，如果是从源码安装的[Gazebo](https://bitbucket.org/osrf/gazebo/src/default/)，可以忽略这一个步。
``` shell
sudo apt-get install libgazebo6-dev
```
如果你安装的不是gazebo6，请用你的版本号替换数组6。
然后，为新插件建立一个目录和.cc文件：
``` shell
mkdir ~/gazebo_plugin_tutorial
cd ~/gazebo_plugin_tutorial
vim hello_world.cc
```
复制下面的代码到helo_world.cc文件中：
``` c++
#include <gazebo/gazebo.hh>

namesapce gazebo
{
    class WorldPluginTutorial : public WorldPlugin
    {
        public : WorldPluginTutorial() : WorldPlugin()
        {
            printf("Hello World!\n");
        }
        public : void Load(physics::WorldPtr _world, sdf::ElementPtr _sdf)
        {
        }
    };
    GZREGISTER_WORLD_PLUGIN(WorldPluginTutorial)
}
```
上述源码来自[examples/glugins/hello_world](https://bitbucket.org/osrf/gazebo/src/default/examples/plugins/hello_world/hello_world.cc)。
###源码解析
``` c++
#include <gazebo/gazebo.hh>

namesapce gazebo
{
```
[gazebo/gazebo.hh](https://bitbucket.org/osrf/gazebo/src/default/gazebo/gazebo.hh)包含了gazebo基本函数，但不包含gazebo/physics/physics.hh, gazebo/rendering/rendering.hh和gazebo/sensors/sensors.hh，这三个文件分别对应model、visual和sensor插件类型。所有的插件必须在gazebo的命名空间。
``` c++
    class WorldPluginTutorial : public WorldPlugin
    {
        public : WorldPluginTutorial() : WorldPlugin()
        {
            printf("Hello World!\n");
        }
```
每个插件都要继承一个插件类，本文中是WorldPlugin类。
``` c++
        public : void Load(physics::WorldPtr _world, sdf::ElementPtr _sdf)
        {
        }
```
仅有的强制函数`Load`接收来自URDF/SDF文件中的元素和属性。`physics::WorldPtr _world`为指向插件依附的world的指针，`sdf::ElementPtr _sdf`为指向插件的sdf元素的指针，其他类型的指针`physics::ModelPtr _model`为指向插件依附的model的指针，`_model`为指向插件依附的model的指针，

###编译插件
请确保正确安装了gazebo，在编译之前需要创建
``` shell
gedit ~/gazebo_plugin_tutorial/CMakeLists.txt
```
并将下面程序复制到CMakeLists.txt文件中
``` cmake
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

find_package(gazebo REQUIRED)
include_directories(${GAZEBO_INCLUDE_DIRS})
link_directories(${GAZEBO_LIBRARY_DIRS})
list(APPEND CAMKE_CXX_FLAGS "${GAZEBO_CXX_FLAGS}")

add_library(hello_word SHARED hello_world.cc)
target_link_libraries(hello_world ${GAZEBO_LIBRARIES})
```
创建编译目录
``` shell
mkdir ~/gazebo_plugin_tutorial/build
cd ~/gazebo_plugin_tutorial/build
```
编译代码
``` shell
cmake ../
make
```
编译后生成一个共享库`~/gazebo_plugin_tutorial/build/libhello_world.so`，可以插入到Gazebo仿真中。
最后，将共享库的路径加入到GAZEBO_PLUGIN_PATH中：
``` shell
export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:~/gazebo_plugin_tutorial/build/
```
>>注：如在当前的命令行里运行上面命令，只改变了当前的shell中的路径，如果希望每次打开命令窗口都使用这个插件，需要将上面命令行加入到`~/.bashrc`文件中。

###使用插件
一旦将插件编译成共享库，就可以把它添加到SDF文件中的world或者model内。启动时，Gazebo解析SDF文件，定位插件并加载代码，Gazebo找到插件至关重要，要么说明插件的全局路径，要么插件路径加入到GAZEBO_PLUGIN_PAHT环境变量中。
以在world中调用插件为例进行插件使用说明，先建立一个[wold文件](https://bitbucket.org/osrf/gazebo/src/default/examples/plugins/hello_world/hello.world)
``` shell
gedit ~/gazebo_plugin_tutorial/hello.world
```
在文件中添加下面代码
``` xml
<?xml version="1.0"?>
<sdf version="1.4">
  <world name="default">
    <plugin name="hello_world" filename="libhello_world.so"/>
  </world>
</sdf>
```
通过gzserver命令打开world文件
` gzserver ~/gazebo_plugin_tutorial/hello.world --verbose`
