## 1. linux中的.bashrc与.profile区别
[参考链接](https://wido.me/sunteya/understand-bashrc-and-profile/)  
## 2. d2l-en通过conda创建虚拟环境时，出现CondaHTTPError问题解决方法
[添加anacondad清华镜像源到.condarc](https://blog.csdn.net/k7arm/article/details/77799092)
## 3. linux中的无法在文件目录建立新的文件
>说明没有对文件的写入权利，通过命令行给所有用户增加文件写入权限  

>>```sudo chmod a+w [file]```  

## 4. conda
```conda list```  #显示安装包  
```conda search [package]```  # 搜索包的信息  

## 5. sys.path.insert(0,...)


## 6. 树莓派3.5寸屏幕和hdmi显示互换
设为HDMI显示：  
```cd LCD-show```  # 进入LCD安装目录  
```./LCD-hdmi```   
设为LCD显示：  
```cd LCD-show```  # 进入LCD安装目录  
```./LCD35-show```   
[LCD-wiki](http://www.waveshare.net/wiki/3.5inch_RPi_LCD_(A))   
## 7. %matplotlib inline  
inline表示将matplotlib的图表嵌入到Notebook中（不适用到其他IDE），而%或%%是IPython的magic命令，表示单元命令，对整个但与的代码进行处理。  

## 8. nd.gamma()  
 Factorials are implemented in Gluon using the Gamma function, where 
 $n! = \Gamma(n+1)$

## 9. Python元组tuple()  
 python的元组与列表类似，不同之处在于元组的元素不能修改，元组使用圆括号，列表使用方括号。
 ## 10. Python的yield 
 如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个生成器。
 [yield用法](https://blog.csdn.net/mieleizhi0522/article/details/82142856)
 
## 11. 公式？？？？
 $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$  
  $ L(Y,f(X)=(Y-f(X)^2 $
  
## 12. python函数参数：*args， **kwargs
 在函数调用中使用”*”，我们需要元组;在函数调用中使用”**”，我们需要一个字典
## 13. python的self指向类的实例对象 
注意到__init__方法的第一个参数永远是self，表示创建的实例本身，因此，在__init__方法内部，就可以把各种属性绑定到self，因为self就指向创建的实例本身。有了__init__方法，在创建实例的时候，就不能传入空的参数了，必须传入与__init__方法匹配的参数，但self不需要传，Python解释器自己会把实例变量传进去。如果要让实例的内部属性不被外部访问，可以把属性的名称前加上两个下划线__，在Python中，实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问。  

## 14. ubuntu 18.04 设置静态ip
首先，在命令行中输入"ifconfig -a", 查看系统的网口名称，比如：enp2s0;
然后，编辑/etc/netplan/下的*.yaml文件，如果不存在，自己通过如下命令生成：
sudo vim /etc/netplan/01-network-manage.yaml
再，在*.yaml文件中输入：
```
network:
    version: 2
    renderer: NetworkManager
    ethernets:
        enp2s0:
            addresses: [192.168.1.47/24]
            dhcp4: no
            gateway4: 192.168.1.1
            nameservers:
             addresses: [114.114.114.114, 8.8.8.8]
```
[参考链接](https://www.cnblogs.com/SH170706/p/10357676.html)



