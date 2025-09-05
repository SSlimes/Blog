---
title: 【玄机靶场】第五章 Windows 实战-evtx文件分析
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/46.webp
---
# 简介
点击下载附件获取附件
任务环境说明
注：样本请勿在本地运行！！！样本请勿在本地运行！！！样本请勿在本地运行！！！
应急响应工程师在收到设备告警后，在受到攻击的服务器保存了一份log 请你协助分析 LOG 文件提交对应的 FLAG。
# 靶场
三个日志文件进行分析：
![Pasted image 20250320094426](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320094426.png)
# 过程
## flag1
**1.将黑客成功登录系统所使用的IP地址作为Flag值提交；**
查看登录成功事件(ID:4624)
点击筛选事件ID 4624即可；
![Pasted image 20250320111846](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320111846.png)
**flag:**
```
flag{192.168.36.133}
```
## flag2
**2.黑客成功登录系统后修改了登录用户的用户名，将修改后的用户名作为Flag值提交；**
**事件 ID**: 4738
直接事件查看器即可查看
![Pasted image 20250320111936](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320111936.png)
```
flag{Adnimistartro}
```
## flag3
**3.黑客成功登录系统后成功访问了一个关键位置的文件，将该文件名称（文件名称不包含后缀）作为Flag值提交；**
筛选 事件 ID 4663
![Pasted image 20250320112102](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320112102.png)
flag:
```
flag{SCHEMA}
```
## flag4
**4.黑客成功登录系统后重启过几次数据库服务，将最后一次重启数据库服务后数据库服务的进程ID号作为Flag值提交；**
直接进应用程序.evtx文件里找就行
![Pasted image 20250320113111](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320113111.png)
flag
```
flag{7036}
```
## flag5
**5.黑客成功登录系统后修改了登录用户的用户名并对系统执行了多次重启操作，将黑客使用修改后的用户重启系统的次数作为Flag值提交。**
题目说黑客登录系统后修改了登录用户并且进行了多次重启，提交黑客修改用户重启的次数，这里我们需要在系统日志里面进行查看，查看**事件ID 1074**；
![Pasted image 20250320131525](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320131525.png)
flag:
```
flag{3}
```
# 事件ID:
**事件 ID 4624** - 成功的账户登录
**事件 ID 4625** - 登录失败
**事件 ID 4634** - 用户注销
**事件 ID 4647** - 用户主动注销
**事件 ID 4720** - 用户账户已创建
**事件 ID 4722** - 用户账户已启用
**事件 ID 4725** - 用户账户已禁用
**事件 ID 4726** - 用户账户已删除
**事件 ID 4670** - 权限服务状态变更
**事件 ID 4719** - 系统审计策略已更改
**事件 ID 6005** - 事件日志服务启动
**事件 ID 6006** - 事件日志服务停止
**事件 ID 4672** - 特权服务已分配
**事件 ID 4673** - 特权服务已请求
**事件 ID 7036** - 服务已更改状态
**事件 ID 1074** - 系统关机,重启或注销