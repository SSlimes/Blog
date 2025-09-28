---
title: 【极核GetShell靶场】JWT Token 垂直越权漏洞
date: 2025-09-28
categories:
  - 靶场
image: images/88.webp
---
# 靶机信息
可以通过访问极核官方靶场开启靶机实验：[极核靶场](https://hackhub.get-shell.com/) -> 渗透测试靶场 -> JWT Token – 垂直越权漏洞
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153102.png)
# 漏洞科普
JWT Token垂直越权漏洞的核心在于服务端未正确验证令牌的签名或内容，导致攻击者可通过篡改Payload中的权限字段（如用户角色）非法提升权限。例如，当服务端未校验签名时，攻击者可直接将普通用户的`sub`字段（如`wiener`）修改为`admin`，或修改`sysadmin`字段为管理员标识（如`Y`），从而绕过身份认证，访问管理员专属功能（如管理面板删除用户）。此外，若系统仅依赖客户端提供的未加密字段（如`userInfo`）进行权限判断，攻击者伪造管理员令牌后可直接获得系统最高权限。该漏洞的修复需严格校验签名、限制敏感字段修改，并确保权限与令牌内容的一致性。
# 靶机实战
访问首页，可以看到已经有了默认用户 **`user`** ，但是没有密码。随便输入一个密码，抓取HTTP请求数据包，传输未加密。
```java
POST /dev-api/login HTTP/1.1
Host: node.hackhub.get-shell.com:52555
Content-Length: 37
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Content-Type: application/json;charset=UTF-8
Origin: http://node.hackhub.get-shell.com:52555
Referer: http://node.hackhub.get-shell.com:52555/login?redirect=%2Findex
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: keep-alive

{"username":"user","password":"user"}
```
进行登录请求爆破，对password字段的值进行遍历爆破。根据响应，爆破出**user**用户的密码为 **`123456`**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153140.png)
使用登录凭据：**`user / 123456`** 登录系统，根据提示：Flag在管理员后台。根据靶机标题：**JWT Token垂直越权漏洞** 提示，需要进行垂直越权到admin后台。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153155.png)
查看登录的Cookie，发现为一串JWT：**`eyJhbGciOiJIUzI1NiJ9.eyJsb2dpbl91c2VyX2tleSI6InVzZXIifQ.IKiUadJl6hUuSCnD38CVuIsl-BnGEkSEEECoDpHtFpU`** ，该JWT就是校验当前用户身份的Token。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153211.png)
将JWT Token丢入相关 **解析工具** 或者 **网站** ，这里用无影TscanPlus工具进行演示。解析后可以获取两个信息：**1、login_user_key是对应的用户名，可以尝试篡改为admin越权。2、存在签名校验，需要爆破秘钥。**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153231.png)
使用无影TscanPlus的JWT秘钥破解功能，可以爆破出当前JWT的秘钥为 **`123456`**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153245.png)
将**login_user_key**的值修改为**admin**，配上正确的秘钥：**123456**，然后重新编码，得到新的JWT：**`eyJhbGciOiJIUzI1NiJ9.eyJsb2dpbl91c2VyX2tleSI6ImFkbWluIn0.1M1MhiNpVCNivAfhqsDr4NDlsAnLDriBzgpWEO2aeg0`**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153300.png)
将重新编码后的JWT Token在浏览器Cookie进行替换，然后刷新网页，可以发现后台变了，现在是admin用户的管理后台了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153313.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153322.png)
根据提示：🎃 **听说Flag在管理员的备注里**，在**系统管理** – **用户管理**中，找到admin用户，点击修改，即可在备注里发现Flag。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928153334.png)