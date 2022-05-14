# Nginx总结
## 1 概述
### 1.1 简介
Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。俄罗斯人于2004年发布。  
Nginx完全用C语言从头写成。官方数据测试表明能够支持高达 50,000 个并发连接数的响应。
### 1.2 安装
Windows：http://nginx.org/en/download.html 下载稳定版本。
以nginx/Windows-1.16.1为例，直接下载 nginx-1.16.1.zip。下载后解压到相应目录即可。  
Linux：

    1、安装依赖：
    yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel wget
    2、Nginx下载：
    wget -P /opt/ http://nginx.org/download/nginx-1.18.0.tar.gz
    3、Nginx解压进入目录：
    tar -zxvf nginx-1.18.0.tar.gz
    cd nginx-1.18.0
    4、Nginx安装：
    ./configure
    make && make install
    5、安装完成后的路径为（默认）：/usr/local/nginx
卸载：
    1.首先输入命令 ps -ef | grep nginx检查一下nginx服务是否在运行。
    2.停止Nginx服务   
    3.查找、删除Nginx相关文件   
    查看Nginx相关文件：whereis nginx  
    find查找相关文件：find / -name nginx   
    依次删除find查找到的所有目录：rm -rf /usr/sbin/nginx   
    4.再使用yum清理：yum remove nginx
### 1.3 配置文件
​    配置文件路径./conf/nginx.conf
​    

    系统配置：server，可以配置多个server  
    转发规则：location路径、root目录、index欢迎页面  
    反向代理规则：location拦截路径、proxy_pass转向地址、index
### 1.4 基本命令
​    启动  ./sbin/nginx
​    停止 ./sbin/nginx -s stop 或 quit 或 taskkill /f /t /im nginx.exe
​    重载配置  ./sbin/nginx -s reload 或service nginx reload 
​    重载指定配置文件 ./sbin/nginx -c /usr/local/nginx/conf/nginx.conf
​    查看nginx版本 ./sbin/nginx -v
​    检查配置文件 ./sbin/nginx -t
​    显示帮助信息 ./sbin/nginx -h

## 2 反向代理
正向代理：访问一个代理服务器，由代理服务器再访问我们要访问的网站，代理服务器获得访问网站的返回信息后，再返回给我们。
反向代理：我们访问nginx，而nginx（前置机）代理后面的服务器，由它去决定具体访问哪台服务器。这种方式和正向代理刚好反过来。

反向代理的好处，它屏蔽了后台具体的服务器，我们访问者根本不知道访问的哪台服务器，这样使访问更加安全。

**场景1**：访问www.123.com直接跳转到127.0.0.1:8080。

步骤：启动一个tomcat服务（127.0.0.1:8080），通过修改本地host文件（在C:\windows\system32\drivers\etc或/etc/hosts）增加下面一行，将 www.123.com 映射到 127.0.0.1：

    127.0.0.1 www.123.com
在nginx.conf中增加一个server配置：

	#1、全局配置块
	user  nginx;          #设置nginx服务的系统使用用户
	worker_processes  1;             #工作进程数（和cpu核心数保持一致）
	#error_log  /var/log/nginx/error.log warn;     #nginx的错误日志
	#pid        /var/run/nginx.pid;      #nginx服务启动时候pid
	
	#2、events配置块
	events {
		worker_connections  1024;    #每个进程允许最大连接数
	}
	
	#3、http配置块
	http {
		...
		#server配置块
		server {
			listen  80;
			server_name  www.123.com;
	
			location / {
				proxy_pass http://127.0.0.1:8080;      #反向代理
				index  index.html index.htm index.jsp;
			}
		}
	}
## 3 负载均衡
提供两种负载均衡策略：内置策略（轮询、加权轮询）、扩展策略。
首先通过一个nginx进行转发到2个nginx上，然后再通过nginx进行负载到多个tomcat集群上。这样的结构优点是，防止单个节点压力过重。

也可以实现keepalive和HAProxy+nginx集群，实现nginx高可用。

