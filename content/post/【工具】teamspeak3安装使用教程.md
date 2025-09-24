---
title: 【工具】teamspeak3安装使用教程
date: 2025-09-24
categories:
  - 游戏
image: images/84.webp
---
# 前言
主要跟朋友开黑语音使用，感觉比较不错的一个语音软件，用别人的经常掉线，就自己部署一台服务器。
部署前需要安装docker，这里就不细说了。
# 部署
我这里使用**阿里云服务器**进行部署：
部署前需要开通三个端口：
```
9987
10011
30033
```
UDP和TCP都要开。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924215117.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924215130.png)
命令：
```
docker run -d -p 9987:9987/udp -p 10011:10011 -p 30033:30033 -e TS3SERVER_LICENSE=accept teamspeak
```
直接这一行命令即可。
部署完成后
使用命令：
```
docker ps
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924215022.png)
记录docker镜像ID。
然后查看
```
docker logs -f 238b55088c9a
```
需要把这几个值记录下来。
**loginname**
**password**
**apikey**
**token**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924215242.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924215348.png)
这样就部署完成了。
# 管理员登录
直接进行连接即可。
**服务器别名或地址：（服务器IP地址）**
**服务器密码：（password）**
**昵称：（loginname）**
**一次性权限密钥（token）**
![9f3e9acc9b6bbeb15ab960415487b9aa.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/9f3e9acc9b6bbeb15ab960415487b9aa.png)
# 普通用户登录
直接服务器输入**服务器IP地址**即可。其他都不需要输入，即可连接。