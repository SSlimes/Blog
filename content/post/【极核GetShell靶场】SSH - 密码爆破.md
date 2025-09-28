---
title: 【极核GetShell靶场】SSH - 密码爆破
date: 2025-09-28
categories:
  - 靶场
image: images/86.webp
---
# 前言
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928112134.png)
# 漏洞科普
SSH爆破攻击是一种通过自动化工具（如Hydra、Medusa等）对SSH服务进行大规模密码猜测的攻击手段，其核心原理是利用预先设定的用户名和密码字典或暴力生成的字符组合，对目标服务器的SSH端口（通常是22端口）发起高频次登录尝试，直到找到正确的凭据。攻击者常采用分布式策略，通过多IP并发请求绕过单点封禁限制，并利用弱密码或默认账户（如root、admin）进行针对性破解。
这种攻击的危害主要体现在三个方面：
1. ​**​系统安全风险​**​：一旦成功，攻击者可完全控制目标服务器，执行恶意操作（如窃取数据、植入后门或勒索软件），甚至将服务器纳入僵尸网络用于DDoS攻击或加密货币挖矿。
2. ​**​资源消耗​**​：高频次登录尝试会占用大量CPU、内存和带宽资源，导致服务器性能下降甚至服务中断。
3. ​**​隐蔽性威胁​**​：大量失败登录日志可能掩盖真实攻击事件，而成功入侵后植入的隐藏计划任务（如定时反弹Shell）会长期维持权限，进一步扩大攻击面。
# 靶机实战
首先开启实例，获取入口，获取的地址+端口为**SSH服务**，并非WEB服务！
在Kali（Linux）下可以使用 **`hydra`** 工具进行服务密码爆破，通过靶机信息已知默认用户为 **`user`**，使用如下命令进行服务密码爆破，可以获取到密码为：**`123456`**
```php
# 示例
hydra -l user -P /home/slimer/桌面/rockyou.txt ssh://node.hackhub.get-shell.com:59313
# 使用方法
hydra -l <用户名> -P <字典路径> <协议头>://<目标地址和端口>
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928112640.png)
在Windows上可以借助 [无影TscanPlus](https://get-shell.com/2388.html) 工具中的密码破解模块进行服务密码爆破
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928112732.png)
修改SSH端口号，将地址填入后即可开始爆破，可以获取到密码为：**`123456`**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928113148.png)
使用题目信息给出的用户名和爆破获取到的密码：**`user / 123456`** 进行SSH登录。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928113005.png)
登录后为普通用户，然后使用 **`sudo -i`** 进行提权后，查看根目录 flag文件 即可获取flag。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928113105.png)
flag:
```
GetShell{d433d242-ac32-4c56-93b8-5c00fe0d8b25}
```