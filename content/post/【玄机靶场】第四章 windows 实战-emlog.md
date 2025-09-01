---
title: 【玄机靶场】第四章 windows 实战-emlog
date: 2025-09-01
categories:
  - 靶场
image: images/25.webp
---
# 简介
服务器场景操作系统 Windows
服务器账号密码 administrator xj@123456
题目来源公众号 知攻善防实验室 
https://mp.weixin.qq.com/s/89IS3jPePjBHFKPXnGmKfA
任务环境说明
    注：样本请勿在本地运行！！！样本请勿在本地运行！！！样本请勿在本地运行！！！
    应急响应工程师小王某人收到安全设备告警服务器被植入恶意文件，请上机排查
开放题目
    漏洞修复
参考
https://mp.weixin.qq.com/s/1gebC1OkDgtz4k4YtN10dg
# 靶机启动
![Pasted image 20250320132002](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320132002.png)
# 过程
## flag1
**通过本地 PC RDP到服务器并且找到黑客植入 shell,将黑客植入 shell 的密码 作为 FLAG 提交;**
上传D盾,直接扫就行.
![Pasted image 20250320132223](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320132223.png)
打开文件,发现默认密码
![Pasted image 20250320132347](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320132347.png)
flag
```
flag{rebeyond}
```
## flag2
**通过本地 PC RDP到服务器并且分析黑客攻击成功的 IP 为多少,将黑客 IP 作为 FLAG 提交;**
查看apache.log文件,发现写入了webshell.php文件.
```
log日志地址:C:\phpstudy_pro\Extensions\Apache2.4.39\logs
```
![Pasted image 20250320133630](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320133630.png)
flag:
```
flag{192.168.126.1}
```
## flag3
**通过本地 PC RDP到服务器并且分析黑客的隐藏账户名称,将黑客隐藏账户名称作为 FLAG 提交;**
直接使用工具,即可
![Pasted image 20250320134054](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320134054.png)
flag:
```
flag{hacker138}
```
## flag4
**通过本地 PC RDP到服务器并且分析黑客的挖矿程序的矿池域名,将黑客挖矿程序的矿池域名称作为(仅域名)FLAG 提交;**
找到hacker128用户的桌面,发现一个恶意的挖矿程序,然后进行反编译一下,得到flag
![Pasted image 20250320134714](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320134714.png)
![[Pasted image 20250320134737.png]]

![Pasted image 20250320134701](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320134701.png)
![Pasted image 20250320134647](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250320134647.png)
flag
```
flag{wakuang.zhigongshanfang.top}
```