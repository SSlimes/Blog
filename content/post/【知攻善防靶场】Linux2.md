---
title: 【知攻善防靶场】Linux2
date: 2025-08-28
categories:
  - 靶场
  - 蓝队
  - 内网攻防
image: images/14.webp
---
# 前提须知
```go
看监控的时候发现webshell告警，领导让你上机检查你可以救救安服仔吗！！
（1）提交攻击者IP
（2）提交攻击者修改的管理员密码(明文)
（3）提交第一次Webshell的连接URL
（4）提交Webshell连接密码
（5）提交数据包的flag1
（6）提交攻击者使用的后续上传的木马文件名称
（7）提交攻击者隐藏的flag2
（8）提交攻击者隐藏的flag3
```
虚拟机账号/密码：
```go
账号：root
密码：Inch@957821.
```
# 1. 提交攻击者IP
### 方法一：
```go
首先看一下管理员登录情况，确定一下黑客登录IP
命令：last -f /var/log/wtmp
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015239.png)
### 方法二：
```go
如果不确定的话，我们可以登录bt面板看一下
这里我看var目录文件时，看到了bt配置文件，那就bt看一下
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015259.png)
```go
果然安装了宝塔，那就登录一下，因为不知道面板的密码那我们就直接修改一下面板的密码。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015315.png)
然后看一下面板的默认信息，看一下url地址
```go
外网面板地址: <https://58.247.126.6:12485/5a2ce72d>
内网面板地址: <https://192.168.198.130:12485/5a2ce72d>
username: uysycv5w
password: admin123
进行登录。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015356.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015404.png)
```go
我们从bt面板里面看到了他添加了一个网站，我们去看一下他的日志（这里我就不用宝塔面板中的去查看日志了），我们首先去/www/wwwlogs目录下127.0.0.1.log 查看一下日志
使用命令：cat 127.0.0.1.log | gerp 200
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015419.png)
```go
答案：192.168.20.1
```
# 2. 提交攻击者修改的管理员密码(明文)
```go
在宝塔面板发现一个phpmyadmin的mysql数据库，进管理看数据，找密码即可。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015432.png)

```go
找到一个x2_user表，密码,这里使用过的是md5进行加密的，不清楚的可以去网上搜一下或者看一下源码，md5加密是不可逆的，但是可以在网上公开的网站进行解密：
URL：<https://www.somd5.com/>
答案：Network@2020
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015447.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015502.png)
```go
答案：Network@2020
```
# 3. 提交第一次Webshell的连接URL
```go
查看流量包，可以看出
index.php?user-app-register
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015513.png)
```go
Webshell的连接：<http://192.168.198.130/index.php?user-app-register>
```
# 4. 提交Webshell连接密码
```go
根据题目3可以看出。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015530.png)
```go
webshell密码是：Network2020
使用蚁剑进行连接。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015548.png)
# 5. 提交数据包的flag1
```go
ls一下，你就会发现默认的文件夹下给了一个流量数据包，下载这个数据包使用wireshark进行分析。

[root@web-server ~]# ls
anaconda-ks.cfg  wp  数据包1.pcapng
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015601.png)
```go
首先，过滤一下http报文，可以看到这些报文都是攻击者192.168.20.1在访问Linux主机。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015612.png)
```go
先浏览一下过滤出来的报文，发现攻击者访问了/flag1路径，追踪http流。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015622.png)
```go
发现了一个第一个flag：flag1{Network@_2020_Hack}，提交判题程序得知正确。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015637.png)
```go
答案：flag1{Network@_2020_Hack}
```
# 6. 提交攻击者使用的后续上传的木马文件名称
```go
查看数据包，可以看出，黑客上传了一个version2.php文件。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015650.png)
```go
答案：version2.php
```
# 7. 提交攻击者隐藏的flag2
```go
根据命令可以看出，黑客进入了127.0.0.1文件夹，并创建了一个隐藏文件夹.api，进入后查看
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015702.png)
```go
可以看出他编辑了这两个文件，cat看一下。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015711.png)
```go
cat alinotify.php,发现flag2。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015724.png)
```go
答案：$flag2 = "flag{bL5Frin6JVwVw7tJBdqXlHCMVpAenXI9In9}";
```
# 8. 提交攻击者隐藏的flag3
```go
使用命令：history,查看历史使用过的命令可以发现
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829015740.png)
```go
答案：flag{5LourqoFt5d2zyOVUoVPJbOmeVmoKgcy6OZ}
```
# 全部答案：
```go
192.168.20.1
Network@2020
<http://192.168.198.130/index.php?user-app-register>
Network2020
flag1{Network@_2020_Hack}
version2.php
flag{bL5Frin6JVwVw7tJBdqXlHCMVpAenXI9In9}
flag{5LourqoFt5d2zyOVUoVPJbOmeVmoKgcy6OZ}
```
