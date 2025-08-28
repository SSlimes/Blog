---
title: 【知攻善防靶场】Linux1
date: 2025-08-28
categories:
  - 靶场
  - 蓝队
image: images/8.webp
---
# 前景需要
```go
看监控的时候发现webshell告警，领导让你上机检查你可以救救安服仔吗！！
1. 黑客的IP地址
2. 遗留下的三个flag
```
**虚拟机账号/密码**
```go
账号/密码：defend/defend
账号/密码：root/defend
```
# 1. 黑客的IP地址
```go
查看日志文件,Linux查看/var/log/wtmp文件查看可疑IP登陆
last -f /var/log/wtmp     或者直接使用命令 lastlog命令
可以看出192.168.75.129，非常可疑。
```
![Pasted image 20250301201516](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201516.png)
```go
答案：192.168.75.129
```
# 2. 第一个flag
```go
黑客使用root登录了系统，我们可以先去root用户下看看黑客使用了什么命令：
使用命令：history
得到第一个flag：thisismybaby
```
![Pasted image 20250301201537](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201537.png)
```go
答案：flag{thisismybaby}
```
# 3. 第二个flag
```go
从第一个flag中我们可以看出，黑客对/etc/rc.d/rc.local进行编辑和提升了执行权限

Linux中/etc/rc.d/rc.local 用于添加开机启动命令

我们cd /etc/rc.d/下，使用ls -la ，查看有rc.local，直接用cat rc.local得到第二个flag
```
![Pasted image 20250301201615](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201615.png)

```go
答案：flag{kfcvme50}
```
# 4. 第三个flag
```go
我们通过查看log日志，我们发现一个redis日志文件，然后看一下redis日志并没有什么结果，那么我们去看一下redis配置文件（more /etc/redis.conf）就得到第三个flag。
```
![Pasted image 20250301201639](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201639.png)
```go
答案：flag{P@ssWrd_redis}
```
# 全部答案：
```go
192.168.75.129
flag{thisismybaby}
flag{kfcvme50}
flag{P@ssW0rd_redis}
```
![Pasted image 20250301201658](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201658.png)