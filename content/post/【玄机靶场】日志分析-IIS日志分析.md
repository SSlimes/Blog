---
title: 【玄机靶场】日志分析-IIS日志分析
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/41.webp
---
# 简介
用户名:server2012
密码:URSZf3A
# flag1
**1.phpstudy-2018站点日志.(.log文件)所在路径，提供绝对路径**
• **默认路径**：`%SystemDrive%\inetpub\logs\LogFiles\`
![Pasted image 20250324193009](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324193009.png)
flag
```
flag{C:\inetpub\logs\LogFiles\W3SVC2}
```
# flag2
**2.系统web日志中状态码为200请求的数量是多少？**
下载日志文件 awk分析
```
awk '$12 == 200 ' u_ex250220.log| wc -l
```
![Pasted image 20250324194157](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194157.png)
flag
```php
flag{2315}
```
# flag3
**3.系统web日志中出现了多少种请求方法**
继续分析请求方式
```
awk '{print $4} ' u_ex250220.log| sort| uniq -c
```
![Pasted image 20250324194228](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194228.png)
flag
```
flag{7}
```
# flag4
**4.存在文件上传漏洞的路径是什么(flag{/xxxxx/xxxxx/xxxxxx.xxx})**
`发现是emlog bing搜索emlog文件上传漏洞`
![Pasted image 20250324194409](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194409.png)
在日志文件中搜索`/admin/plugin.php` 并且返回值为200的日志
![Pasted image 20250324194430](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194430.png)
找到存在文件上传漏洞的php文件
flag
```
flag{/emlog/admin/plugin.php}
```
# flag5
**5.攻击者上传并且利用成功的webshell的文件名是什么**
打包下载源码 解压 火绒报毒
![Pasted image 20250324194518](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194518.png)
在日志文件中搜索`window.php`并且返回值为200
```
grep  "window.php" u_ex250220.log | awk '$12==200
```
![Pasted image 20250324194534](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324194534.png)
也找到上传用的zip插件  
https://github.com/yangliukk/emlog/tree/main
flag
```php
flag{window.php}
```