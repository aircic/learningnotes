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


## shell
0. shell文件约定首行以`#!`开头，表明解释shell脚本的解释器的具体位置，比如， `#!/usr/bin/env bash`
###1. 变量
####1.1 定义变量
 var1,var2,var3是变量名，value 是赋给变量的值。如果 value 不包含任何空白符（例如空格、Tab 缩进等），那么可以不使用引号；如果 value 包含了空白符，那么就必须使用引号包围起来。使用单引号和使用双引号也是有区别的，稍后我们会详细说明。
 ==注意： 赋值号`=`周围不能有空格==
 ```shell  
    var1=value
    var2='value'
    var3="value"
 ```
 Shell 变量的命名规范和大部分编程语言都一样：
 * 变量名由数字、字母、下划线组成；
 * 必须以字母或者下划线开头；
 * 不能使用 Shell 里的关键字（通过 help 命令可以查看保留关键字）。
####1.2 使用变量
使用一个定义过的变量，只要在变量名前面加美元符号`$`即可。
``` shell
    var=1
    echo $var
    echo ${var}
```
变量名外面的花括号`{}`是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界。
当变量值中嵌入变量名时，`''`和`""`效果不同，如
``` shell
    url="http://c.biancheng.net"
    website1='C语言中文网：${url}'
    website2="C语言中文网：${url}"
    echo $website1
    echo $website2
```
运行结果：
C语言中文网：${url}
C语言中文网：http://c.biancheng.net
以单引号`''`包围变量的值时，单引号里面是什么就输出什么，即使内容中有变量和命令（命令需要反引起来）也会把它们原样输出。这种方式比较适合定义显示纯字符串的情况，即不希望解析变量、命令等的场景。

以双引号`""`包围变量的值时，输出时会先解析里面的变量和命令，而不是把双引号中的变量名和命令原样输出。这种方式比较适合字符串中附带有变量和命令并且想将其解析后再输出的变量定义。
####1.3 命令的结果赋值给变量
Shell 也支持将命令的执行结果赋值给变量，常见的有以下两种方式：
``` shell
    variable=`command`
    variable=$(command)
```
第一种方式把命令用反引号` `` `（位于 Esc 键的下方）包围起来，反引号和单引号非常相似，容易产生混淆，所以不推荐使用这种方式；第二种方式把命令用`$()`包围起来，区分更加明显，所以推荐使用这种方式。
####1.4 变量作用域
Shell 变量的作用域可以分为三种：
* 变量只能在函数内部使用，这叫做局部变量（local variable），使用关键字`local`；
* 变量可以在当前Shell进程中使用，这叫做全局变量（global variable），shell定义的变量默认为全局变量；
* 变量还可以在子进程中使用，这叫做环境变量（environment variable）。

全局变量的作用范围是当前的Shell进程，而不是当前的Shell脚本文件，它们是不同的概念。打开一个Shell窗口就创建了一个 Shell进程，打开多个Shell窗口就创建了多个Shell进程，每个Shell进程都是独立的，拥有不同的进程ID。在一个Shell进程中可以使用`source`命令执行多个Shell脚本文件，此时全局变量在这些脚本文件中都有效。
全局变量只在当前Shell进程中有效，对其它Shell进程和子进程都无效。如果使用`export`命令将全局变量导出，那么它就在所有的子进程中也有效了，这称为“环境变量”。
环境变量被创建时所处的Shell进程称为父进程，如果在父进程中再创建一个新的进程来执行Shell命令，那么这个新的进程被称作Shell子进程。当Shell子进程产生时，它会继承父进程的环境变量为自己所用，所以说环境变量可从父进程传给子进程。不难理解，环境变量还可以传递给孙进程。

==注意，两个没有父子关系的Shell进程是不能传递环境变量的，并且环境变量只能向下传递而不能向上传递，即“传子不传父”。==

###2. 函数
####2.1 Bash Shell 内建命令

命令 | 说明
---- | ----
: | 扩展参数列表，执行重定向操作
. | 读取并执行指定文件中的命令（在当前 shell 环境中）
alias | 为指定命令定义一个别名，若直接输入该命令且不带任何参数，则列出当前Shell进程中使用了哪些别名。
bg | 将作业以后台模式运行
bind | 将键盘序列绑定到一个 readline 函数或宏
break | 退出 for、while、select 或 until 循环
builtin | 执行指定的 shell 内建命令
caller | 返回活动子函数调用的上下文
cd | 将当前目录切换为指定的目录
command | 执行指定的命令，无需进行通常的 shell 查找
compgen | 为指定单词生成可能的补全匹配
complete | 显示指定的单词是如何补全的
compopt | 修改指定单词的补全选项
continue | 继续执行 for、while、select 或 until 循环的下一次迭代
declare | 声明一个变量或变量类型。
dirs | 显示当前存储目录的列表
disown | 从进程作业表中刪除指定的作业
echo | 将指定字符串输出到 STDOUT
enable | 启用或禁用指定的内建shell命令
eval | 将指定的参数拼接成一个命令，然后执行该命令
exec | 用指定命令替换 shell 进程
exit | 强制 shell 以指定的退出状态码退出
export | 设置子 shell 进程可用的变量
fc | 从历史记录中选择命令列表
fg | 将作业以前台模式运行
getopts | 分析指定的位置参数
hash | 查找并记住指定命令的全路径名
help | 显示帮助文件
history | 显示命令历史记录
jobs | 列出活动作业
kill | 向指定的进程 ID(PID) 发送一个系统信号
let | 计算一个数学表达式中的每个参数
local | 在函数中创建一个作用域受限的变量
logout | 退出登录 shell
mapfile | 从 STDIN 读取数据行，并将其加入索引数组
popd | 从目录栈中删除记录
printf | 使用格式化字符串显示文本
pushd | 向目录栈添加一个目录
pwd | 显示当前工作目录的路径名
read | 从 STDIN 读取一行数据并将其赋给一个变量
readarray | 从 STDIN 读取数据行并将其放入索引数组
readonly | 从 STDIN 读取一行数据并将其赋给一个不可修改的变量
return | 强制函数以某个值退出，这个值可以被调用脚本提取
set | 设置并显示环境变量的值和 shell 属性
shift | 将位置参数依次向下降一个位置
shopt | 打开/关闭控制 shell 可选行为的变量值
source | 读取并执行指定文件中的命令（在当前 shell 环境中）
suspend | 暂停 Shell 的执行，直到收到一个 SIGCONT 信号
test | 基于指定条件返回退出状态码 0 或 1
times | 显示累计的用户和系统时间
trap | 如果收到了指定的系统信号，执行指定的命令
type | 显示指定的单词如果作为命令将会如何被解释
typeset | 声明一个变量或变量类型。
ulimit | 为系统用户设置指定的资源的上限
umask | 为新建的文件和目录设置默认权限
unalias | 刪除当前Shell进程中指定的别名
unset | 刪除指定的环境变量或 shell 属性
wait | 等待指定的进程完成，并返回退出状态码

