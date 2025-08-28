---
title: 【蓝队】windows 端口排查案例
date: 2025-08-28
categories:
  - 蓝队
image: images/6.webp
---
# 0x01 查看端口状态
命令:
```
netstat -ano      
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153540.png)
连接状态有一下几种
- LISTENING：表示**监听** ，表示这个端口处于**开放状态，** 可以提供服务
- ESTABLISHED"：表示是对方与你**已经连接** 正在通信交换数据
- CLOSING：表示**关闭的** 表示端口人为或者防火墙使其关闭(也许服务被卸载)
- TIME WAIT ：表示正在**等待连接** 就是你正在向该端口发送请求连接状态
通过netstat查看网络连接，每条连接后面都有一个PID号，根据PID号可以定位出是哪个进程在监听这个端口
#### 直接查看程序与外部地址的已建立的连接情况
命令:
```
netstat -b
```
显示在创建每个连接或侦听端口时涉及的可执行程序，需要管理员权限，这条程序对于查找可疑程序非常有帮助
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153728.png)

或者我们先通过查看网络连接状态，查看pid再通过PID定位程序
#### 查看已建立的连接
通过如下命令我们优先查找已建立的连接，看是否存在可疑的连接
```
netstat -ano|findstr "ES"
```
如下，我们发现一条可疑tcp连接，本机与一个外部的地址的一个特殊端口已经建立了连接，pid号为5840。（我们要着重观察本地是否与外部地址的特殊端口进行连接）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153714.png)
# 0x02 根据PID定位进程
命令:
```
tasklist
```
tasklist ：显示运行在本地或远程计算机上的所有进程。如下图显示了进程对应的PID号
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153755.png)
#### PID定位进程
上面我们发现了一个可疑的TCP连接，pid号为5840，现在通过这pid定位程序
```
tasklist | findstr "5840"
```
如下可知，5840对一个的程序为payload2.exe
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153808.png)
#### 获取进程的全路径
命令:
```php
wmic process | findstr "payload2.exe"
```
如下，显示了程序的全路径
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153938.png)
或者通过任务管理器找到该进程，再打开进程所在路径
#### 杀死进程
```
taskkill /f /pid  pid号     # /f为强制的意思
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828153948.png)
注：以上可疑程序的payload为MSF的payload。CS的payload通过上述方式并不能查看到，可知CS的payload隐蔽性还是很高的。