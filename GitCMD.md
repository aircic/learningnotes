##
1. 创建远程仓库

2. 安装配置Git工具
 2.1 首先在终端下面敲入 `git --version`， 如果正确回显版本号，则说明已经安装好，如果没有则在终端敲入下面这条命令进行安装:
 ``` shell
 sudo apt-get install git -y
 ```
 2.2 配置用户名和邮箱
 ``` shell
 # 如果想设置为全局生效，添加 --global 参数
 git config --global user.name "你的用户名"
 git config --global user.email "你的邮箱"
 ```
 执行上面代码会在家目录（/home/shiyanlou）下建立一个叫 .gitconfig 的文件（该文件为隐藏文件，需要使用 ls -al 查看到）. 内容一般像下面这样， 可以使用 vim 或 cat 查看文件内容:
 ``` shell
 cat ~/.gitconfig
 ```
上面的配置文件就是 Git 全局配置的文件，一般配置方法是 `git config --global <配置名称> <配置的值>`。如果你想使项目里的某个值与前面的全局设置有区别（例如把私人邮箱地址改为工作邮箱），你可以在项目中使用 `git config` 命令不带 --global 选项来设置. 这会在你当前的项目目录下创建 .git/config，从而使用针对当前项目的配置。

3. clone远程仓库到本地
 首先到远程仓库中，点击 Clone or download 按钮，选择使用 Use SSH（或者Use HTTPS），然后点击复制链接按钮，通过下列命令行clone到本地
 ``` shell
 # Use SSH(需要登入github账号)
 git clone git@github.com:anfederico/Clairvoyant.git
 # Use HTTPS
 git clone https://github.com/anfederico/Clairvoyant.git  
 ```

4. 添加文件到索引库
 4.1 添加/修改
 要把一个文件添加或者更新内容到本地索引中，可以使用 `git add` 命令，命令的用法是 git add <文件名|路径名>，具体步骤如下:  

 ``` shell
 # 添加文件之前，先通过cd命令进入到clone下来的文件目录
 echo '这是一个新文件' > new.txt
 cat new.txt         # 在命令行里显示new.txt文件内容
 git add new.txt     # 添加文件到本地索引
 ```
 4.2 删除
 要把仓库里的文件删除掉，可以使用 `git rm` 命令，用法是 git rm [-rf] <文件名|路径>，具体步骤如下:
 ``` shell
 git rm README.md
 ``` 
 4.3 撤销
 要把仓库里的改动撤销回clone下来时的状态（注意，如果改动之后执行了提交就无法再撤销，只能从远程仓库重新克隆一份到本地），可以使用 `git reset` 命令，具体步骤如下:
 ``` shell
 #比如我们要把刚才删除的 README.md 文件给还原回来，就可以在仓库目录下，敲入 git reset --hard HEAD 来回退 
 git reset --hard HEAD
 ```

5. 提交仓库的改动
 在仓库的每一次改动操作之后，推送同步到远程仓库之前，都需要对这一次或这一批次的操作做提交，命令为 `git commit`，用法是 git commit -m "你的提交备注"，只有做好提交动作，才可以开始推送改动到远程仓库同步因为我之前已经撤销了仓库的改动，这里就重新创建一个新的文件，内容就写“测试”两个字，然后提交改动:
 ``` shell
 echo '测试' > new.txt
 git add new.txt  # 如果通过git rm删除了文件，commit会将删除文件信息添加到缓存
 git commit -m '提交一个测试文件'
 ```
6. 推送改动到远程仓库中
 当我们提交了仓库的改动后，就可以开始推送改动的内容到远程仓库了，可以使用 `git push` 命令来推送，用法是 `git push [-u] origin <分支名>`，分支名默认是 master 具体步骤如下。
 第一次推送改动可以使用 -u 参数，使用之后会绑定你这一次的仓库分支名，这样的话下一次推送就不需要加上分支名了，如图，使用之后回提示已经绑定好分支，而且因为我们是 HTTPS 协议方式来克隆的仓库，所以每一次同步操作都需要输入用户名和密码
 ``` shell
 git push -u origin master
 ```
7. 在其他PC上同步进度
 7.1 查看仓库改动
 首先我们可以通过 `git fetch` 命令查看有哪一些新改动，用法是在仓库目录下敲入 :
 ``` shell
 git fetch origin 
 ```
 7.2 下拉仓库同步
确认好更新的内容后，下一步就是把更新给同步到本地仓库中了，通过 `git pull' 命令来实现，具体用法是 `git pull origin <分支名>`，分支名默认是 master，再查看一下目录，可以看到已经同步好了 

8. 创建新的仓库
 ``` shell
 cd <project space>    # 进入项目目录
 mkdir project         # 创建项目子目录
 cd project
 git init              # 初始化Git仓库，在该目录下生产.git文件
 echo "test" >> file1  # 在该目录下创建文件
 git add  file1        # 将文件file1添加到缓存区(index)
 # git status ## 查看状态
 # git diff --cached ## 查看缓存区的修改
 git commit -m "add file1" # 将缓存区的改动文件提交到本地仓库
 # git remote 将本地仓库与远程仓库关联 ## 取消关联git remote remove origin
 git remote add origin https://github.com/aircic/learningnotes.git 
 git push origin master # 将本地仓库同步到远程仓库
 # 如果出现Error：Updates were rejected because the remote contains work that you do not have locally，则先git pull远程仓库，自动合并远程仓库与本地仓库，再git push同步到远程仓库
 git pull origin master
 git push origin master
 ```


 [1] [Github快速上手实战教程，Git实战教程](www.shiyanlou.com)
 [2] Git Community Book 中文版
 [3] [图解Git](https://www.jianshu.com/p/d823cfcb6ee8)
 []
 []
 []
 []
 []