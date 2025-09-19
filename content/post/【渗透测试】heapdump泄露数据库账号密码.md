---
title: 【渗透测试】heapdump泄露数据库账号密码
date: 2025-09-19
categories:
  - 渗透测试
image: images/78.webp
---
开局一个报告，怎么说呢，护网期间被打穿了，不过庆幸的是不是我们负责的网站。没有任何责任，哈哈哈哈
，话不多说，上过程。
# 过程
首先一个heapdump的连接
```
http://xxxx:8888/admin/actuator/heapdump
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250919102858.png)
经过分析，泄露的信息总结一下（每个文件泄露的信息都是不一样的，这时候就要看运气了）：
```
数据库账号密码：
- MySQL 用户名：xxxx 
- MySQL 密码：xxxx（明文泄露）  
- 数据库地址：`xxxx-mysql:3306`（服务名 / 端口）  
- 数据库名：`xxxx`
  
  - Spring Security 默认用户密码：`84acxxxxx2c`（明文）
这个可以直接用user/默认用户密码进行登录试一下，绝对有惊喜。
```
就这两个信息有用，可以直接对服务器进行攻击。
这里尝试了一下，发现数据库可以直接进行连接，就离谱。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250919103103.png)