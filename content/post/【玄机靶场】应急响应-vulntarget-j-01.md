---
title: 【玄机靶场】应急响应-vulntarget-j-01
date: 2025-08-31
categories:
  - 靶场
image: images/24.webp
---
# 简介
题目来源公众号  vulntarget
https://mp.weixin.qq.com/s/LHq8O2F-r6rbhVW84Q4KEg
任务环境说明
windows账密：workstation admin@20221123
web端口外部无法访问，请RDP连接上机排查
# flag1
**主站进入后台的文件名称？**
查看apache日志，找到
![Pasted image 20250325111723](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325111723.png)
![Pasted image 20250325111801](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325111801.png)
flag
```
flag{FNeSOgYGkp.php}
```
# flag2
**黑客是从哪个端口上传木马文件的?**
打开小皮查看网页端口，7001打不开，所以为80
![Pasted image 20250325111917](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325111917.png)
flag
```
flag{80}
```
# flag3
**黑客添加的木马文件名称和密码分别是什么，将黑客添加 的木马名称和密码作为flag提交{fag(名称:密码)**
直接可以使用D盾扫出该文件。然后使用webshell检测一下，发现确实是webshell后门工具。
![Pasted image 20250325112640](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325112640.png)
![Pasted image 20250325112156](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325112156.png)
```
flag{api3.php:Admin}
```
# flag4
可以根据日志查看：
![Pasted image 20250325131342](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325131342.png)
```
flag{192.168.112.123}
```