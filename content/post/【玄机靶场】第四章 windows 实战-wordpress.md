---
title: 【玄机靶场】第四章 windows 实战-wordpress
categories:
  - 靶场
date: 2025-09-01
image: images/26.webp
---
# 简介
第四章 windows实战-wordpress
rdp 端口 3389
账号：administrator
密码：xj@123456
# flag1
**请提交攻击者攻击成功的第一时间，格式：flag{YY:MM:DD hh:mm:ss}**
登录后台，获取后台地址：http://localhost:8080/manage/login.php
![Pasted image 20250325154712](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325154712.png)
在日志里筛选一下：
```
grep  "manage/login.php" access.log
```
可以看到302成功跳转：
```php                                                                                                 
192.168.141.55 - - [29/Apr/2023:22:45:23 +0800] "POST /index.php/action/login?_=139102b0477b064f9cf570483837d74c HTTP/1.1" 302 5 "http://  
192.168.141.188/manage/login.php?referer=http%3A%2F%2F192.168.141.188%2Fmanage%2F" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) G  
ecko/20100101 Firefox/110.0"                                                                                                               
192.168.141.55 - - [29/Apr/2023:22:45:23 +0800] "GET /manage/ HTTP/1.1" 302 5 "http://192.168.141.188/manage/login.php?referer=http%3A%2F  
%2F192.168.141.188%2Fmanage%2F" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                         
192.168.141.55 - - [29/Apr/2023:22:45:23 +0800] "GET /manage/welcome.php HTTP/1.1" 200 10013 "http://192.168.141.188/manage/login.php?ref  
erer=http%3A%2F%2F192.168.141.188%2Fmanage%2F" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"
```
flag
```
flag{2023:04:29 22:45:23}
```
# flag2
**请提交攻击者的浏览器版本 flag{Firgfox/2200}**
由flag1可知
flag
```
flag{Firefox/110.0}
```
# flag3
**请提交攻击者目录扫描所使用的工具名称**
可以看出扫描器的特征：
```
Fuzz Faster U Fool v1.5.0
```
![Pasted image 20250325155258](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325155258.png)
flag
```
flag{Fuzz Faster U Fool}
```
# flag4
**找到攻击者写入的恶意后门文件，提交文件名（完整路径）**
查看日志：
![Pasted image 20250325151625](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325151625.png)
![Pasted image 20250325151645](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325151645.png)
```
flag{C:\phpstudy_pro\WWW\.x.php}
```
# flag5
**找到攻击者隐藏在正常web应用代码中的恶意代码，提交该文件名（完整路径）**
D盾直接扫描即可：
![Pasted image 20250325152028](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325152028.png)
```
flag{C:\phpstudy_pro\WWW\usr\themes\default\post.php}
```
# flag6
**请指出可疑进程采用的自动启动的方式，启动的脚本的名字 flag{1.exe}**
查看开机自动文件夹，并没有发现有什么恶意的东西。
```php
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
并无可疑文件，接下来查看 `Temp` 目录和 `Windows` 目录
在 `Windows` 目录下，发现可疑文件
在windows目录下，发现一个x.bat文件，发现这个bat文件是打开360.exe文件
![Pasted image 20250325153807](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325153807.png)
![Pasted image 20250325161044](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325161044.png)
flag
```
flag{x.bat}
```