## Vim
### 普通模式下
按Esc进入普通模式，在该模式下使用方向键或者h,j,k,l键可以移动游标。

按键 | 说明
---- | ----
h | 左
l | 右（小写L）
j | 下
k | 上
w | 移动到下一个单词
b | 移动到上一个单词

在普通模式下使用下面的键将进入插入模式，并可以从相应的位置开始输入
命令 | 说明
---- | ----
i | 在当前光标处进行编辑
I | 在行首插入
A | 在行末插入
a | 在光标后插入编辑
o | 在当前行后插入一个新行
O | 在当前行前插入一个新行
cw | 替换从光标所在位置后到一个单词结尾的字符
从普通模式输入:进入命令行模式，输入w回车，保存文档。输入:w 文件名可以将文档另存为其他文件名或存到其它路径下

在普通模式下
命令 | 说明
---- | ----
. | 重复上一次命令
d | 删除第一个字符， 10x，删除10个连续的字符
dd | 删除一行， 3dd，删除3行文本
dw | dw或者daw(delete a word)删除一个单词，dnw(n替换为相应数字)， 表示删除n个单词

行间跳转
命令 | 说明
---- | ----
nG(n Shift+g) | 游标移动到第 n 行(如果默认没有显示行号，请先进入命令模式，输入:set nu以显示行号)
gg | 游标移动到到第一行
G(Shift+g) | 到最后一行
ctrl+o | 回到上一次光标所在位置

行内跳转
普通模式下使用下列命令在行内按照单词为单位进行跳转

命令 | 说明
---- | ----
w | 到下一个单词的开头
e | 到当前单词的结尾
b | 到前一个单词的开头
ge | 到前一个单词的结尾
0或^ | 到行头
$ | 到行尾
f<字母> | 向后搜索<字母>并跳转到第一个匹配的位置(非常实用)
F<字母> | 向前搜索<字母>并跳转到第一个匹配的位置
t<字母> | 向后搜索<字母>并跳转到第一个匹配位置之前的一个字母(不常用)
T<字母> | 向前搜索<字母>并跳转到第一个匹配位置之后的一个字母(不常用)


复制及粘贴文本
普通模式中使用`y`复制，使用`p`粘贴
命令 | 说明
---- | ----
yy | 复制游标所在的整行（3yy表示复制3行）
y^  | 复制至行首，或y0。不含光标所在处字符。
y$  | 复制至行尾。含光标所在处字符。
yw  | 复制一个单词。
y2w | 复制两个单词。
yG | 复制至文本末。
y1G | 复制至文本开头。
p(小写) | 代表粘贴至光标后（下）
P(大写) | 代表粘贴至光标前（上）
`dd`删除命令就是剪切，你每次`dd`删除文档内容后，便可以使用`p`来粘贴，也这一点可以让我们实现一个很爽快的功能——交换上下行：ddp。

替换和撤销(Undo)命令
替换和Undo命令都是针对普通模式下的操作

命令 | 说明
---- | ----
r+<待替换字母> | 将游标所在字母替换为指定字母
R | 连续替换，直到按下Esc
cc | 替换整行，即删除游标所在行，并进入插入模式
cw | 替换一个单词，即删除一个单词，并进入插入模式
C(大写) | 替换游标以后至行末
~ | 反转游标所在字母大小写
u{n} | 撤销一次或n次操作
U(大写) | 撤销当前行的所有修改
Ctrl+r | redo，即撤销undo的操作

普通模式下输入`>>`整行将向右缩进（使用，用于格式化代码超爽）
普通模式下输入`<<`整行向左回退

快速查找
普通模式下输入/或?, 然后键入需要查找的字符串，按回车后就会进行查找。
命令 | 说明
---- | ----
/<查找内容> | 向下查找
?<查找内容> | 向上查找
n | 继续查找下一个内容
N | 继续查找上一个内容
\\* | 向下寻找游标处的单词
\\# | 向上寻找游标处的单词




### 命令模式
在普通模式下，输入`:`，进入命令模式。
命令 | 说明
---- | ----
set nu | 显示行号
set shiftwidth=n | 设置控制缩进/回退的字符数量
ce | (center)命令使本行内容居中
ri | (right)命令使本行文本靠右
le | (left)命令使本行内容靠左