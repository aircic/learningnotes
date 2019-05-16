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
#### 功能：ROS中运行的最小处理器单元，视作一个可执行程序。
节点运行需要向主节点注册名称：
* 节点名称
* 发布者(publisher)名称
* 订阅者(subsriber)名称
* 服务服务器(service server)名称
* 服务客户端(service client)名称
* 消息形式
* URI地址和端口
#### 节点间通信：
* 普通节点与主节点之间通信通过XMLRPC
* 普通节点之间的通信通过TCPROS

### 消息
#### 消息通信方法：TCPROS和UDPROS等
#### 消息分类：
* 单向消息发送/接收方式的话题(topic)
 * * 
* 双向消息请求(request)/响应(response)方式的服务(servive)

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


## catkin_make配置CMakeList.txt
1. `cmake_minimum_required(VERSION 2.8.3)`
2. `project(ros_tutorials_topic)`
3. find catkin packages
``` cmake
find_package(catkin REQUIRED COMPONENTS 
    message_generation
    ros_cpp
    std_msgs)
```
4. generate messages in the 'msg' folder
``` cmake
add_message_files(
    FILES MsgTutorial.msg
)
```
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
**DEPENDS**: system dependencies of this project that dependent projects also need
 message_generation 
``` cmake
catkin_package(
    LIBRARIES ros_tutorial_topic
    CATKIN_DEPENDS message_generation roscpp std_message
)
```
6. `include_directories(${catkin_INCLUDE_DIRS})`
7. 
``` cmake
add_executable(topic_publisher src/topic_publisher.cpp)
add_dependencies(topic_publisher ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_publisher ${catkin_LIBRARIES})
```
``` cmake
add_executable(topic_subscriber src/topic_subscriber.cpp)
add_dependencies(topic_subscriber ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_subscriber ${catkin_LIBRARIES})
```

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

## ROS Simulation Introduction
1. URDF建模
URDF是以<robot>标签来开始，详细内容中通常会反复交替出现<link>标签和<joint>标签，这两种标签分别用来定义机器人的组件：连杆和关节。其中，为了与ROS-Control共用，通常还包括用于设置关节和电机之间的关系的<transmission>标签。
1.1 `<link>`标签的属性
*  `<link>`    设置栏杆的可视化、碰撞和惯性信息
* `<material>` 描述连杆颜色和纹理等信息，其中颜色使用<color>标签，eg. `<color rgba="0.0 0.0 0.0 1.0"/>`，其中rgba输入的四个数字分别表示红色、绿色、蓝色和透明度。
* `<collision>`设置碰撞计算的信息，允许输入标量连杆外形范围的几何信息
* `<visual>`   设置连杆可视化信息
* `<inertial>` 设置连杆惯性信息
* `<mass>`     设置连杆的质量（单位：kg）
* `<inertia>`  设置惯性张量
* `<origin>`   设置相对与连杆相对坐标系的移动和旋转
* `<geometry>` 输入模型的形状，如box、cylinder、sphere，也可以导入COLLADA(.dae)、STL(.stl)格式的设计文件
* `<material>` 
1.2 `<joint>`标签属性
* `<joint>`    设置与连杆的关系和关节类型
* `<paremnt>`  关节的父连杆
* `<child>`    关节的子连杆
* `<origin>`   将父连杆的坐标系转化为子连杆的坐标系
* `<axis>`     设置旋转轴
* `<limit>`    设置关节的速度(单位rad/s)、力(单位N)和半径(仅当关节是revolute或prismatic时)
1.3 




## USV ROS Simulation
[]


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