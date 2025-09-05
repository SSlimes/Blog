---
title: 【知攻善防靶场】web3
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/38.webp
---
# 前提须知
```go
前景需要：小苕在省护值守中，在灵机一动情况下把设备停掉了，甲方问：为什么要停设备？小苕说：我第六感告诉我，这机器可能被黑了。
这是他的服务器，请你找出以下内容作为通关条件：
1.攻击者的两个IP地址
2.隐藏用户名称
3.黑客遗留下的flag【3个】
```
**虚拟机账号/密码**：
```go
用户：administrator
密码：xj@123456
```
登录到系统之后：
```go
看到有小皮，看一下是否存在后门文件，但未发现有用的信息，看来一下也并非后门文件。
```
看到有小皮，看一下是否存在后门文件，但未发现有用的信息，看来一下也并非后门文件。
![Pasted image 20250301200918](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200918.png)
# 1. 隐藏用户
```go
当我们去检查是否有隐藏用户时发现一个hack6618$的隐藏管理员账户。
```
![Pasted image 20250301200937](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301200937.png)
```go
答案：hack6618$
```
# 2. 攻击者两个IP
```go
先不管账号，先看一下apache的日志文件，是否有异常。
根据日志可以看出，有两个IP访问频率很高，都是一些注入信息。
所以能确定的是这两个IP为攻击IP。
192.168.75.129
192.168.75.130
```
![Pasted image 20250301201014](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201014.png)
![Pasted image 20250301201022](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201022.png)
```go
根据日志可以看出
<http://127.0.0.1/zb_system/login.php>  为后台管理页面

攻击者后面一直访问 /zb_system/cmd.php?act=verify打开网站发现有登录失败的提示，攻击者可能是尝试爆破。
```
![Pasted image 20250301201045](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201045.png)
![Pasted image 20250301201054](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201054.png)
![Pasted image 20250301201104](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201104.png)
```go
继续看日志，发现攻击者使用了admin用户成功登陆进了系统
```
![Pasted image 20250301201122](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201122.png)
```go
尝试测试一下啊
<http://127.0.0.1/zb_system/admin/index.php?act=admin>
发现没有权限，目前仅仅只是访客状态。
```
![Pasted image 20250301201134](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201134.png)
```go
答案：双IP：
192.168.75.129
192.168.75.130
```
# 3. 三个flag
## flag1
```go
登录hack6618$用户看一下是否有什么信息，密码自己改一下即可
```
![Pasted image 20250301201200](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201200.png)
```go
登录到系统，找到了一圈发现下载里有一个system.php文件，打开发现一个flag。
flag{888666abc}
```
![Pasted image 20250301201214](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201214.png)
```go
答案：flag{888666abc}
```
## flag2
```go
顺着思路找一下,下一步看看有没有自启动项或者计划任务什么的
cmd输入Taskschd.msc
```
![Pasted image 20250301201247](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201247.png)
![Pasted image 20250301201258](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201258.png)
```go
成功找到flag
答案：flag{zgsfsys@sec}
```
## flag3
```go
因为是z-blog博客系统，可以找一下重置密码插件，重置一下密码

在 Z-Blog官网找到密码找回工具（免密登录）
#官网文章链接
<https://bbs.zblogcn.com/thread-83419.html>
#工具下载地址
<https://update.zblogcn.com/tools/nologin.zip>
```
```go
下载解压后将 nologin.php文件放到网站根目录下,直接访问即可。
```
![Pasted image 20250301201333](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201333.png)
```go
重置完成后，发现黑客创建了一个账号
```
![Pasted image 20250301201350](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201350.png)
```go
点击用户编辑，发现flag
```
![Pasted image 20250301201411](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250301201411.png)
```go
答案：flag{H@Ck@sec}
```
# 全部答案：
```go
192.168.75.129
192.168.75.130
hack6618$
flag{888666abc}
flag{zgsfsys@sec}
flag{H@Ck@sec}
```