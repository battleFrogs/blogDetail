---
title: linux常用命令
layout: post
tags:   
  - 常用命令  
---

Linux是一套免费使用和自由传播的类Unix操作系统，是一个基于POSIX和Unix的多用户、多任务、支持多线程和多CPU的操作系统。它能运行主要的Unix工具软件、应用程序和网络协议。它支持32位和64位硬件。Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。

<!--more-->


### 1.查看内容 

	ls：查看当前的目录的文件   
	ls -l： 显示详细的当前的目录   
	ls -la ：显示隐藏文件   
	ls -R 文件夹：   就会遍历整个文件夹
	pwd： 查看当前所处的路径   
	cat 文件 ： 查看文件内容   
	more 文件  ：可以分页看   
	less 文件  ： 不仅可以分页(空格)，上下翻行（上下键）还可以方便地搜索，(:/),回到末尾（shift+g）,调到文件的起始位置（gg）
	tail -10 文件  ： 查看文件的尾部的10行   
	tail -f user : 实时刷新文件的尾部   
	head -20 文件   ： 查看文件的头部20行    
	cat redis.conf | grep -v ^# 查看剔除#开头的文件内容

### 2.切换移动
	cd 目录：  进入目录   
	cd .. ：回退上一级    
	cd ：默认回到家目录 
	mv zz.txt xx/yy/  将zz文件移动到xx/yy/下面  
	mv xx yy 修改文件xx该名称为yy
	cp srcFile destFile ：拷贝文件   
	cp -r sourceFolder targetFolder 递归复制整个文件夹
    

### 2.修改删除  

	rmdir 文件夹 ：可删除这个空目录  
	rm -r 文件夹： 递归删除文件  （-f 强制删除）   
	vi xx.txt 可以创建出文本编辑器  ：wq保存文件并退出 ，：w保存不退出 ，：q!强制不保存退出    
	sed '/s/aa/AA/g' ：s表示替换功能，g表示符合所有的内容（不加只是针对每一行第一个匹配的内容）
  

### 4.创建增加
	mkdir 文件夹：   创建文件夹   
	mkdir -p 文件夹 ：  可以创建一连串文件夹   
	touch xx.txt 创建一个空的文件  
	echo hello world > xx.txt  创建文件并含有内容 （> 覆盖原文件内容，>> 追加内容）
	

### 5.显示打印
	echo hello world 打印显示出来

### 6.压缩解压   

	tar -cvf xx.tar 解压文件 文件夹路径：   打包归档操作
	gzip testdir.tar ： 压缩操作  
	tar -czvf xx.tar.gz 需要解压的文件 ：打包压缩操作gz后缀
	tar -zxvf xx.tar.gz -C 文件夹目录 ：解压文件夹到指定文件夹目录
	tar -xzvf xx.tar.gz :解压到当前目录 
	
	1、*.tar 用 tar –xvf 解压 
    2、*.gz 用 gzip -d或者gunzip 解压 
    3、*.tar.gz和*.tgz 用 tar –xzf 解压 

### 7.查找文件   

    whereis java whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。
    which java  到某个系统命令是否存在，以及执行的到底是哪一个位置的命令
	find / -name filename.txt 根据名称查找/目录下的filename.txt文件。
	find . -name "*.xml" 递归查找所有的xml文件
	find . -name "*.xml" |xargs grep "hello world" 递归查找所有文件内容中包含hello world的xml文件
	grep -H 'spring' *.xml 查找所以有的包含spring的xml文件
	find ./ -size 0 | xargs rm -f & 删除文件大小为零的文件
	ls -l | grep '.jar' 查找当前目录中的所有jar文件
	grep 'test' d* 显示所有以d开头的文件中包含test的行。
	grep 'test' aa bb cc 显示在aa，bb，cc文件中匹配test的行。
	grep '[a-z]\{5\}' aa 显示所有包含每个字符串至少有5个连续小写字符的字符串的行。
	grep -v ^# 剔除#开头的行
	grep -v ^# redis.conf  > redis-6379.conf  在redis.conf文件剔除#开头输出到新文件（追加可以用>>）
	grep redis | grep 6379 ：过滤多个条件

### 8.请求

    
    curl example.com/form.cgi?data=xxx   get请求，数据在后面带上
    curl -X POST --data "name=kk&name2=gg" example.com/form.cgi  post方法，数据和网站分开（-X 就是提供除get以外的动作）


