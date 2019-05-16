## launch文件
参考
[ROS探索总结（五十六）--launch文件](http://www.guyuehome.com/2195)
[ROS wiki roslaunch/XML](http://wiki.ros.org/roslaunch/XML)

1. `<launch>`
launch文件采用XML形式进行描述，包含一个根元素<launch>。
``` xml
<launch>
    ......
</launch>
```
2. `<node>`
launch文件的核心是启动ROS的节点，采用<node>标签定义，一个节点标签最常用的属性：
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
运行上面设置的launch文件后，output_frame的值就设为odom，并加载到ROS参数服务器上。如果需要设置的参数比较多，一个一个设置会非常麻烦，ROS提供了另一种方式加载参数--<rosparam>:
``` xml
<rosparam file=$"(find 2dnav_pr2)/config/costmap_common_params.yaml" command="load" ns="local_costmap" />
```
`<rosparam>`将一个yaml格式文件中的全部参数加载到ROS参数服务器中，需要设置“command”属性为“load”，还可以选择命名空间“ns”。

4. `<arg>`
相当于launch文件内部的局部变量，仅限于launch文件使用，便于launch文件的重构，和ROS节点内部的实现没有关系。
``` xml
<arg name="arg_name" default="arg_value" />


