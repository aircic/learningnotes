# linux 常用命令
cmd | 功能说明
---| -------
pwd | print work directory 打印当前目录 显示出当前工作目录的绝对路径
ps | process status(进程状态，类似于windows的任务管理器)常用参数－auxf，如ps -auxf 显示进程状态
df | disk free 其功能是显示磁盘可用空间数目信息及空间结点信息。换句话说，就是报告在任何安装的设备或目录中，还剩多少自由的空间。
du | Disk usage
rpm | 即RedHat Package Management，是RedHat的发明之一
rmdir | Remove Directory（删除目录）
rm | Remove（删除目录或文件）
cat | concatenate 连锁，cat file1file2>>file3 把文件1和文件2的内容联合起来放到file3中
insmod | install module,载入模块
ln -s  | link -soft 创建一个软链接，相当于创建一个快捷方式
mkdir | Make Directory(创建目录)
touch | touch
man | Manual
su | Swith user(切换用户)
cd | Change directory
ls | List files
ps | Process Status
mkdir | Make directory
rmdir | Remove directory
mkfs | Make file system
fsck | File system check
uname | Unix name
lsmod | List modules
mv | Move file
rm | Remove file
cp | Copy file
ln | Link files
fg | Foreground
bg | Background
chown | Change owner
chgrp | Change group
chmod | Change mode
umount | Unmount
dd | 本来应根据其功能描述"Convert an copy"命名为"cc"，但"cc"已经被用以代表"CComplier"，所以命名为"dd"
tar | Tape archive （磁带档案）
ldd | List dynamic dependencies
insmod | Install module
rmmod | Remove module
lsmod | List module
文件结尾的"rc"（如.bashrc、.xinitrc等） | Resource configuration
Knnxxx /Snnxxx（位于rcx.d目录下） | K（Kill）；S(Service)；nn（执行顺序号）；xxx（服务标识）
.a（扩展名a） | Archive，static library
.so（扩展名so） | Shared object，dynamically linked library
.o（扩展名o） | Object file，complied result of C/C++ source file
RPM | Red hat package manager
dpkg | Debian package manager
apt | Advanced package tool（Debian或基于Debian的发行版中提供）


#[Linux 命令大全](http://www.runoob.com/linux/linux-command-manual.html)


## Linux命令行快捷键
CMD  | 功能说明
---- | ---------
ctrl+a | 将光标移动到行开头
ctrl+e | 将光标移动到行结尾
ctrl+u | 删除光标到行开头的内容
ctrl+k | 删除光标到行结尾的内容