**场景2**：假设tomcat1（192.168.32.134:8080），tomcat2（192.168.32.135:8080）布置的是同样的服务，用户通过www.xx.com进行访问，实现请求在两台服务器上均匀分布。

	http{
		...
		#设定负载均衡的服务器列表
		upstream myserver {
			#weigth参数表示权值，权值越高被分配到的几率越大
			server 192.168.32.134:8080  weight=1;
			server 192.168.32.135:8080  weight=1;
		}
		server {
			#监听80端口
			listen  80;
			#定义使用www.xx.com访问
			server_name  www.xx.com;
			#默认请求
			location / {
				# 代理地址
				proxy_pass  http://myserver;
				root   /root;      #定义服务器的默认网站根目录位置
				index index.php index.html index.htm;  
			}
		}
	}

**场景3：热备。**如果你有2台服务器，当1台服务器发生事故时，才启用第2台服务器给提供服务。服务器处理请求的顺序：AAA突然A挂啦，BBBBB…

	upstream mysvr { 
		server 192.168.32.134:8080 
		server 192.168.10.121:3333 backup;  #backup热备     
	}

**场景4：轮询。**nginx默认就是轮询其权重都默认为1，服务器处理请求的顺序：ABABABABAB…

	upstream mysvr { 
		server 127.0.0.1:7878;
		server 192.168.10.121:3333;       
	}

**场景5：加权轮询。**跟据配置的权重的大小而分发给不同服务器不同数量的请求。下面服务器的请求顺序为：ABBABBABB…

	upstream mysvr { 
		server 127.0.0.1:7878 weight=1;
		server 192.168.10.121:3333 weight=2;
	}

## 4 动静分离
静态资源：图片、css、js、html（静态资源处理时并发非常高）  
动态资源：asp/aspx、php、jsp

**场景6**：只要是以jpg或者gif结尾的，去配置的指定目录下查找：

        location ~ .*\.(jpg|gif|png|flv)$ {
            root /usr/local/images;
        }

## 5 实战

## 6 Nginx优化
部分常用优化如下：
> user root; 对应系统哪个用户，最好专为nginx创建用户和组，并单独设置权限，这样安全。如：user nginx nginx。
> 
> worker_processes  1; 定义了nginx对外提供web服务时的worker进程数，习惯配置和当前服务器的core数（CPU内核数）相同，或者2倍。

> worker_connections  1024; 设置可由一个worker进程同时打开的最大连接数。开启数为实际数量的1/4，浏览器访问时会自动发起2个；反向代理tomcat又是两个。这个数值和操作系统能打开的文件数。理论上并发数=worker_processes * worker_connections。跟物理内存大小也有关系，因为系统打开的文件数和系统的内存成正比。一般1GB内存可也打开大约10W左右。

> worker_rlimit_nofile  65535; 定义了一个nginx进程打开最多的文件数目，配置和Linux下文件打开个数一致。ulimit–n来查看，最大设置为65535。设置为“auto”将尝试自动检测它。

> keepalive_timeout  10; 给客户端分配keep-alive链接超时时间，服务器将在这个超时时间过后关闭链接。我们将它设置低些可以让ngnix持续工作的时间更长。一般设置65左右。

> open_file_cache  max=100000  inactive=20s; 打开缓存的同时也指定了缓存最大数目，以及缓存的时间。我们可以设置一个相对高的最大时间，这样我们可以在它们不活动超过20秒后清除掉。

> gzip  on; 告诉nginx采用gzip压缩的形式发送数据，这将会减少我们发送的数据量。

> gzip_comp_level  4; 设置数据的压缩等级。这个等级可以是1-9之间的任意数值，9是最慢但是压缩比最大的。通常设置为3-5，太高会占用CPU。

> events { use  epoll; } 设置用于复用客户端线程的轮询方法。Linux推荐使用epoll模型。

> access_log  off; access_log 设置nginx是否将存储访问日志。关闭这个选项可以让读取磁盘IO操作更快(aka,YOLO)。

> error_log /var/log/nginx/error.log  crit; error_log 告诉nginx只能记录严重的错误，Error日志优化，运行期间设置crit，可以减少I/O。
