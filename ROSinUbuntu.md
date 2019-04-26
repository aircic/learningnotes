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

## References
[1] [Ubuntu 16.04 使用docker资料汇总与应用docker安装caffe并使用Classifier（ros kinetic+usb_cam+caffe）](https://blog.csdn.net/zhangrelay/article/details/54671412)
[2] [Ubuntu与ROS的Docker桌面系统与ROS在线练习课程（在线Linux虚拟机）](https://www.cnblogs.com/shangchele/p/7328215.html)
[3] [ROS 安装、环境配置与测试](https://www.shiyanlou.com/courses/854/labs/3096/document/)
[4] [在Ubuntu中安装ROS Kinetic](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu)
[5] [ROS中文教程](http://wiki.ros.org/cn/ROS/Tutorials)
[] []()
[] []()
[] []()
[] []()
