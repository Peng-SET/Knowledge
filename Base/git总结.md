# Git总结
Git是一个开源的分布式版本控制系统，发展史：CSV->SVN->Git。

## 一、安装

最早Git是在Linux上开发的，很长一段时间内，Git只能在Linux/Unix系统上运行。随着Git的使用逐渐普及，一些开发者也慢慢将Git移植到了Windows平台上。目前Git已经发展为可以在 Windows/macOS/Linux/Unix 上运行的跨平台工具。  
你可以从 https://git-scm.com/ 获得Git在Windows/macOS/Linux三个操作系统相关的安装包。  
Centos系统安装：  

    $ yum install git
第一个要配置的是你个人的用户名称和电子邮件地址：

    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com

要检查已有的配置信息，可以使用命令：

    $ git config --list
想了解 Git 的各式工具该怎么用，可以阅读它们的使用帮助，方法有三：

    $ git help <verb>
    $ git <verb> --help
    $ man git-<verb>

## 二、设置SSH公钥
略
## 三、基本操作

初始化一个Git仓库(以/home/gitee/test文件夹为例)

    $ makedir /home/gitee/test 创建文件夹
    $ cd /home/gitee/test   进入文件夹
    $ git init 初始化一个Git仓库
    
    $ touch readme.txt
    $ git diff 查看修改
将文件添加到Git的暂存区

    $ git add readme.txt
    $ git add . 添加所有文件
    $ git commit  -m  "提交信息"（注：“提交信息”里面换成你需要，如“first commit”）
    $ git remote add origin https://gitee.com/xxx/xx.git
    $ git push -u origin master 
    $ git push https://gitee.com/***/test.git

查看仓库当前文件提交状态（A：提交成功；AM：文件在添加到缓存之后又有改动）

    $ git status -s 
    打印历史记录
    $ git log
    版本回退：
    $ git reset --hard head^
    HEAD表示当前版本，上一个版本就是HEAD^，上上个版本就是HEAD^^，当前向上100个可以写成HEAD~100。也直接使用commit id来代替HEAD^。
    历史命令
    $ git reflog