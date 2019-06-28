## 目录
[toc]
## 版本说明
uubuntu-16.04.6-desktop-amd64
ros-kinetic

## u盘安装ubuntu出现：[error5]Input/output error的解决办法之一
[U盘格式为fat32，而电脑的磁盘格式为ntfs，解决办法：将u盘手动格式化为NTFS,然后用工具直接将ISO写入到u盘](https://blog.csdn.net/m0_37775034/article/details/86497983)

## ubuntu安装教程
[教程1](https://blog.csdn.net/liuxiaodong400/article/details/80946225)
[教程2](https://www.jianshu.com/p/2ad73fb3855e)
[教程3](https://blog.csdn.net/hitzijiyingcai/article/details/81627816)
[教程4](https://blog.csdn.net/lingyunxianhe/article/details/81415675)

## ubuntu16.04实现远程桌面登入
[VNC实现Windows远程访问Ubuntu 16.04](https://www.cnblogs.com/xuliangxing/p/7642650.html)
(远程太卡了，找其他方法)
[Ubuntu中vnc,nomachine或Teamviwer等远程终端无显示器卡顿问题](https://blog.csdn.net/u010284636/article/details/80915659)
``` shell
sudo apt install xorg-video-abi-20 xserver-xorg-core
sudo apt install  xserver-xorg-video-dummy

sudo wget -P /etc/X11 https://gist.githubusercontent.com/mangoliou/ba126832f2fb8f86cc5b956355346038/raw/b6ad063711226fdd6413189ad905943750d64fd8/xorg.conf
```
关闭和启动vncserver
```shell
vncserver -kill :1
vncserver :1
```
调整vnc显示分辨率同windows桌面分别率：`vncserver -geometry 1920x1200`
## ubuntu16.04安装vscode
1. 从[官网](https://code.visualstudio.com/docs/setup/linux)下载.deb安装文件
2. 在命令行中运行：`sudo apt install ***.deb`
3. 安装c/c++插件

## 网页ubuntu云桌面--Docker-ubuntu-vnc-desktop
1. 安装docker环境，参考[6]
2. 下载最新的ubuntu-desktop-lxde-vnc镜像
 ``` shell 
 docker pull dorowu/ubuntu-desktop-lxde-vnc
 ```



## 在Ubuntu中安装ROS kinetic教程
[在Ubuntu中安装ROS Kinetic](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu)
1. 添加sources.list

 配置你的电脑使其能够安装来自 packages.ros.org 的软件。ROS Kinetic 只支持Wily (Ubuntu 15.10), Xenial (Ubuntu 16.04) 和Jessie (Debian 8) 的debian包。
 ``` shell
 sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
 ```
 注：强烈建议使用国内或者新加坡的镜像源，这样能够大大提高安装下载速度。
 USTC (China)
 ```shell
 sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
 ```

2. 添加 keys
 为了从ROS存储库下载功能包，下面添加公钥
 ``` shell
 sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
 ```

3. 安装
 首先，确保你的Debian软件包索引是最新的
 ``` shell
 sudo apt-get update
 ```
 在ROS中，有很多不同的库和工具。我们提供了四种默认的配置来帮助你开始。你也可以单独安装ROS包。
 桌面完整版: (推荐) : 包含ROS、rqt、rviz、机器人通用库、2D/3D 模拟器、导航以及2D/3D感知
 ``` shell
 sudo apt-get install ros-kinetic-desktop-full
 ```
 ==ROS安装在/opt/ros/kinitic/目录中==，目录结构如下
 > * /bin        可执行二进制文件
 > * /etc        与ROS和catkin相关的配置文件
 > * /include    头文件
 > * /lib        库文件
 > * /share      ROS功能包
 > * env.*       配置文件
 > * setup.*     配置文件



 桌面版安装: 包含ROS、rqt、rviz以及通用机器人函数库。
 ``` shell
 sudo apt-get install ros-kinetic-desktop
 ```

 基础版安装: (简版) 包含ROS核心软件包、构建工具以及通信相关的程序库，无GUI工具。
 ``` shell
 sudo apt-get install ros-kinetic-ros-base
 ```

 单个软件包安装: 你也可以安装某个指定的ROS软件包（使用软件包名称替换掉下面的PACKAGE）:
 ``` shell
 sudo apt-get install ros-kinetic-PACKAGE
 ```
 例如：
 ``` shell
 sudo apt-get install ros-kinetic-slam-gmapping
 ```
 要查找可用软件包，请运行：
 ``` shell
 apt-cache search ros-kinetic
 ``` 

4. 初始化 rosdep
 在开始使用ROS之前你还需要初始化rosdep。rosdep可以方便在你需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。
 ``` shell
 sudo rosdep init
 rosdep update
 ```

5. 环境配置
 如果每次打开一个新的终端时ROS环境变量都能够自动配置好（即添加到bash会话中），那将会方便很多：
 ``` shell
 echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
 source ~/.bashrc
 ``` 
 如果你安装有多个ROS版本, ~/.bashrc 必须只能 source 你当前使用版本所对应的 setup.bash。

 如果你只想改变当前终端下的环境变量，可以执行以下命令：
 ``` shell
 source /opt/ros/kinetic/setup.bash
 ```
 如果你使用 zsh，替换其中的 bash， 你可以用以下命令来设置你的shell:
 ``` shell
 echo "source /opt/ros/kinetic/setup.zsh" >> ~/.zshrc
 source ~/.zshrc
 ```
6. 构建工厂依赖
 到目前为止，你已经安装了运行核心ROS包所需的内容。为了创建和管理自己的ROS工作区，有各种各样的工具和需求分别分布。例如：rosinstall是一个经常使用的命令行工具，它使你能够轻松地从一个命令下载许多ROS包的源树。

 要安装这个工具和其他构建ROS包的依赖项，请运行:
 ``` shell
 sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
 ```
7. 创建并初始化工作目录
``` shell
mkdir -p ~/catkin_ws/src
cd ~/catkin/src
catkin_init_workspace # 生成CMakeLists.txt文件
```
编译生成build和devel目录，catkin的构建系统的相关文件保存在build目录中，构建后的可执行文件保存在devel目录中。
``` shell
cd ~/catkin_ws/
catkin_make 
```
``` shell
打开`~/.bashrc`文件就可以看到已经有了很多设置。不要修改以前的设置，而是到
bashrc文件的最底部添加以下内容（xxx.xxx.xxx.xxx是用户自己的IP地址, 如果用户在一台PC上运行所有ROS功能包，则可以指定localhost而不是指定特定的IP。)
# Set ROS Kinetic
source/opt/ros/kinetic/setup.bash
source~/catkin_ws/devel/setup.bash
# Set ROS Network
export ROS_HOSTNAME=xxx.xxx.xxx.xxx
export ROS_MASTER_URI=http://${ROS_HOSTNAME}:11311
# Set ROS alias command
alias cw='cd~/catkin_ws'
alias cs='cd~/catkin_ws/src'
alias cm='cd ~/catkin_ws && catkin_make'
```
为了让修改了的bashrc文件发挥作用，输入如下命令。或者，如果用户关闭当前正在
运行的终端窗口并运行新的终端窗口，用户也将得到相同的效果，因为用户在bashrc中
所做的设置会适用于新的终端窗口。
`source~/.bashrc`

8. 安装qt和ros_qtc_plugin
``` shell
sudo apt-get install qtcreator
```
[ros_qtc_plugin安装教程](https://ros-qtc-plugin.readthedocs.io/en/latest/_source/How-to-Install-Users.html#)
安装完成后通过`qtcreator-ros`启动qt



9. 安装turtlebot3
``` shell
cd src
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
cd .. && catkin_make
```

### 主节点(master)
#### 功能：负责节点之间的连接和消息通信
主节点使用XML远程过程调用（XMLRPC，XML-Remote Procedure Call）与节点进行通信。XMLRPC是一种基于HTTP的协议，主节点不与连接到主节点的节点保持连接。换句话说，节点只有在需要注册自己的信息或向其他节点发送请求信息时才能访问主节点并获取信息。通常情况下，不检查彼此的连接状态。由于这些特点，ROS可用于非常大而复杂的环境。XMLRPC也非常轻便，支持多种编程语言，使其非常适合支持各种硬件和语言的ROS。
#### 运行指令： roscore
### 节点(node)
#### 功能：ROS中运行的最小处理器单元，可视为一个可执行程序。在ROS中，建议为一个目的创建一个节点。节点在运行时向主节点注册节点名称、发布者(publisher)名称、订阅者(subscriber)名称、服务服务器(service server)名称、服务客户端(service client)名称，且注册消息形式、URI地址和端口。
节点运行需要向主节点注册名称：
* 节点名称
* 发布者(publisher)名称
* 订阅者(subsriber)名称
* 服务服务器(service server)名称
* 服务客户端(service client)名称
* 消息形式
* URI地址和端口
#### 节点间通信：
* 节点与主节点之间通信通过XMLRPC。
* 节点之间的连接请求和响应使用XMLRPC，而消息通信使用TCPROS。
### 功能包(package)
功能包是构成ROS的基本单元。ROS应用程序是以功能包为单位开发的，功能包包括至少一个以上的节点或拥有用于运行其他功能包的节点的配置文件。它还包含功能包所需的所有文件，如用于运行各种进程的ROS依赖库、数据集合配置文件等。
元功能包(metapackage)是一个具有共同目的的功能包的集合。比如，导航元功能包包含AMCL、DWA、EKF和map_server等10多个功能包。
### 消息
#### 消息通信方法：TCPROS和UDPROS等
#### 消息分类：
* 单向消息发送/接收方式的话题(topic)
 * * 
* 双向消息请求(request)/响应(response)方式的服务(servive)

#### 常见消息格式
* [std_msgs/Header.mgs](http://docs.ros.org/api/std_msgs/html/msg/Header.html)
``` msg
# Standard metadata for higher-level stamped data types.
# This is generally used to communicate timestamped data 
# in a particular coordinate frame.
# 
# sequence ID: consecutively increasing ID 
uint32 seq
#Two-integer timestamp that is expressed as:
# * stamp.sec: seconds (stamp_secs) since epoch (in Python the variable is called 'secs')
# * stamp.nsec: nanoseconds since stamp_secs (in Python the variable is called 'nsecs')
# time-handling sugar is provided by the client library
time stamp
#Frame this data is associated with
string frame_id
```
* [std_msgs/Float64.msg](http://docs.ros.org/api/std_msgs/html/msg/Float64.html)
``` msg
float64 data
```
* [geometery_msgs/Point.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Point.html)
``` msg
# This contains the position of a point in free space.

float64 x
float64 y
float64 z
```
* [geometry_msgs/Quaternion.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Quaternion.html)
``` msg
# This represents an orientation in free space in quaternion form.

float64 x
float64 y
float64 z
float64 w
```
* [geometry_msgs/Pose.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Pose.html)
``` msg
# A representation of pose in free space, composed of position and origentation.

Point position
Quaternion orientation
```
* [geometry_msgs/Twist.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Twist.html)
``` msg
# This expresses velocity in free sapce broken into its linear and angular parts.

geometry_msgs/Vector3 linear
geometry_msgs/Vector3 angular
```
* [geometry_msgs/Vector3.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Vector3.html)
``` msg
# This represents a vector in free sapce.
# It is only meant to represent a direction. Therefore, it does not
# make sense to apply a translation to it (e.g. when applying a generic 
# rigid transformation to a Vector3, tf2 will only apply the rotation).
# If you want your data to be translatable too, use the 
# geometry_msgs/Point message instead.

float64 x
float64 y
float64 z
```
* [geometry_msgs/PoseArray.msg](http://docs.ros.org/api/geometry_msgs/html/msg/PoseArray.html)
``` msg
# An array of pose with a header for global reference
Header header
Pose[] poses
```

* [geometry_msgs/PoseWithCovariance.msg](http://docs.ros.org/api/geometry_msgs/html/msg/PoseWithCovariance.html)
``` msg
# This represents a pose in free space with uncertainty.

Pose pose

# Row-major representation fo the 6x6 covariance matrix.
# The orientation parameters use a fixed-axis representation.
# In order, th parameters are:
# (x, y, z, rotation about X axis, rotation about Y axis, rotation about Z axis)
float64[36] covariance
```
* [geometry_msgs/TwistWithCovariance.msg](http://docs.ros.org/api/geometry_msgs/html/msg/TwistWithCovariance.html)
``` msg
# This expresses velocity in free space with uncertainty.

geometry_msgs/Twist twist

# Row-major representation of the 6x6 covariance matrix
# The orientation parameters use a fixed-axis representation.
# In order, the parameters are:
# (x, y, z, rotation about X axis, rotation about Y axis, rotation about Z axis)
float64[36] covariance
```
* [geometry_msgs/Transform.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Transform.html)
``` msg
# This represents the transform between two coordinate frames in free space.
Vector3 translation
Quaternion rotation
```
* [geometry_msgs/TransformStamped.msg](http://docs.ros.org/api/geometry_msgs/html/msg/TransformStamped.html)
``` msg
# This expresses a transform from coordinate frame header.frame_id
# to the coordiate frame child_frame_id

# This message is mostly used by tf package.
# See its documentation for more information

std_msgs/Header header
string child_frame_id  # the frame id of the child frame
geometry_msgs/Transform transform

```
* [geometry_msgs/Wrench.msg](http://docs.ros.org/api/geometry_msgs/html/msg/Wrench.html)
```
# This represents force in free space, separated into 
# its linear and angular parts.
Vector3 force
Vector3 torque
```
* [nav_msgs/Odometry](http://docs.ros.org/api/nav_msgs/html/msg/Odometry.html)
```
# This represents an estimate of a position and velocity in free space.
# The pose in this message should be specified in the coordinate frame given by header.frame_id.
# The twist in this message should be specified in the coordinate frame given by the child_frame_id
Header header
string child_frame_id
geometry_msgs/PoseWithCovariance pose
geometry_msgs/TwistWithCovariance twist

```

* [tf2_msgs/TFMessage.msg](http://docs.ros.org/api/tf2_msgs/html/msg/TFMessage.html)
``` msg
geometry_msgs/TransformStamped[] transforms
```
* [sensor_msgs/JointState.msg](http://docs.ros.org/api/sensor_msgs/html/msg/JointState.html)
``` msg
# This is a message that holds data to describ the state of a set of torque controlled joints.

# The state of each joint (revolute or prismatic) is defined by:
#  * the position of the joint (rad or m),
#  * the velocity of the joint (rad/s or m/s),
#  * the effort that is applied in the joint (Nm or N).

# Each joint is uniquely identified by its name.
# The header specifies the time at which the joint states were recorded. All the joint states 
# in one message have to be recorded at the same time.
# The message consists of a multiple arrays, one for each part of the joint state.
# The goal is to make each of the fields optional. When e.g. your joints have no 
# effort associated with them, you can leave the effort array empty.
# All arrays in this message should have the same size, or be empty.
# This is the only way to uniquely associate the joint name with the correct states.

std_msgs/Header header
string[] name
float64[] position
float64[] velocity
float64[] effort
```
* [sensor_msgs/LaserScan.msg](http://docs.ros.org/api/sensor_msgs/html/msg/LaserScan.html)
``` msg
# Single scan from a planar laser range_finder
#
# If you have another ranging device with different behavior (e.g. a sonar array),
# please find or create a different message, since applications will make fairly 
# laser-specific assumptions about this data

Header header         # timestamp in the header is  the acquisition time of 
                      # the first ray in the scan.
                      # In frame frame_id, angles are measured around
                      # the positive Z axis (counterclockwise, if Z is up)
                      # with zero angle being forward along the x axis.

float32 angle_min     # start angle of the scan [rad]
float32 angle_max     # end angle of the scan [rad]
float32 angle_increment # angular distance between measurements [rad]

float32 time_increment # time between measurements [seconds] - if your scanner
                       # is moving, this will be used in interpolating position of 3d points
float32 scan_time      # time between scans [seconds]

float32 range_min      # minimum range value [m]
float32 range_max      # maximum range value [m]

float32[] ranges       # range data [m] (Note: values < range_min or > range_max should be discarded)
float32[] intensities  # intensity data [device-specific units]. If your device does not provide intensities, 
                       # please leave the array empty.
```
* [sensor_msgs/Imu](http://docs.ros.org/api/sensor_msgs/html/msg/Imu.html)
```
# This is a message to hold data from an IMU (Inertial Measurement Unit)
#
# Accelerations should be in m/s^2 (not in g's), and rotational velocity should be in rad/sec
#
# If the covariance of the measurement is know, it should be filled in (if all you know is the 
# variance of each measurement, e.g. from the datasheet, just put those along the diagonal)
# A covariance matrix of all zeros will be interpreted as "covariance unknown", and to use the 
# data a covariance will have to be assumed or gotten from some other source
#
# If you have no estimate for one of the data elements (e.g. your IMU doesn't produce an orientation
# estimate), please set element 0 of the associated covariance matrix to -1
# If you are interpreting this message, please check for a value of -1 in the first element of each 
# covariance matrix, and disregard the associated estimate.
Header header
geometry_msgs/Quanternion orientation
float64[9] orientation_covariance # Row major about x, y, z axes
geometry_msgs/Vector3 angular_velocity
float64[9] angular_velocity_covariance # Row major about x, y, z axes
geometry_msgs/Vector3 linear_acceleratin
float64[9] linear_acceleration_covariance # Row major about x, y, z axes
```
* [nav_msgs/Odometry](http://docs.ros.org/api/nav_msgs/html/msg/Odometry.html)
``` msg
# This represents an estimate of a position and velocity in free space.
# The pose in this message should be specified in the coordinate frame given by header.frame_id.
# The twist in this message should be specified in the coordinate frame given by the child_frame_id.

std_msgs/Header header
string child_frame_id
geometry_msgs/PoseWithCovariance pose
geometry_msgs/TwistWithCovariance twist
```
####[Gazebo ROS API](http://wiki.ros.org/gazebo)
1. Gazebo Subsribed Topics
/Gazebo/set_link_state [gazebo_msgs/LinkState](http://docs.ros.org/api/gazebo_msgs/html/msg/LinkState.html)
/Gazebo/set_model_state [gazebo_msgs/ModelState](http://docs.ros.org/api/gazebo_msgs/html/msg/ModelState.html)
2. Gazebo Published Topics
/clock [rosgraph_msgs/Clock](http://docs.ros.org/api/rosgraph_msgs/html/msg/Clock.html)
/Gazebo/link_states [gazebo_msgs/LinkStates](http://docs.ros.org/api/gazebo_msgs/html/msg/LinkStates.html)
/Gazebo/model_states [gazebo_msgs/ModelStates](http://docs.ros.org/api/gazebo_msgs/html/msg/ModelStates.html)
3. Gazebo Service
3.1 Create and destroy models in simulation
3.2 State and properties getters
3.3 State and properties setters
3.4 Simulation control
    /gazebo/pause_physics [std_srvs/Empty](http://docs.ros.org/api/std_srvs/html/srv/Empty.html)
    /gazebo/unpause_physics [std_srvs/Empty](http://docs.ros.org/api/std_srvs/html/srv/Empty.html)
3.5 Force control

## [CMakeList.txt语法介绍](https://blog.csdn.net/afei__/article/details/81201039)
1. 指定cmake的最小版本
`cmake_minimum_required(VERSION 3.4.1)`
2. 设置项目名称
`project(name)`
这个命令不是强制性的，但最后加上，他会引入两个变量demo_BINARY_DIR和demo_SOURCE_DIR，同时cmake定义两个等价的变量PROJECT_BINARY和PROJECT_SOURCE_DIR。
3. 设置编译类型
``` shell
add_executable(demo demo.cpp) # 生成可执行文件
add_library(common STATIC util.cpp) # 生成静态库
add_library(common SHARED util.cpp) # 生成动态库或共享库
```
add_library默认生成是静态库，同过上面的命令生成的文件名字
* 在Linux下为：
   >demo
   >libcommon.a
   >libcommon.so
* 在Windows下为：
   >demo.exe
   >common.lib
   >common.dll

4. 指定编译包含的源文件

    4.1 明确指定包含哪些文件
    `add_library(demo demo.cpp test.cpp util.cpp)`

    4.2 搜索所有的cpp文件
    `aux_source_directory(dir VAR)`发现一个目录下所有的源码文件并将列表存储在一个变量中，
    ``` cmake
    aux_source_directory(. SRC_LIST) # 搜索当前目录下所有.cpp文件
    add_library(demo ${SRC_LIST})
    ```
    4.3 自定义搜索规则
    ``` cmake
    file(GLOB SRC_LIST "*.cpp" "protocol/*.cpp")
    add_library(demo ${SRC_LIST})
    ```
    或者
    ``` cmake
    file(GLOB SRC_LIST "*.cpp")
    file(GLOB SRC_PROTOCOL_LIST "protocol/*.cpp")
    add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
    ```
    或者
    ``` cmake
    aux_source_directory(. SRC_LIST)
    aux_source_directory(protocol SRC_PROTOCOL_LIST)
    add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
    ```
5. 查找指定的库文件
`find_library(VAR name path)`查找到指定的预编译库，并将它的路径存储在变量中。默认的搜索路径为cmake包含的系统库，因此如果是NDK的公共库只需要指定库的name即可。
``` cmake
find_library(
    # Sets the name of the path variable
    log_lib

    # Specifies the name of the NDK library that 
    # you want CMake to locate.
    log
)
```
类似的命令还有find_file(), find_path(), find_program(), find_package()。
6. 设置包含的目录
``` cmake
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```
Linux下还可以通过如下方式设置包含的目录
`set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I${CMAKE_CURRENT_SOURCE_DIR})`

7. 设置连接库目录
``` cmake
link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/libs
)
```
Linux下还可以通过如下方式设置连接库目录
`set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR}/libs)`

8. 设置target需要连接的库

    在 Windows 下，系统会根据链接库目录，搜索xxx.lib 文件，Linux 下会搜索 xxx.so 或者 xxx.a 文件，如果都存在会优先链接动态库（so后缀）。   
    ``` cmake
    target_link_libraries(
        # 目标库
        demo
        # 目标库需要连接的库
        # log-lib是上面find_library指定的变量名
        ${log-lib}
    )
    ```
    8.1 指定连接动态库或者静态库

    ``` cmake
    target_link_directories(demo libface.a)  # 
    target_link_directories(demo libface.so)
    ```
    8.2 指定全路径
    ``` cmake
    target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a)
    target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.so)
    ``` 
    8.3 指定连接多个库
    target_link_libraries(demo
        ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a
        boost_system.a
        boost_thread
        pthread
    )
9. 设置变量
    9.1 set直接设置变量的值
    ``` cmake
    set(SRC_LIST main.cpp test.cpp)
    add_executable(demo ${SRC_LIST})
    ```
    9.2 set追加设置变量的值
    ``` cmake
    set(SRC_LIST main.cpp)
    set(SRC_LIST ${SRC_LIST} test.cpp)
    add_executable(demo ${SRC_LIST})
    ```
    9.3 list追加或删除变量的值
    ``` cmake
    set(SRC_LIST main.cpp)
    list(APPEND SRC_LIST test.cpp)
    list(REMOVE_ITEM SRC_LIST main.cpp)
    add_executable(demo ${SRC_LIST})
    ```
10. 常用变量
    10.1 预定义变量
    PROJECT_SOURCE_DIR：工程的根目录
    PROJECT_BINARY_DIR：运行cmake命令的目录，通常是${PROJECT_SOURCE_DIR}/build
    PROJECT-NAME：返回通过project命令定义的项目名称
    CMAKE_CURRENT_SOURCE_DIR：当前处理的CMakeLists.txt
    CMAKE_CURRENT_BINARY_DIR：target编译目录
    CMAKE_CURRENT_LIST_DIR：CMakeLists.txt的完整路径
    CMAKE_CURRENT_LIST_LINE：当前所在的行
    CMAKE_MODULE_PATH：定义自己的cmake模块所在的路径，SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)，然后可以用INCLUDE命令来调用自己的模块。
    EXECUTABLE_OUTPUT_PATH：重新定义目标二进制可执行文件的存放位置
    LIBRARY_OUTPUT_PATH：重新定义目标链接库文件的存放位置


## [catkin_make配置CMakeList.txt](http://wiki.ros.org/catkin/CMakeLists.txt)
1. `cmake_minimum_required(VERSION 2.8.3)`
2. `project(ros_tutorials_topic)`
3. `find_package()`
找到build时依赖的其他CMake包，并为找到的包创建CMake环境变量，这些环境变量描述了这些包导出的头文件、源文件和依赖的库的位置，可以在后面的CMake脚本中使用。这些环境变量命名规则为`<PACKAGE_NAME>_<PROPERTY>`:
* `<PACKAGE_NAME>_FOUND`：如果包/库找到了，则设置为true，否则设置为false
* `<PACKAGE_NAME>_INCLUDE_DIRS`或者`<PACKAGE_NAME>_INCLUDES`：包输出的的include路径
* `<PACKAGE_NAME>_LIBRARIES`或者`<PACKAGE_NAME>_LIBS`：包输出的libraries
* `<PACKAGE_NAME>_`
``` cmake
find_package(catkin REQUIRED COMPONENTS 
    message_generation
    ros_cpp
    std_msgs)
```


4. messages, services, and action
Messages (.msg), services (.srv), and actions (.action) files in ROS require a special preprocessor build step before being built and used by ROS packages. The point of these macros is to generate programming language-specific files so that one can utilize messages, services, and actions in their programming language of choice. The build system will generate bindings using all available generators (e.g. gencpp, genpy, genlisp, etc).
处理messages, services, 和action的三个宏函数为
* add_message_files()
* add_service_files()
* add_action_files()
``` cmake
add_message_files(
    FILES MsgTutorial.msg
)
```
这些宏函数后面紧跟着generate_messages()。

generate added messages and services with any dependencies listed here
``` cmake
generate_messages(
    DEPENDENCIES
    std_msgs
)
```
5. catkin specific configuration
**INCLUDE_DIRS**: your package contains header files
**LIBRARIES**: libraries you create in this project that dependent projects also need
**CATKIN_DEPENDS**: catkin_packages dependent projects also need
**DEPENDS**: non-catkin CMake projects that this project depends on
**CFG_EXTRAS**：additional configuration options
 message_generation 
``` cmake
catkin_package(
    LIBRARIES ros_tutorial_topic
    CATKIN_DEPENDS message_generation roscpp std_message
)
```
6. include paths and library paths
`include_directories(${catkin_INCLUDE_DIRS})`: where can header files be found for the code (most common in C/C++) being build.
`link_directories()`: where are libraries located that executable target build against
7. executable targets, library targets and target_link_libraries
`add_executable()`：指明会build一个可执行目标
`add_library(${PROJECT_NAME} ${${PROJECT_NAME}_SRC})`：指明哪些库加入到build
`target_link_libraries()`：指明可执行目标连接依赖的库，通常放在add_executable后面
``` cmake
add_executable(topic_publisher src/topic_publisher.cpp)
add_dependencies(topic_publisher ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_publisher ${catkin_LIBRARIES})
```
8. install
If you want to install targets to the system instead of the devel space of catkin workspace, you can do a "make install" of your code, so that it can be used by others or to a local folder to test a system-level installation.
This is done using the CMake install() function which takes as arguments:
* TARGETS - which targets to install
* ARCHIVE DESTINATION - static libraries and DLL .lib stubs
* LIBRARY DESTINATION - non-DLL shared libraries and modules
* RUNTIME DESTINATION - Executable targets and DLL style shared libraries
``` cmake
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
```
Besides these standard destination some files must be installed to special folders, i.e. a library containing Python bindings must be installed to a different folder to be importable in Python:
``` cmake
install(TARGETS python_module_library
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_PYTHON_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_PYTHON_DESTINATION})
```
8.1 Installing Python Excutable Scripts
For Python code, the install rule looks different as there is no use of the add_library() and add_executable() functions so as for CMake to determine which files are targets and what type of targets they are. Instead, use the following in your CMakeLists.txt file:
``` cmake
catkin_install_python(PROGRAMS scripts/myscript
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
```
8.2 installing header files
``` cmake
install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
    PATTERN ".svn" EXCLUDE)
```
8.3 Installing roslaunch files or other Resources
``` cmake
install(DIRECTORY launch/
    DESTINATION ${CATKIN_GLOBAL_SHARE_DESTINATION}/launch
    PATTERN ".svn" EXCLUDE
    )
```
## [catkin_make](http://wiki.ros.org/catkin/commands/catkin_make)和[catkin_make_isolated](http://www.ros.org/reps/rep-0134.html)
You should always call `catkin_make` in the root of your catkin workspace, assuming your catkin workspace is in ~/catkin_ws:
``` shell
cd ~/catkin_ws
catkin_make
```
The above command will build any packages located in ~/catkin_ws/src. The equivalent commands to do this manually would be:
``` shell
cd ~/catkin_ws
cd src
catkin_init_workspace
cd ..
mkdir build
cd build
cmake ../src -DCMAKE_INSTALL_PREFIX=../install -DCATKIN_DEVEL_PREFIX=../devel
make
```
After running `catkin_make`, you should notice two new folders in the root of your catkin workspace: the *build* and *devel* folders.The *build* folder is where `cmake` and `make` are invoked, and the *devel* folder contains any generated files and targets, plus setup.*sh files so that you can use it like it is installed.
`catkin_make install` would generate *install* folder in the root of your workspace. This is a FHS compliant installatin of all of the packages in your catkin workspace. It contains setup.*sh files which can be sourced, allowing you to utilize the packages your built.
`catkin_make_isolated` builds each package in the workspace individually, calling cmake, make and possibly make install for each catkin package. This is opposed to how catkin workspaces are normally built, with one invocation of CMake which generates a single Make target for all packages in the workspace.
## [package文件](http://wiki.ros.org/catkin/package.xml)
1. 根标签`<package>`
``` xml
<package>
    ...
</package>

```
2. 必需的标签
* `<name>`：package的名字
* `<version>`：package版本号，由“.”分割的三个整数构成
* `<description>`：package内容描述
* `<maintainer>`：package的维护人
* `<license>`：软件许可证，如GPL，BSD，ASL
3. package依赖
* `<build_depend>`：指明package build时需要的依赖
* `<build_export_depend>`：指明根据package构建库时需要的依赖
* `<exec_depend>`：指明哪些依赖需要在package中运行代码
* `<test_depend>`：指明仅在单元测试时需要的额外依赖
* `<buildtool_depend>`：指明build package时需要哪些build系统工具，通常只需要caktin
* `<doc_depend>`：指明package生成文档需要的文档工具
4. metapackages
metapackage可以将多个packages组成一个逻辑上的package。metapackage也是一个标准的package，由`<export>`标签修饰
``` xml
<export>
    <metapackage />
</export>
```
除了必需的`<buildtool_depend>`依赖，metapackage仅可以有组合package的`<exec_depend>`依赖。


## launch文件
参考
[ROS探索总结（五十六）--launch文件](http://www.guyuehome.com/2195)
[ROS wiki roslaunch/XML](http://wiki.ros.org/roslaunch/XML)

1. `<launch>`
launch文件采用XML形式进行描述，包含一个根元素`<launch>`。
``` xml
<launch>
    ......
</launch>
```
2. `<node>`
launch文件的核心是启动ROS的节点，采用`<node>`标签定义，一个节点标签最常用的属性：
pkg="package_name"：表示节点所在的功能包名称；
type="executable_name"：表示节点的可执行文件名称；
name="node_name"：表示节点运行的名称，将覆盖节点中init()赋予的名称；
output="screen"：将节点的标准输出打印到终端屏幕，默认输出为日志文档；
respawn="true"：复位属性，当该节点停止时，会自动重启，默认为false；
required="true"：必要属性，当该节点终止时，launch文件中的其他节点也被终止；
ns="namespace"：命名空间属性，为节点内的相对名称添加命名空间前缀；
args="arguments"：节点需要的输入参数。

``` xml
<node pkg="package_name" type="executable_name" name="node_name" />
```
3. `<param>`
launch文件通过`<param>`加载parameter，launch文件执行后，parameter就加载到ROS的参数服务器上。每个活跃的节点都可以通过ros::param::get()接口获取parameter的值，用户也可以在终端中通过rosparam命令获得parameter的值。
``` xml
<param name="output_frame" value="odom" />
```
运行上面设置的launch文件后，output_frame的值就设为odom，并加载到ROS参数服务器上。如果需要设置的参数比较多，一个一个设置会非常麻烦，ROS提供了另一种方式加载参数--`<rosparam>`:
``` xml
<rosparam file=$"(find 2dnav_pr2)/config/costmap_common_params.yaml" command="load" ns="local_costmap" />
```
`<rosparam>`将一个yaml格式文件中的全部参数加载到ROS参数服务器中，需要设置“command”属性为“load”，还可以选择命名空间“ns”。

4. `<arg>`
相当于launch文件内部的局部变量，仅限于launch文件使用，便于launch文件的重构，和ROS节点内部的实现没有关系。
``` xml
<arg name="arg_name" default="arg_value" />
```
通过命令行给`<arg>`赋值，eg. `roslaunch ros_pkg.launch arg_name:=arv_value`

5. `<include>` 
`<include>` 标签导入其他roslaunch的XML文件到当前文件范围内，文件的所有内容除了`<master>`标签都将导入到文件内，`<include>`的属性包括：
`<file>`: 需要导入的文件名称，eg. `<include file="$(find pkg_name)/path/file_name.xml" />`
子标签包括：`<env>` 和`<arg>` 标签，分别给整个导入文件设置一个环境变量和给导入文件传递参数。

6. `<group>`
`<group>`标签给节点进行分组，每个组有自己独立的命名空间。`<group>`标签等价于顶层的`<launch>`标签，类似于一个标签容器，也是就是可以在`<group>`内使用任何标签，就像在`<launch>`标签内。

7. `<remap>`
在启动ROS节点时，`<remap>`标签将名称映射参数传递给ROS节点，这种方式比直接设置节点参数属性更规范。`<remap>`标签可以在`<launch>` `<node>` `<group>`中应用。
`<remap from="original-name to="new-name" />`

## URDF建模
[ROS urdf/Tutorials](http://wiki.ros.org/urdf/Tutorials)
[ROS URDF/XML](http://wiki.ros.org/urdf/XML)
[URDF文件解析](https://blog.csdn.net/Kalenee/article/details/86485565)
URDF是以`<robot>`标签来开始，详细内容中通常会反复交替出现`<link>`标签和`<joint>`标签，这两种标签分别用来定义机器人的组件：连杆和关节。其中，为了与ROS-Control共用，通常还包括用于设置关节和电机之间的关系的`<transmission>`标签。
1. `<link>`标签的属性
*  `<link>`    设置栏杆的可视化、碰撞和惯性信息
    * `<inertial>`  （可选）设置连杆惯性信息
        * `<origin>`   设置惯性参考系相对于连杆参考系的位姿，惯性参考系的原点在重心处。
            * xyz 表示x，y，z方向的偏置，默认为0。
            * rpy 表示横滚角、俯仰角、航向角，单位（rad）。
        * `<mass>`     设置连杆的质量（单位：kg）
        * `<inertia>`  设置惯性张量，惯性张量为3x3的对称矩阵，通过上三角形元素ixx，ixy，ixz，iyy，iyz，izz来表示。
    * `<visual>`    设置连杆可视化信息
        * `<origin>`   设置可视元素的参考系相对于连杆参考系的位姿。
            * xyz 表示x，y，z方向的偏置，默认为0。
            * rpy 表示横滚角、俯仰角、航向角，单位（rad）。
        * `<geometry>` 输入模型的形状，如
            * box 通过size设置立方体的三边长度，立方体的原点在中心。
            * cylinder 通过radius和length设置圆柱体的半径和高度，圆柱体的原点在中心。
            * sphere 通过radius设置球的半径，球的原点在中心。
            * mesh 通过filename获取网格文件，文件格式推荐使用COLLADA(.dae)，STL(.stl)格式也可以使用，通过scale设置缩放比例。
    * `<collision>` 设置碰撞计算的信息，允许输入标量连杆外形范围的几何信息
* `<mass>`     设置连杆的质量（单位：kg）
* `<inertia>`  设置惯性张量
* `<origin>`   设置相对与连杆相对坐标系的移动和旋转
* `<geometry>` 输入模型的形状，如box、cylinder、sphere，也可以导入COLLADA(.dae)、STL(.stl)格式的设计文件
* `<material>` 描述连杆颜色和纹理等信息，其中颜色使用`<color>`标签，eg. `<color rgba="0.0 0.0 0.0 1.0"/>`，其中rgba输入的四个数字分别表示红色、绿色、蓝色和透明度。
2.  `<joint>`标签属性
* `<joint>`    设置与连杆的关系和关节类型
* `<paremnt>`  关节的父连杆
* `<child>`    关节的子连杆
* `<origin>`   将父连杆的坐标系转化为子连杆的坐标系
* `<axis>`     设置旋转轴
* `<limit>`    设置关节的速度(单位rad/s)、力(单位N)和半径(仅当关节是revolute或prismatic时)
3.  
* `<gazebo>`      描述仿真属性，比如阻尼、摩擦等
* `<model>`       描述机器人结构的运动学和动力学属性
* `<model_state>` 描述在某一时间的model状态
* `<sensor>`      描述传感器，比如相机、激光雷达

## xacro
[xacro](http://wiki.ros.org/xacro)是一种XML的宏语言，通过xacro可以构建更短更易读的XML文件
### [check xacro syntax](https://answers.ros.org/question/9208/how-to-check-xacro-syntax-like-check_urdf/)
``` shell
xacro --inorder file.acro | check_urdf -
```
``` shell
xacro --inorder file.acro > temp.urdf && check_urdf temp.urdf && rm temp.urdf
```
``` shell
check_urdf < (xacro --inorder file.acro)
```

## [Parameter Server](http://wiki.ros.org/Parameter%20Server)
参数服务器存储全局可见的系统状态配置参数，方便节点在运行时存储和配置所需参数。参数服务器使用XMLRPC实现，运行在Master节点中，通过XMLRPC库获取参数调用API。
1. parameters
参数使用ROS的标准命名规则，采用层级结构匹配话题和节点的命名空间，来避免参数名称冲突。层级结构使参数可以单独获取，比如
``` xmlrpc
/camera/left/name : leftcamera
/camera/left/exposure: 1
/camera/right/name: rightcamera
/camera/right/exposure: 1.1
```
上面参数camera是一个字典
`{left: {name: leftcamera, exposure: 1}, right: {name: rightcamera, exposure: 1}}`
2. parameter types
参数服务器使用xmlrpc数据类型，包括
```
32-bit integers
boolens
strings
doubles
iso8601 dates
lists
base64-encoded binary data
```
当然可以在参数服务器上存储`directory`，参数服务器将ROS的命名空间(namespace)表示成字典。
```
/gains/P = 10.0
/gains/I = 1.0
/gains/D = 0.1
```
对于上面参数，如果检索/gains/P，返回10.0，如果检索/gains，则返回一个字典
`{'P': 10.0, 'I': 1.0, 'D': 0.1}`
3. [rosparam](http://wiki.ros.org/rosparam)
rosparam命令行工具使用YAML文件获取和设置参数服务器上的参数，rosparam也可以在launch文件中被调用。
支持的命令如下：
```
rosparam set    set parameter
rosparam get    get parameter
rosparam load   load parameters from file
rosparam dump   dump parameters to file
rosparam delete delete parameter
rosparam list   list parameter names
```
4. [roscpp's parameter API](http://wiki.ros.org/roscpp/Overview/Parameter%20Server)
roscpp有两种不同的参数API：“bare”版用于ros::param命名空间，“handle”版通过[ros::NodeHandle](http://wiki.ros.org/roscpp/Overview/NodeHandles)调用。
4.1 Getting Parameters
`ros::param::get()`
`ros::NodeHandle::getParam()`

4.2 Setting Parameters
`ros::param::set()`
`ros::NodeHandle::setParam()`
5. [rospy's parameter API](http://wiki.ros.org/rospy/Overview/Parameter%20Server)
`rospy.get_param()`
`rospy.set_param()`
## [Index of ROS Enhancement Proposals (REP)](http://www.ros.org/reps/rep-0000.html)
### [Standard Units of Measure and Coordinate Conventions](http://www.ros.org/reps/rep-0103.html)
### [Coordinate Frames for Mobile Platforms](http://www.ros.org/reps/rep-0105.html)
**base_link**
`base_link`坐标系固定在机器人移动基座上，可以放置在任意位姿，不同硬件平台可以放置基座的不同位置，提供一个显而易见的参考点。
**odom**
`odom`坐标系是一个世界固定坐标系。移动平台在`odom`坐标系的位置可能随时间漂移而没有边界。这种漂移使得`odom`坐标系不能长期作为全局参考系。然而，机器在`odom`坐标系中的位姿是连续的，没有离散的跳跃。`odom`坐标系作为短期的精确的局部参考非常有用。通常基于一个里程计源建立`odom`坐标系，比如车轮里程计、视觉里程计、IMU等。
`map`坐标系是一个世界固定坐标系，z轴指向上。移动机器人相对`map`坐标系的位姿没有随时间明显漂移。`map`坐标系不是连续的，意味着移动机器人在`map`坐标系中的位姿可能存在离散的跳跃。`map`坐标系作为长期的全局参考非常有用，但是位置估计器的离散跳动不适合局部感知和行动
**earth**
`earth`坐标系位于ECEF原点，这个坐标系允许在不同的`map`坐标系的多机器人交互。如果实际中只有一个`map`坐标系，就不需要`earth`坐标。
各坐标树状关系：`earth`——>`map`——>`odom`——>`base_link`

## [roscpp](http://wiki.ros.org/roscpp/Tutorials)
节点初始化有两个过程
(1)通过调用ros::init()函数初始化节点，这种方式提供ROS命令行参数声明节点名称和其他选项。
`void ros::init(<command line or remapping arguments>, std::string node_name, uint32_t options);`
(2)通过创建ros::NodeHandle启动节点，当然也有其他方法启动节点。

### [NodeHandles](http://wiki.ros.org/roscpp/Overview/NodeHandles)
ros::NodeHandle类有两个功能：(1)启动和关闭程序内部的节点，(2)提供命名空间外层方法。
1. 节点启动与关闭
`ros::NodeHandle nh;`
节点创建时，如果内部节点还没有启动，ros::NodeHandle会启动节点，一旦所有ros::NodeHandle的实例被注销，节点将自动关闭。
2. 命名空间
`ros::NodeHandle nh("my_namespace");`
这使得与该节点句柄一起使用的任何相对名称相对于`<node_namespace>/my_namespace`，而不仅仅是`<node_namespace>`。
声明父节点句柄和相对命名空间
```
ros::NodeHandle nh1("ns1");
ros::NodeHandle nh2(nh1, "ns2");
```
上面语句的节点句柄nh2的命名空间`<node_namespace>/ns1/ns2`。
**全局名称**
`ros::NodeHandle nh("/my_gloabl_namespace");`
**私有名称**
``` c++
ros::NodeHandle nh("~my_private_namespace");
ros::Subscriber sub = nh.subscribe("my_private_topic", ...);
```
上面例子将订阅`<node_name>/my_private_namespace/my_private_topic`。

### [Using Parameters in roscpp](http://wiki.ros.org/roscpp_tutorials/Tutorials/Parameters)
#### 1. 获取参数
1.1 `getParam()`
```
 std::string s;
 n.getParam("my_param", s);
 ```
 其中，n是一个ros::NodeHandle的实例，上面例子中如果获取参数"my_param"成功，getParam()返回true并将参数赋值给变量s，否则，getParam()返回false。
1.2 `param()`
```
 int i;
 n.param("my_param", i, 42);
 ```
 param()与getParam()相似，但在不能成功获取参数时，会声明一个默认值。
#### 2. 设置参数
`n.setParam("my_param", "hello there");`
#### 3. 删除参数
`n.deleteParam("my_param");`
#### 4. 查验参数是否存在
`n.hasParam("my_param");`
## [rospy](http://wiki.ros.org/rospy/Tutorials)
### [rospy API index](http://docs.ros.org/api/rospy/html/identifier-index.html)
### [Creating a ROS Package](http://wiki.ros.org/ROS/Tutorials/CreatingPackage)
### [Writing a Simple Publisher and Subscriber](http://wiki.ros.org/rospy_tutorials/Tutorials/WritingPublisherSubscriber)
1. Writing the Publisher Node
``` shell
roscd beginner_tutorials # 进入包beginner_tutorials目录，在[Creating a ROS Package](http://wiki.ros.org/ROS/Tutorials/CreatingPackage)教程中创建
mkdir scripts # 创建scripts目录用来存储python代码
cd scripts
wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/talker.py # 下载实例代码
chmod +x talker.py # 赋予代码可执行权
```
`rosed beginner_tutorials talker.py`打开代码文件，内容如下：
``` python
#!/usr/bin/env python ## 第一行声明确保脚本以python脚本执行
# license removed for brevity
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10) # 声明节点发布的话题名称“chatter”，类型为std_msgs.msg.String，
    rospy.init_node('talker', anonymous=True)               # 初始化节点名称，才能启动与master节点的通信。更多内容参考[Initialization and Shutdown - Initializing your ROS Node](http://wiki.ros.org/rospy/Overview/Initialization%20and%20Shutdown#Initializing_your_ROS_Node)
    rate = rospy.Rate(10) # 10hz                            # 创建Rate对象速率，配合sleep()实现期望的速率执行循环。
    while not rospy.is_shutdown():                          # 标准的循环结构
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str) 
        pub.publish(hello_str)                              # 发布字符串“hello_str”到话题“chatter”
        rate.sleep()                                        # 进行足够长时间的休眠以实现期望的循环速率

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```
2. Writing the Subscriber Node
```
roscd beginner_tutorials/scripts/ # 进入包的scripts目录
wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py # 下载源码
```
打开源码文件
``` python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)
    
def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.
    rospy.init_node('listener', anonymous=True)   # 初始化节点名称“listener”

    rospy.Subscriber("chatter", String, callback) # 订阅话题“chatter”，类型为std_msgs.msg.String，当接收到新的消息，函数“callback”会被调用且新消息作为函数的第一参数。

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    listener()
```

### [Sevrices](http://wiki.ros.org/rospy/Overview/Services)
####1. Service definitions, request messages, and response message
ROS Services are defined by srv files, which contains a request message and a response message. There are identical to the messages used by ROS Topics. ros.py converts these srv files into Python source code and creates three classes that you need to be familiar with: service definitions, request messages, and response messages. The names of these classes come directly from the srv filename:
``` 
my_package/srv/Foo.srv ——> my_package.srv.Foo
my_package/srv/Foo.srv ——> my_package.srv.FooRequest
my_package/srv/Foo.srv ——> my_package.srv.FooResponse
```
**服务定义(Service Definitions)** 是一个包含请求和响应的数据类型的容器。在创建或调用服务时，需要导入服务定义并传递给服务初始化方法，
```
add_two_ints = rospy.ServiceProxy('service_name', my_package.srv.Foo)
```
**服务请求信息(Service Request Messages)** 用于调用相关服务。
**服务请求信息(Service Response Messages)** 用于接收相关服务返回值。
####2. Service proxies
可以通过想要调用的服务名称创建`rospy.ServiceProxy`来调用一个服务，通常会调用`rospy.wait_for_service()`进行阻断直到服务可用。如果服务返回错误，`rospy.ServiceException`会被触发。
``` 
rospy.wait_for_service('add_two_ints')
add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
try:
    resp1 = add_two_ints(x, y)
except rospy.ServiceException as exc:
    print("Service did not process request:" + str(exc))
```
其中，`rospy.ServiceProxy(name, service_class, persistent=False, headers=None)`创建了一个服务代理，`rospy.wait_for_service(service, timeout=None)`等待直到服务可用，如果设置了timeout值，则超过等待设定值会触发ROSException。
2.1 Calling services
`rospy.ServiceProxy`实例是可调用的，意味着你可以使用想要方法去调用它，
```
add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
add_two_ints(1, 2)
```

## tf
`tf`包允许用户随时追踪多个坐标系，并以树形结构表达各坐标系之间的关系，随时可以在任意两个坐标系之间变换点、矢量等。`tf::StampedTransform`和`geometry_msgs/TransformStamped`是相同的变换，都将数据从"child_frame_id"变换到"frame_id"。
### [Writing a tf broadcaster (C++)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28C%2B%2B%29)
创建包
``` shell
cd %YOUR_CATKIN_WORKSPACE_HOME%/src
catkin_create_pkg learning_tf tf roscpp rospy turtlesim  # 创建包learning_tf
cd %YOUR_CATKIN_WORKSPACE_HOME%/
catkin_make   # 编译包
source ./devel/setup.bash # 启动配置
roscd learning_tf && mkdir src && cd src
```
1. 代码
新建turtle_tf_broadcaster.cpp并复制下面代码
``` c++
#include <ros/ros.h>
#include <tf/transform_broadcaster.h>
#include <turtlesim/Pose.h>

std::string turtle_name;

void poseCallback(const turtlesim::PoseConstPtr& msg){
  // 创建TransformBroadcaster对象用于发送transformations
  static tf::TransformBroadcaster br;  
  // 创建Transform对象，并复制turtle的2D pose到3D transform
  tf::Transform transform;  
  transform.setOrigin( tf::Vector3(msg->x, msg->y, 0.0) );
  tf::Quaternion q;
  q.setRPY(0, 0, msg->theta);
  transform.setRotation(q);  
  // 通过TransformBroadcaster发送transform需要四个参数
  // 1. transform本身
  // 2. 发送transfrom的时间戳
  // 3. 创建的link的父坐标系名称，此处为“world”
  // 4. 创建的link的子坐标系名称，此处为turtle自己的名称
  br.sendTransform(tf::StampedTransform(transform, ros::Time::now(), "world", turtle_name));
  // 注意：senTransform和StampedTransform的父子顺序相反
}

int main(int argc, char** argv){
  ros::init(argc, argv, "my_tf_broadcaster");
  if (argc != 2){ROS_ERROR("need turtle name as argument"); return -1;};
  turtle_name = argv[1];

  ros::NodeHandle node;
  ros::Subscriber sub = node.subscribe(turtle_name+"/pose", 10, &poseCallback);

  ros::spin();
  return 0;
};
```
2. 运行broadcaster
完成代码后，在CMakeLists.txt文件底部加入下列内容：
```
add_executable(turtle_tf_broadcaster src/turtle_tf_broadcaster.cpp)
target_link_libraries(turtle_tf_broadcaster ${catkin_LIBRARIES})
```
编译包
```
cd %YOUR_CATKIN_WORKSPACE_HOME%
caktin_make
```
一切正常的话会在devel/lib/learning_tf目录下出现一个二进制文件turtle_tf_broadcaster。接下来创建一个launch文件start_demo.launch并复制下面的内容
``` xml
  <launch>
    <!-- Turtlesim Node-->
    <node pkg="turtlesim" type="turtlesim_node" name="sim"/>

    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>
    <!-- Axes -->
    <param name="scale_linear" value="2" type="double"/>
    <param name="scale_angular" value="2" type="double"/>

    <node pkg="learning_tf" type="turtle_tf_broadcaster"
          args="/turtle1" name="turtle1_tf_broadcaster" />
    <node pkg="learning_tf" type="turtle_tf_broadcaster"
          args="/turtle2" name="turtle2_tf_broadcaster" />

  </launch>
```
启动上面创建的turtle broadcaster例子
`roslaunch learning_tf start_demo.launch`
3. 检查运行结果
使用tf_echo工具检查是否turtle pose正在broadcast到tf
`rosrun tf tf_echo /world /turtle`

### [Writing a tf broadcaster(Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28Python%29)
创建包
``` shell
cd %YOUR_CATKIN_WORKSPACE_HOME%/src
catkin_create_pkg learning_tf tf roscpp rospy turtlesim  # 创建包learning_tf
cd %YOUR_CATKIN_WORKSPACE_HOME%/
catkin_make   # 编译包
source ./devel/setup.bash # 启动配置
roscd learning_tf && mkdir nodes && cd nodes
```
1. broadcast transforms
下面将说明如何实现广播坐标系到tf。在本实例中，随着turtle的运动，我们希望广播turtle变化的坐标系。首先在包中创建源文件turtle_tf_broadcaster.py，并复制下列代码
``` python
#!/usr/bin/env python  
import roslib
roslib.load_manifest('learning_tf')
import rospy

import tf
import turtlesim.msg

def handle_turtle_pose(msg, turtlename):
    br = tf.TransformBroadcaster()
    br.sendTransform((msg.x, msg.y, 0),
                     tf.transformations.quaternion_from_euler(0, 0, msg.theta),
                     rospy.Time.now(),
                     turtlename,
                     "world")
    # turtle pose消息的句柄函数广播它的平移和旋转，并作为从坐标系“world”到坐标系“turtleX”的transform进行发布。
if __name__ == '__main__':
    rospy.init_node('turtle_tf_broadcaster')
    turtlename = rospy.get_param('~turtle') 
    # 前缀[“~”](http://wiki.ros.org/Names)表示这是一个私有名称，这些私有名称主要用于特定的单个节点的参数，“~”将参数名转换到节点的命名空间。本行程序中指定一个turtle名称，比如，“turtle1”或者“turtle2”
    rospy.Subscriber('/%s/pose' % turtlename,
                     turtlesim.msg.Pose,
                     handle_turtle_pose,
                     turtlename)
    # 本节点订阅话题"/turtleX/posse"，接收到新消息后运行handle_turtle_pose函数，接收到的消息作为第一参数
    rospy.spin()
```
2. 运行broadcaster
创建一个新的文件launch/start_demo.launch，并增加下面代码
```xml
  <launch>
    <!-- Turtlesim Node-->
    <node pkg="turtlesim" type="turtlesim_node" name="sim"/>
    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>

    <node name="turtle1_tf_broadcaster" pkg="learning_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen" >
      <param name="turtle" type="string" value="turtle1" />
    </node>
    <node name="turtle2_tf_broadcaster" pkg="learning_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen" >
      <param name="turtle" type="string" value="turtle2" /> 
    </node>

  </launch>
```
启动turtle broadcaster
`roslaunch learning_tf start_demo.launch`
3. 检查运行结果
使用tf_echo工具检查是否turtle pose已经广播到tf
`rosrun tf tf_echo /world /turtle1`

### [Writing a tf listener (C++)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28C%2B%2B%29)
1. 代码
进入前面创建的包,
`roscd learning_tf`
创建源文件`src/turtle_tf_listener.cpp`并复制下面代码
``` c++
#include <ros/ros.h>
#include <tf/transform_listener.h> //提供TransformListener
#include <geometry_msgs/Twist.h>   //
#include <turtlesim/Spawn.h>

int main(int argc, char** argv){
  ros::init(argc, argv, "my_tf_listener");

  ros::NodeHandle node;

  ros::service::waitForService("spawn");
  ros::ServiceClient add_turtle = node.serviceClient<turtlesim::Spawn>("spawn");
  turtlesim::Spawn srv;
  add_turtle.call(srv);

  ros::Publisher turtle_vel = node.advertise<geometry_msgs::Twist>("turtle2/cmd_vel", 10);

  tf::TransformListener listener;  // 创建TransformListener对象，一旦listener被创建，它就开始接收tf变换。

  ros::Rate rate(10.0); // 10hz
  while (node.ok()){
    tf::StampedTransform transform;
    try{
      listener.lookupTransform("/turtle2", "/turtle1", ros::Time(0), transform);
      // 前两个参数说明需要从/turtle1到/turtle2的变换，
      // 第3个参数是想要的变换的时间，ros::Time(0)表示最新可以获得的变换，
      // 第4个参数是存储变换。
    }
    catch (tf::TransformException &ex) {
      ROS_ERROR("%s",ex.what());
      ros::Duration(1.0).sleep();
      continue;
    }

    geometry_msgs::Twist vel_msg;
    // 变换用于计算/turtle2新的线速度和角速度
    vel_msg.angular.z = 4.0 * atan2(transform.getOrigin().y(),
                                    transform.getOrigin().x());
    vel_msg.linear.x = 0.5 * sqrt(pow(transform.getOrigin().x(), 2) +
                                  pow(transform.getOrigin().y(), 2));
    turtle_vel.publish(vel_msg);

    rate.sleep();
  }
  return 0;
};
```
2. 运行listener
打开CMakeLists.txt文件，在末尾加上下面内容
```
add_executable(turtle_tf_listener src/turtle_tf_listener.cpp)
target_link_libraries(turtle_tf_listener ${catkin_LIBRARIES})
```
回退到catkin工作目录，编译包
`catkin_make`
一切正常的话，在devel/lib/learning_tf文件夹下，出现一个名为turtle_tf_listener的二进制文件。如果存在，把它加入到launch文件中。打开start_demo.launch文件，在launch块中加入node块
```
<launch>
    ...
    <node pkg="learning_tf" type="turtle_tf_listener" name="listener" />
    ...
</launch>
```
启动launch文件
`roslaunch learning_tf start_demo.launch`

### tf命令行工具
`static_transform_publisher`是用于发布静态transforms的命令行工具。它既可以在命令行中使用，又可以在launch文件中设置静态变换。
`static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms`
利用x/y/z（单位m）和yaw/pitch/roll（单位radian）发布静态坐标变换到tf，周期单位为ms。
`static_transform_publisher x y z qx qy qz qw frame_id child_frame_id period_in_ms`
利用x/y/z（单位m）和四元数发布静态坐标变换到tf，周期单位为ms。

## [topic_tools/relay](http://wiki.ros.org/topic_tools/relay)
relay是ROS的一个节点，属于topic_tools包的一部分，它订阅一个话题并将收到的数据再发布给另一个话题，使用规范：
`relay <intopic> [outtopic]`
Subscribe to `<intopic>`and republish to another topic.
* intopic: Incoming topic to subscribe to
* outtopic: Outgoing topic to publish on (default: intopic_relay)
## [move_base](http://wiki.ros.org/move_base/)
The move_base package provides an implementation of an action (see the actionlib package) that, given a goal in the world, will attempt to reach it with a mobile base. The move_base node links together a global and local planner to accomplish its global navigation task. It supports any global planner adhering to the nav_core::BaseGlobalPlanner interface specified in the nav_core package and any local planner adhering to the nav_core::BaseLocalPlanner interface specified in the nav_core package. The move_base node also maintains two costmaps, one for the global planner, and one for a local planner (see the costmap_2d package) that are used to accomplish navigation tasks.
## [joint_state_publisher](http://wiki.ros.org/joint_state_publisher)
This package contains a tool for setting and publishing joint state values for given URDF.This package publishes [sensor_msgs/JointState](http://docs.ros.org/api/sensor_msgs/html/msg/JointState.html) messages for a robot. The package reads the robot_description parameter, finds all of non-fixed joints and publishes a JointState message with all those joints defined. Can be used in conjunction with the [robot_state_publisher](http://wiki.ros.org/robot_state_publisher) node to also publish transforms for all joint states.

## [robot_state_publisher](http://wiki.ros.org/robot_state_publisher)
This package allows you to publish the state of a robot to [tf](http://wiki.ros.org/tf). Once the state gets published, it is available to all components in the system that also use tf. The package takes the joint angles of the robot as input and publishes the 3D poses of the robot links, using a kinematic tree model of the robot. The package can both be used as a library and as a ROS node. This package has been well tested and the code is stable.

## problems
1. "Package [] does not have a path"
出现这个问题，可能是因为`<mesh filename="package://your_package/meshes/file.DAE"/>`中yourpackage的路径设置有问题，或者没有启动相应的setup.bash。


## Navigation
### [Setting up your robot using tf](http://wiki.ros.org/navigation/Tutorials/RobotSetup/TF)
tf manage the transformation between two coordinate frames for us by transform tree. tf assumes that all transforms move from parent to child.

### []()

## USB Camera(UVC)

## SLAM
0. 贝叶斯滤波
基于条件独立状态转移概率
$$p({\bm{x_t}} | \bm{x_{0:t-1}}, \bm{z_{1:t-1}}, \bm{u_{1:t}}) = p(\bm{x_{t}} | \bm{x_{t-1}}, \bm{u_{t}})$$
概率$p(\bm{x_{t}} | \bm{x_{t-1}}, \bm{u_{t}})$为状态转移概率，指明环境状态作为机器人控制$\bm{u_t}$的函数如何随着时间变化，即时刻t的状态随机的依赖时刻t-1的状态和时刻t的控制。
基于条件独立的测量概率
$$p({\bm{z_t}} | \bm{x_{0:t}}, \bm{z_{1:t-1}}, \bm{u_{1:t}}) = p(\bm{z_{t}} | \bm{x_{t}})$$
概率$p(\bm{z_{t}} | \bm{x_{t}})$为测量概率，说明测量$\bm{z_t}$只由环境状态$\bm{x_t}$产生，即时刻t的测量sui9ji的依赖时刻t的状态。
上述状态转移概率和测量概率模型也称为隐马尔科夫模型，或者动态贝叶斯网络。
**贝叶斯滤波算法**
贝叶斯滤波是递归的，根据测量和控制数据计算置信度分别$bel()$，算法过程如下：
$\bm{Algorithm Bayes\_filter}(bel(x_{t-1}), u_t, z_t)$
$for\ all\ x_t\ do$
$\quad $ $\overline{bel}(x_t) = \int{p(x_t|u_{t}, x_{t-1})\ bel(x_{t-1})}dx_{t-1}$
$\quad $ $bel(x_t) = \eta p(z_t | x_t) \overline{bel}(x_t)$
$end\ for$
$return\ bel(x_t)$
公式第3行是预测(prediction)过程，公式第4行是测量更新(measurement update)。
为了递归的计算后验置信度，算法需要一个时刻$t=0$的初始置信度$bel(x_0)$作为边界条件。如果$x_0$是确定值，$bel(x_0)$应该用一个点式群体分布进行初始化，该点式群体分布将所有概率集中在$x_0$的修正值周围，而其他点的概率为0。如果$x_0$完全忽略，$bel(x_0)$可使用在$x_0$的邻域上的均匀分布来初始化。


1. 粒子滤波器(Particle Filter)
1.1 初始化(initialization)
如果机器人的起始位姿未知，用N个粒子在所有可能的位姿范围内随机安排机器人的位姿。每个初始粒子的权重为1/N，总和为1，N为经验值，通常为数百。如果机器人的初始位置已知，则将粒子放置在其附近。
1.2 预测(prediction)
根据机器人的运动的系统模型(system model)，给被观察到的移动量(odometry, IMU等)加上噪声，用这种方式移动各个粒子。
1.3 调整(update)
基于所测量到的传感器信息，计算每个粒子的概率，并且通过反映这个值更新每个粒子的权重值。
1.4 姿态估计(pose estimation)
利用所有粒子的位置、方向和权重值来计算加权平均值、中央值和最大权重的粒子值，并用这些估计机器人的姿态。
1.5 重采样(Resampling)
这是生成新粒子的过程，去除权重小的粒子，以权重大的粒子为中心创建继承了现有粒子的特性(姿态信息)的新粒子。粒子的总数量保持不变。

2. 蒙特卡洛定位(Monte Carlo Locationlization)
设机器人在t时刻的位置和方向$(x, y, \theta)$为$\bm{x}_t$, 距离传感器到t时刻为止获得的距离信息记为$z_{0...t} = \{z_0, z_1,\cdots ,z_t\}$，编码器到t时刻为止获得的机器人的运动信息记为$u_{0...t} = \{u_0, u_1, \cdots, u_t\}$。建立机器人传感器模型和移动模型，并通过贝叶斯滤波器获得后验概率。首先，在预测阶段利用机器人的移动模型$p(\bm{x}_t | \bm{x}_{t-1}, \bm{u}_{t-1})$、前一个位置上的概率$bel(\bm{x}_{t-1})$和从编码器获得的运动信息$u$，计算下一个时刻的机器人位置$\overline{bel}$,
$$\overline{bel}(\bm{x}_t) = \int p(\bm{x}_t | \bm{x}_{t-1}, \bm{u}_{t-1})bel(\bm{x}_{t-1})dtx_{t-1}$$
在预测阶段，利用传感器模型$p(z_t|x_t)$、上面求得的概率$\overline{bel}(\bm{x}_t)$和归一化常数$\eta_t$，求得基于传感器信息提高准确度的当前位置的概率$bel(\bm{x}_t)$，
$$bel(\bm{x}_t) = \eta_tp(z_t|x_t)\overline{bel}(\bm{x}_t)$$
得到当前位置的概率$bel(\bm{x}_t)$后，通过粒子滤波器生成N个粒子来估计位置。在MCL中，使用术语“样品”代替“粒子”。首先，在抽样阶段， 使用前一个位置的概率$bel(x_{t-1})$中的机器人的移动模型$p(\bm{x}_t | \bm{x}_{t-1}, \bm{u}_{t-1})$来提取新的样品集合$x_{t}^{'}$。利用样品集合$x_{t}^{'}$中的第i个样品$x_{t}^{'(i)}$、距离传感器获得的距离信息$z_t$和归一化常数$\eta$来计算权重值$\omega_{t}^{(i)}$,
$$\omega_{t}^{(i)} = \eta p(z_t | x_{t}^{'(i)})$$
在重采样过程中，使用样品$x_{t}^{'}$和权重$\omega_{t}^{(i)}$来创建N个新的样品集合$X_t$，
$$X_t = \{x_{t}^{(j)} | j = 1 \cdots N\} \sim \{x_{t}^{'(i)}, \omega_{t}^{(i)}\}$$
3. 自适应蒙特卡洛定位

4. 动态窗口法(Dynamic Windows Approach, DWA)

## Package Dependence
1. 查看包缺少的依赖
`rosdep check --from-paths`
2. 在~/catkin_ws/下安装缺少的依赖
`rosdep install -r --from-path src --ignore-src`
[在open_manipulator catkin_cmake出错，缺少依赖解决方法](https://answers.ros.org/question/310452/the-issue-came-by-catkin_make-of-open_manipulator/)



## References
[1] [Ubuntu 16.04 使用docker资料汇总与应用docker安装caffe并使用Classifier（ros kinetic+usb_cam+caffe）](https://blog.csdn.net/zhangrelay/article/details/54671412)
[2] [Ubuntu与ROS的Docker桌面系统与ROS在线练习课程（在线Linux虚拟机）](https://www.cnblogs.com/shangchele/p/7328215.html)
[3] [ROS 安装、环境配置与测试](https://www.shiyanlou.com/courses/854/labs/3096/document/)
[4] [在Ubuntu中安装ROS Kinetic](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu)
[5] [ROS中文教程](http://wiki.ros.org/cn/ROS/Tutorials)
[6] [Docker--从入门到实践，安装Docker](https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html)
[7] [usv_gazebo_plugins](https://bitbucket.org/osrf/vrx/wiki/tutorials)
[8] [ROS(1和2)机器人操作系统相关书籍、资料和学习路径](https://blog.csdn.net/zhangrelay/article/details/78179097)
[9] Probabilistic Robotics
[10] udacity中的[Artificial Intelligence for Robotics](https://classroom.udacity.com/courses/cs373/lessons/48739381/concepts/487350240923)
[] [细说贝叶斯滤波](https://www.cnblogs.com/ycwang16/p/5995702.html?utm_source=itdadao&utm_medium=referral)
[] []()
[] []()
[] []()
[] []()
[] []()