## 权限控制   
### 1.权限机制：
	linux系统中对文件权限的描述机制：   
	d  　r  w  x   　r  w  x  　 r  -  x   
	   　所有者      　所属组    　　其他人   
	可以表示为二进制：  111 111 101   
	也可以表示为十进制： 7     7   5   

### 2.修改权限


	chmod  u+rwx file   为file添加或取消所属用户的权限 (u代表所属用户  o代表其他用户  g代表所属组的成员用户)   
	chmod 567 file 用数字也可以表示权限  （5 --->  101 ---> r-x）     

### 3.修改拥有者  

	chown hadoop:hadoop  file
	当外部文件权限没有可以删除，则子文件无法删除内部权限    
	修改文件的所有者owner（只能由root权限才能操作chown hadoop:hadoop  file 将file的所有者改成hadoop用户，所属组改成hadoop组  


## 用户管理   

### 1.查看  

	whoami  查看当前登录的身份   
	groups  查看当前用属的组 
	cat /etc/passwd  查看所有用户信息  
	id root 查看用户root信息

### 2.修改   

	usermod -l b a :将a的登陆名改为b   
	usermod -g hadoop itcast0830 :将itcast0830的组改为hadoop组
	usermod -d /home/itcast itcast0830 将itcast0830的主目录改成/home/itcast（要事先创建itcast目录，并且拷入环境变量文件）  
	passwd itcast0830 修改用户的密码 
	groupmod 修改用户组属性   

### 3.删除  

	userdel xx ：删除用户
	userdel -r xx :删除用户的同时删除用户的主目录  
	groupdel yy 删除用户组   

### 4.增加   

	useradd xx ：添加用户  
	passwd xxyyzz  ：设置密码  
	usermod -G hadoop,root itcast0830  : 给itcast0830添加两个组hadoop,root     
	groupadd yy 添加用户组   

### 5.切换   

	su root 切换到root
	sudo rm a.txt 使用管理员身份删除文件


## 常见系统管理（凡是涉及到修改，就一定要用root权限）  
不要轻易使用su去切换到root身份，普通用户使用sudo来执行root权限命令，而且需要将用户添加到sudoers文件中  # vi /etc/sudoers  :hadop ALL=（ALL） ALL  
 
	free 查看内存使用状况   
	top  查看实时刷新的系统的进程信息  
	df -h  ：查看磁盘空间状态信息   
	du -sh * : 查看当前目录下所有子目录和文件的汇总大小   
	ps -ef : 查看系统中当前瞬间进程信息快照
	ps -ef | grep myshell.sh  搜索myshell进程信息      
	ps -ef|grep --color java 高亮要查询的关键字  
	（| grep -v xx 剔除参数xx， "^$"代表空行，"#"代表注释部分）
	ps aux|grep java 查看java进程
	ps aux 查看所有进程
	kill -9 pid 杀掉进程  （-9 表示强制杀死）  
	lsof -i :8080 查看端口属于哪个程序
	

## 网络管理  
修改ip地址配置   
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0  修改该配置文件即可该ip地址或者在root权限下用setup指令通过一个带提示的伪图像界面来修改   

	ifconfig 查看ip地址（ip addr）   
	sudo vi /etc/sysconfig/network  修改其中的hostname配置项，想立即实现执行命令 hostname nidezhujiming   ：  来修改主机名
	sudo vi /etc/hosts  管理内网的主机名  -- ip地址“本地映射”  
	service network restart  重启网络服务root权限下   
	service iptables stop  ：关闭防火墙服务    
	chkconfig iptables off ： 关闭防火墙自动启动   
	vi /etc/inittab 修改系统的默认启动级别  0-关机 6-重启 5-图形界面 3-不带图形
	netstat -nltp  查看当前的进程连接网络的信息      
	ping www.just-ping.com 网络检测    
	
	netstat -lntup  查看服务暴露给外界的端口信息
    说明： l:listening   n:num   t:tcp  u:udp  p:process  
    
    netstat -an |grep ESTABLISHED |wc -l    查看建立稳定连接数量
    
    netstat  -anp  |grep   端口号  查看端口情况
    netstat   -nultp（此处不用加端口号）  查看所有已经使用端口情况


## 常用工具指令   
	wc ：统计文本信息  （行数，词数，字符数）  
	date ：查看或者修改系统的日期和时间   
	echo ： 输出字符或者变量的值   
	vi ： linux系统最通用的文本编辑器  (yy复制，p粘贴，i插入)   