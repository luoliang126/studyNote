# git基本使用

1. ##### git安装

   ```
   在官网上下载https://git-for-windows.github.io
   
   安装选择路径后next
   
   安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
   
   因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址
   输入：git config --global user.name "Your Name"
   git config --global user.email "email@example.com"
   
   vscode虽然自动集成了git操作，但有时候还是要懂得git的命令行操作！
   git的可视化操作界面，推荐使用sourceTree。
   ```

2. ##### git介绍

   ```
   git、cvs、svn的区别？
   cvs、svn属于 集中式版本控制系统：版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。
   缺点：集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要5分钟，这还不得把人给憋死啊。所有有了git（分布式版本控制系统）。
   git属于分布式版本控制系统：分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
   ```

3. ##### cmd操作

   ```js
   进入CMD 在开始菜单输入框中输入cmd+回车，出现cmd.exe(可右击已管理员省份打开)。键盘上的 “微软键+R”，在命令行输入cmd可以进入cmd操作。
   输入services.msc进入windows服务操作
   ipconfig 查看本机的ip地址信息
   e:  // 进入e盘
   dir 查看当前目录下的所有文件及文件夹
   cd/ 或 cd\  返回根目录
   cd.. 返回上一级
   cd test  去test目录
   cd test/try  去test目录下的try目录
   md newfile 在当前目录下新建一个名为：newfile的文件夹（也可用mkdir newfile）
   cd>a.txt 在当前目录下新建一个a.txt的文件
   del a.txt  删除a.txt文件
   del *.* 删除当前目录下的所有文件（不包括文件夹）
   rd/s D:\aaaaa   删除d盘下的aaaaa文件夹（包括里面的所有内容）
   cls  清除以上所有的操作内容
   exit 退出当前cmd操作面板
   ```
   
4. ##### vs code 快捷键操作

   ```
   F1或Ctrl+Shift+P（俗称万能键）：打开命令面板
   Ctrl+G+行数   跳转到指定行数
   新建文件:   Ctrl+N
   文件之间切换:   Ctrl+Tab
   打开一个新的VS Code编辑器: Ctrl+Shift+N
   关闭当前文件:   Ctrl+W
   关闭当前的VS Code编辑器:   Ctrl+Shift+W
   切换左中右3个编辑器窗口的快捷键:   Ctrl+1  Ctrl+2  Ctrl+3
   代码行向左或向右缩进:   Ctrl+[ 、 Ctrl+]
   代码格式化:   Shift+Alt+F（在文件--首选--设置中，输入tabsize查询，将缩进字符串修改为2或者4）
   向上或向下移动一行:   Alt+Up 或 Alt+Down
   向上或向下复制一行:   Shift+Alt+Up 或 Shift+Alt+Down
   在当前行下方插入一行:   Ctrl+Enter
   在当前行上方插入一行:   Ctrl+Shift+Enter
   移动到行首:   Home
   移动到行尾:   End
   移动到文件结尾:   Ctrl+End
   移动到文件开头:   Ctrl+Home
   移动到定义处:   F12
   查看定义处缩略图(只看一眼而不跳转过去):    Alt+F12
   选择从光标到行尾的内容:   Shift+End
   选择从光标到行首的内容： Shift+Home
   删除光标右侧的所有内容(当前行):   Ctrl+Delete
   扩展/缩小选取范围： Shift+Alt+Right 和 Shift+Alt+Left
   多行编辑(列编辑):   Alt+Shift+鼠标左键 或 Ctrl+Alt+Down/Up
   同时选中所有匹配编辑(与当前行或选定内容匹配):   Ctrl+Shift+L
   下一个匹配的也被选中:   Ctrl+D
   回退上一个光标操作:   Ctrl+U
   撤销上一步操作: Ctrl+Z
   手动保存:   Ctrl+S
   查找:   Ctrl+F
   查找替换:   Ctrl+H
   全屏显示(再次按则恢复):   F11
   放大或缩小(以编辑器左上角为基准):   Ctrl +/-
   侧边栏显示或隐藏： Ctrl+B
   显示资源管理器(光标切到侧边栏中才有效):   Ctrl+Shift+E
   显示搜索(光标切到侧边栏中才有效):   Ctrl+Shift+F
   显示(光标切到侧边栏中才有效):   Git Ctrl+Shift+G
   显示 Debug:    Ctrl+Shift+D
   显示 Output:    Ctrl+Shift+U
   ```
   
   
   
5. ##### git基本操作

   ```
   git init 本地初始化（用于最初创建git版本库）
   
   git remote add url   向远程url添加一个仓库
   	例如：git remote add origin git@github.com:daixu/WebApp.git
   
   git clone git://github.com/schacon/grit.git 从服务器上将代码给拉下来
   
   git branch 查看本地所有分支
   git branch -a 查看所有的分支
   git branch -r 查看远程所有分支
   git branch -D master develop 删除本地库develop
   git branch branch_0.1 master 从主分支master创建branch_0.1分支
   git branch -m branch_0.1 branch_1.0 将branch_0.1重命名为branch_1.0
   
   git status 查看当前状态
   
   git ls-files 看已经被提交的
   
   git diff 查看尚未暂存的更新
   git diff --cached 或 git diff --staged 查看尚未提交的更新
   
   git log 看你当前分支commit的日志
   
   git config --list 查看所有用户
   
   git checkout --track origin/dev 切换到远程dev分支
   git checkout -b dev 建立一个新的本地分支dev
   git checkout dev 切换到dev分支
   
   git add .  暂存当前所有修改内容
   git add README 暂存README文件
   
   git commit 提交(只是暂存，不会push)
   git commit -am "init" 提交并且加注释
   git commit -m 'first commit' 提交暂存，并标注提交内容‘first commit’
   git commit -a 提交当前repos的所有的改变
   git commit -v 当你用－v参数的时候可以看commit的差异
   git commit -a    -a是代表所有，把所有的change加到git
   git commit -a -v 一般提交命令
   git commit -a -m "log_message" (-a是提交所有改动，-m是加入log信息) 本地修改同步至服务器端 ：
   
   git pull 拉取当前分支，远端代码。
   git pull ll 拉取远端ll分支到本地。
   
   git fetch 是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。而git pull则是将远程主机的最新内容拉下来后直接合并，
   	即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。
    git fetch <远程主机名> <分支名>  可以不用主机名，直接分支名
   
   git push (远程仓库名) (分支名) 将本地分支推送到服务器上去。
   git push origin master 将本地项目给提交到服务器中
   git push origin master:hb-dev 将本地库与服务器上的库进行关联
   
   git rm 文件名  git没有直接删除远端代码库的方法，只有从本地或暂存区中删除指定文件，再add，再commit。
   
   git remote show origin 显示远程库origin里的资源
   git remote add origin git@192.168.1.119:ndshow
   
   git merge origin/dev 将分支dev与当前分支进行合并
   
   分支合并：https://www.jianshu.com/p/bfb4b0de7e03
   1、问题描述：当前在ll分支，需要把master分支的代码拉取并合并到ll分支，解决冲突，再提交到ll分支？
   1-1、合并分支都是在本地进行，首先拉取最新的ll分支、master分支代码到本地。
   1-2、checkout到ll分支，执行git merge master 将master最新代码合并到ll分支，这时候再查看冲突，或者全局搜索 <<< （有冲突都会有这个标识）
   1-3、解决冲突后，再执行git add .， git commit等操作，实现代码提交！
   ```

6. 虚位以待