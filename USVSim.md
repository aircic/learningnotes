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
``` shell
r
```
#### 4. 创建机器人模型
#### 5. 建立机器人模型与ROS的连接
#### 6. 使用teleop节点控制机器人
#### 7. 机器人增加传感器（摄像头）
#### 8. 使用Rviz可视化机器人信息


