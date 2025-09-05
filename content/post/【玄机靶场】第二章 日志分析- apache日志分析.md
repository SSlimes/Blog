---
title: 【玄机靶场】第二章 日志分析- apache日志分析
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/44.webp
---
# 简介
账号密码 
```
root apacherizhi
ssh root@IP
```
1、提交当天访问次数最多的IP，即黑客IP：
2、黑客使用的浏览器指纹是什么，提交指纹的md5：
3、查看包含index.php页面被访问的次数，提交次数：
4、查看黑客IP访问了多少次，提交次数：
5、查看2023年8月03日8时这一个小时内有多少IP访问，提交次数:
**常见日志文件位置**
**Apache日志**
```php
访问日志：默认位置通常是
/var/log/apache2/access.log.1（Debian/Ubuntu）
/var/log/httpd/access_log.1（CentOS/RHEL）。
错误日志：默认位置通常是
/var/log/apache2/error.log.1（Debian/Ubuntu）
/var/log/httpd/error_log.1（CentOS/RHEL）。
```
**SSH日志**
```php
身份验证日志：通常位于
/var/log/auth.log（Debian/Ubuntu）
/var/log/secure（CentOS/RHEL）。
```
**系统日志**
```php
系统日志：通常位于
/var/log/syslog（Debian/Ubuntu）
/var/log/messages（CentOS/RHEL）。
```
# flag1
**1、提交当天访问次数最多的IP，即黑客IP：**
使用命令：
```php
cut -d- -f 1 access.log.1|uniq -c | sort -rn | head -20
```
![Pasted image 20250331110811](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331110811.png)
```
flag{192.168.200.2}
```
# flag2
**2、黑客使用的浏览器指纹是什么，提交指纹的md5：**
思路是什么，现在我们知道黑客的ip了啊，日志位置我们也知道了啊，直接筛选一波黑客ip不就行了；
命令：
```php
cat access.log.1 |grep 192.168.200.2 |more
```
得到浏览器指纹
```
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
```
![Pasted image 20250331111216](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331111216.png)
![Pasted image 20250331111144](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331111144.png)
加密如下：
```
flag{2D6330F380F44AC20F3A02EED0958F66}
```
# flag3
**3、查看包含index.php页面被访问的次数，提交次数：**
```php
cat access.log.1 | grep "/index.php" | wc -l

`wc -l` 命令用于统计行数，即访问次数。
```
![Pasted image 20250331111410](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331111410.png)
```
flag{27}
```
发现不对，然后看一下日志：
```
cat access.log.1 | grep "/index.php"
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250407140259.png)
最后发现了有两个返回200其他都是404，尝试了flag{2}也不对，在改为25的时候对了。
```
flag{25}
```
# flag4
**4、查看黑客IP访问了多少次，提交次数：**
这里给出三个命令，都是可以查到的：
```
cat access.log.1 | grep "192.168.200.2 - -" | wc -l

cat access.log.1 | grep "192.168.200.2" | cut -d' ' -f1 | sort | uniq -c

grep "192.168.200.2" access.log.1 | cut -d' ' -f1 | sort | uniq -c
```
![Pasted image 20250331111815](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331111815.png)
```
flag{6555}
```
# flag5
**5、查看2023年8月03日8时这一个小时内有多少IP访问，提交次数:**
三条命令如下：
```php
cat access.log.1 | grep "03/Aug/2023:08:" | awk '{print $1}' | sort -nr| uniq -c |wc -l

grep "03/Aug/2023:08:" access.log.1 | awk '{print $1}' | sort -nr | uniq -c | wc -l

cat access.log.1 | grep "03/Aug/2023:08:" | awk '{print $1}' | sort -nr| uniq -c
```
![Pasted image 20250331112024](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331112024.png)
```
flag{5}
```