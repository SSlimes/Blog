---
title: 【知攻善防靶场】web2
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/37.webp
---
# 前提须知
```go
1.攻击者的 IP 地址（两个）
2.攻击者的 webshell 文件名
3.攻击者的 webshell 密码
4.攻击者的 QQ 号
5.攻击者的服务器伪 IP 地址
6.攻击者的服务器端口
7.攻击者是如何入侵的
8.攻击者的隐藏用户名
```
**虚拟机账号/密码：**
```go
账号：Administrator
密码：Zgsf@qq.com
```
# 2.攻击者的 webshell 文件名
```go
首先登录到系统，可以看到有一个phpstudy小皮面板，就存在搭建的网站。
```
![Pasted image 20250301200012](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200012.png)
```go
上传D盾文件，先扫一边是否存在webshell。并且成功找到后门文件（system.php文件）成功找到第二个题目。
```
![Pasted image 20250301200030](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200030.png)
```go
答案：system.php
```
# 3.攻击者的 webshell 密码
```go
使用记事本查看一下system.php文件，发现pass为：hack6618
```
![Pasted image 20250301200149](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200149.png)
```go
答案：hack6618
```
# 1.攻击者的 IP 地址（两个）
## a)第一个IP地址
```go
打开小皮面板，查看一下Apache日志，
```
![Pasted image 20250301200223](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200223.png)
```go

从日志里可以看出，192.168.126.135上传了webshell文件，说明这个IP确实是攻击者的IP地址。
192.168.126.135为第一个黑客地址。
```
![Pasted image 20250301200311](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200311.png)
![Pasted image 20250301200322](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200322.png)
# 7.攻击者是如何入侵的
```go
然后我们再看一下ftp是否被攻击，一般黑客也比较喜欢使用ftp发起攻击。
```
![Pasted image 20250301200340](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200340.png)
![Pasted image 20250301200355](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200355.png)
```go
FTP 服务中 226 为登录成功的，530 是登录失败的操作
```
![Pasted image 20250301200419](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200419.png)
```go
从日志中我们可以看到黑客使用ftp服务器上传了system.php文件。这里可以确定黑客使用ftp上传的webshell。
答案：攻击者使用fpt进行入侵。
```
## b)第二个IP地址
```go
我们使用window的日志分析工具，查看远程登录连接成功的日志。
```
![Pasted image 20250301200440](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200440.png)
```go
可以看到hack887$用户连接两次，IP地址为:192.168.126.129。
```
![Pasted image 20250301200500](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200500.png)
```go
答案：192.168.126.129
```
# 5.攻击者的服务器伪 IP 地址
```go
查看最近操作目录
使用命令：%userprofile%/Recent
可以看到黑客编辑了内网穿透工具 frp 相关的文件
```
![Pasted image 20250301200529](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200529.png)
```go
打开frp.ini文件
看到伪 ip
256.256.66.88
伪端口
65536
```
![Pasted image 20250301200543](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200543.png)
```go
答案：256.256.66.88
```
# 6.攻击者的服务器端口
```go
**根据题目五得知：
答案：65536**
```
# 4.攻击者的 QQ 号
```go
直接打开frp.ini文件文件位置可以看出
```
![Pasted image 20250301200620](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200620.png)
```go
那么QQ号应该就是

答案：777888999321
```
# 8.攻击者的隐藏用户名
```go
我们再使用检查隐藏用户名的脚本，看一下黑客是否新加了一个admin账号
```
![Pasted image 20250301200642](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200642.png)
```go
答案：hack887$
```
# 所有答案：
```go
system.php
hack6618
192.168.126.135
192.168.126.129
hack887$
256.256.66.88
65536
777888999321
```
![Pasted image 20250301200703](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200703.png)