---
title: 【知攻善防靶场】web1
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/36.webp
---
# 前提须知
```go
小李在值守的过程中，发现有CPU占用飙升，出于胆子小，就立刻将服务器关机，并找来正在吃苕皮的hxd帮他分析，这是他的服务器系统，请你找出以下内容，并作为通关条件：

1.攻击者的shell密码
2.攻击者的IP地址
3.攻击者的隐藏账户名称
4.攻击者挖矿程序的矿池域名(仅域名)
5.有实力的可以尝试着修复漏洞
```
虚拟机账号/密码:
```go
用户：administrator
密码：Zgsf@admin.com
```
# 1. 攻击者的shell密码
Webshell后门检查：
这里推荐使用D盾/河马查杀来检查webshell后门。
登录系统后，找到web源码地址，上传D盾直接扫描web源码地址，可以看出扫出一个**shell.php**后门文件。
![Pasted image 20250301195404](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195404.png)
打开shell.php文件，查看源码，获取连接密码。
![Pasted image 20250301195428](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195428.png)
**答案：rebeyond**
# 2. 攻击者的IP地址
查看日志信息，确定攻击者的IP地址。
查看/分析 Apache日志可以看出攻击IP。
![Pasted image 20250301195511](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195511.png)
**答案：192.168.126.1**
# 3. 攻击者的隐藏账户名称
使用克隆工具，检查账号信息，确定是否存在未知管理员权限的用户信息。
![Pasted image 20250301195550](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195550.png)
**答案：hack168$**
# 4.攻击者挖矿程序的矿池域名
登录**hack168$用户**，看到一个Kuang.exe文件,发现是pyinstaller打包的exe文件,使用pyinstxtractor进行反编译。
```go
#pyinstxtractor项目地址：<https://github.com/extremecoders-re/pyinstxtractor>
挖矿病毒与反编译脚本放在一个文件夹
执行：python .\\pyinstxtractor.py .\\Kuang.exe
```
![Pasted image 20250301195647](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195647.png)
得到一个反编译好的文件夹，找到一个pyc的文件，Pytpyc文件是Python编译后的字节码文件，通常用于提高加载模块时的速度。
![Pasted image 20250301195728](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195728.png)
使用在线pyc反编译工具，得到源码
使用在线网站即可：
<https://www.lddgo.net/string/pyc-compile-decompile>
![Pasted image 20250301195814](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301195814.png)
**答案**：
**URL**：<http://wakuang.zhigongshanfang.top>
**域名**：wakuang.zhigongshanfang.top
# 5. 全部答案
```go
rebeyond
192.168.126.1
hack168$
wakuang.zhigongshanfang.top
```