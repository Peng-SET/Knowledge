## Linux命令总结
### 开关机

````
  shutdown -h 关机 -r 重启 now立刻 5分钟后执行  
  poweroff 立刻关机  
  reboot 立刻重启
````

### 快捷键

````
   tab键 自动补全  
   ctrl c 中止进行  
   ctrl z 退出  
   鼠标右键 粘贴  
   ctrl l或clear 清屏
````

### 常用命令

     --help 查看帮助文档  
     man 查看命令说明  
     free -h 查看内存使用情况  
     history 查看历史命令  
     date 显示时间  
     cal 显示公里日期 -j在当年第几天  
     alias 别名 unalias 取消别名 用法：alias xx=‘某命令’  
     | 管道命令 左边获得输出右边执行
    
    df -h 查看磁盘空间  
    du -h 查看目录大小  
    wc -l 查看文件行数 -m 字符数 -c 字节数  
    ln -s 创建软链接 用法：ln 源路径 链接路径  
    sh或./ 执行脚本文件  
    
    zip 打包为zip文件 unzip 解压 用法：zip 目标.zip 源文件  
    tar -czvf打包为tar文件 -xzvf解压 用法：tar -czvf 目标.tar.gz 源文件
### 文件与目录管理

````
 ls -a所有隐藏 -l详细 -t按时间排序 -h易读 -Sr以大小排列  
 cd /根目录 ~home目录 .当前目录 ..上级目录  
 pwd 展示当前路径 tree 树形展示目录结构  
 touch 创建空白文件  
 rm 删除文件 -f强制 -rvf可视化强制  
 mdkir 在当前目录下创建 rmdir删除空目录  
 cp 拷贝 -a全部 用法：cp 源 目标  
 cat/tac 正序/倒序查看  
 more/less 翻页查看按q退出 /xxx向下搜索xxx  
 head/tail -x xxx读取xxx文件前/后x行  
 find / -name xxx 查找xxx文件  
 whereis 显示二进制文件位置  
 which 显示系统文件位置  
 locate 查找  
 ps -ef |grep xxx 查看xxx进程
````

### vi/vim编辑器

````
  vim 进入编辑器命令模式, 可以/xxx和？xxx正反搜索字符串，按n下一个，x删除当前，按dd删除整行，按u撤销上次操作。  
  按i 输入模式 Enter换行，Del删除，ESC退出到命令模式   
  按：进入底线模式，输入wq保存并退出，输入q！取消本次编辑并退出，ESC退出到命令模式
````

### 文件权限

````
 su 切换用户  
 sudo -i 临时拥有root用户权限  
 groupadd 创建一个用户组 groupdel删除一个用户组 groupmod重名名用户组  
 useradd 创建新用户 userdel删除用户 用法：useradd -c “x” -g group xx  
 passwd修改密码  
 chomd 修改文件权限 r可读=4 w可写=2 x可执行=1 -无权限 文件所有者/组者/其他人
````

### 服务管理
    service xxx status/start/stop/restart 查看服务状态/启动/停止/重启  
    ps -ef |grep xxx 查看xxx进程
    ps -ef 查看所有进程  
    kill -9 强制杀死进程  
    ping ip 连接ip地址  
    top -p 查看服务情况

查看日志

````
tail -f /var/log/xxx
````



