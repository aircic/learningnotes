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



## USV ROS Simulation
[]


## References
[1] [Ubuntu 16.04 使用docker资料汇总与应用docker安装caffe并使用Classifier（ros kinetic+usb_cam+caffe）](https://blog.csdn.net/zhangrelay/article/details/54671412)
[2] [Ubuntu与ROS的Docker桌面系统与ROS在线练习课程（在线Linux虚拟机）](https://www.cnblogs.com/shangchele/p/7328215.html)
[3] [ROS 安装、环境配置与测试](https://www.shiyanlou.com/courses/854/labs/3096/document/)
[4] [在Ubuntu中安装ROS Kinetic](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu)
[5] [ROS中文教程](http://wiki.ros.org/cn/ROS/Tutorials)
[6] [Docker--从入门到实践，安装Docker](https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html)
[7] [usv_gazebo_plugins](https://bitbucket.org/osrf/vrx/wiki/tutorials)
[8] [ROS(1和2)机器人操作系统相关书籍、资料和学习路径](https://blog.csdn.net/zhangrelay/article/details/78179097)
[] []()
[] []()
[] []()
[] []()
[] []()
[] []()