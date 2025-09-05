---
title: 【玄机靶场】第八章 内存马分析-java01-nacos
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/47.webp
---
# 简介
靶机来源 @vulntarget
靶机可以采集本地搭建或者是云端调度
搭建链接 https://github.com/crow821/vulntarget
ssh root@ip    密码xjnacos 启动 /var/local/下的 autorun.sh即可正常启动
问题 1 nacos 用户密码的密文值作为 flag 提交 flag{密文}
问题 2 shiro 的key为多少  shiro 的 key 请记录下来 （备注请记录下，可能有用）
问题 3 靶机内核版本为 flag{}
问题 4 尝试应急分析，运行 get_flag 然后尝试 check_flag 通过后提交 flag
问题 5 尝试修复 nacos 并且自行用 poc 测试是否成功
# flag1
**nacos 用户密码的密文值作为 flag 提交 flag{密文}**
根据题目描述，连接ssh后需在/var/local下运行autorun.sh以启动nacos服务。 
在/var/local目录下存在nacos目录，在nacos/conf/目录下发现nacos-mysql.sql,从中发 现了加密的nacos密码
使用命令：
```
cd nacos/conf   //目录下
查看数据库中的pass关键字：
cat nacos-mysql.sql | grep pass
```
![Pasted image 20250331204127](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331204127.png)
```
flag{$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu}
```
# flag2
**shiro 的key为多少 shiro 的 key 请记录下来 （请记录下来，会有用d）**
在nacos目录下存在一个nacos_config_xxx的zip文件，下载到本地后解压后DEFAULT_GROUP文件下存在三个文件 ADMIN_API、ADMIN_CONFIG、GATEWAY。在ADMIN_CONFIG中发现shiro配置
```
其中key为 KduO0i+zUIMcNNJnsZwU9Q==
```
![Pasted image 20250331204553](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331204553.png)
找到这个文件夹下的这个文件：
```
nacos-server-2.0.1\nacos\nacos_config_export_20231206050259\DEFAULT_GROUP
```
![Pasted image 20250331204658](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331204658.png)
打开找到权限认证的key值：
```
KduO0i+zUIMcNNJnsZwU9Q==
```
![Pasted image 20250331204746](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331204746.png)
```
flag{KduO0i+zUIMcNNJnsZwU9Q==}
```
# flag3
**靶机内核版本为 flag{}**
```
root@vulntarget:/var/local/nacos# uname -a
Linux vulntarget 5.4.0-164-generic #181-Ubuntu SMP Fri Sep 1 13:41:22 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```
![Pasted image 20250331204949](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331204949.png)
```
flag{5.4.0-164-generic}
```
# flag4
**尝试修复 nacos 并且自行用 poc 测试是否成功 此题无flag**
尝试应急分析，运行get_flag然后尝试check_flag通过后提交flag
找一下get-flag和check_flag
```
root@vulntarget:/var/local# find / -name get_flag
/home/vulntarget/get_flag
root@vulntarget:/var/local# 

root@vulntarget:/var/local# find / -name check_flag
/home/vulntarget/check_flag
root@vulntarget:/var/local# 
```
然后运行一下看一下，这里的flag需要应急响应成功后刷新的flag才是正确的
![Pasted image 20250331205849](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331205849.png)
```
cat /etc/passwd
```
![Pasted image 20250331205930](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331205930.png)
由于存在后门用户，这里我们需要删除这个用户即可：
使用命令；
```
userdel -f bt
```
删除bt用户，这里使用的是强制删除
![Pasted image 20250331210249](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331210249.png)
删除用户后
要删除bt用户之后再去执行文件
这里的文件删除rm -rf 这一行
保存文件后，去执行之前的文件
![Pasted image 20250331210349](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250331210349.png)
尝试修复nacos并且自行用poc测试是否成功
这里找不到网站，网上看了大牛的wp，使用扫描器扫出来了
1、弱口令 2、未授权 3、SQL注入 4、认证绕过
```
pyc反编译
https://www.lddgo.net/string/pyc-compile-decompile
```