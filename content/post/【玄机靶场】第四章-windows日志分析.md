---
title: 【玄机靶场】第四章-windows日志分析
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/39.webp
---
# 简介
服务器场景操作系统 Windows7
服务器账号密码：winlog/winlog123
连接端口为：ip:3389
按照题目提示可以根据系统功能分析，或桌面工具进行辅助分析
注意：远控软件内IP为虚拟IP，如在进行进程中没有找到相关外连，应该是由于连接超时造成的断开了，重启环境服务器或软件即可继续对外发起请求，请见谅
注意：题目中shell如需在本地分析，提前关闭杀毒软件，会被杀掉，非免杀
注意：winlog用户在操作关于系统权限功能时，一定要使用管理员权限打开工具再去执行
如：cmd直接打开则可能无法进行操作系统权限性操作，需右击cmd-使用管理员权限打开，才可以，其它工具也如此
# 题目描述
  某台Windows服务器遭到攻击者入侵，管理员查看发现存在大量rdp爆破的请求，攻击者使用了不同位置的IP(此处模拟)，进行爆破并成功，并成功进入了系统，进入系统后又做了其它危害性操作，请根据各题目完成填写
题目来源公众号  州弟学安全
```
文档：https://mp.weixin.qq.com/s/eJpsOeaEczcPE-uipP7vCQ
```
# 靶场开启
![Pasted image 20250321201520](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321201520.png)
# 步骤
## flag1
1. **审计桌面的logs日志，定位所有扫描IP，并提交扫描次数**
直接把access.log文件拉出来，放kali中或者任意linux系统中查一下即可。
```
awk '{print $1}' access.log | sort | uniq -c | sort
```
![Pasted image 20250321203836](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321203836.png)
flag:
```php
flag{6385}
```
## flag2
2. **审计相关日志，提交rdp被爆破失败次数**
Win+R打开，eventvwr.msc
打开日志：
![Pasted image 20250321204216](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321204216.png)
然后根据要求进行筛选，事件ID为4625，可以看到登录失败次数为2594次
**登录失败日志ID为4625**
![Pasted image 20250321205126](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321205126.png)
也可以使用工具进行查看
![Pasted image 20250321210158](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321210158.png)
flag
```
flag{2594}
```
## flag3
3. **审计相关日志，提交成功登录rdp的远程IP地址，多个以&连接,以从小到大顺序排序提交**
事件ID筛选为4648，表示用使用凭据登录
![Pasted image 20250321211501](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321211501.png)
然后到处文件，放FullEventLogView软件中
找到一个**192.168.150.1**
![Pasted image 20250321211545](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321211545.png)
第二个：**192.168.150.128**
![Pasted image 20250321211621](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321211621.png)
第三个：**192.168.150.178**
![Pasted image 20250321211652](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321211652.png)
flag:
```
flag{192.168.150.1&192.168.150.128&192.168.150.178}
```
## flag4
4. **提交黑客创建的隐藏账号**
关于隐藏账号在用户组中可以查看到，快捷键WIN+R输入 '**lusrmgr.msc**'，看到用户中hacker$以及属性及所属的组
![Pasted image 20250321212302](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321212302.png)
```
flag{hacker$}
```
## flag5
5. **提交黑客创建的影子账号**
影子账号真实环境中是无法在用户组/netuser/用户面板中看到，但是可以在注册表中看到并删除，快捷键WIN+R '**regedit**'
```
注册表地址：HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\Names
```
下图为`hackers$`对应的权限值/组信息/映射关系及用户相关目录，操作删除后将不存在此用户信息
![Pasted image 20250321212524](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321212524.png)
或者直接使用D盾即可
![Pasted image 20250321212752](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321212752.png)
flag
```
flag{hackers$}
```
## flag6
5. **提交远程shell程序的连接IP+端口，以IP:port方式提交**
在应急响应中应排查对外连接，这一步是必不可少的，使用netstat -nao查看到相应的端口状态，在后面看到可疑连接
![Pasted image 20250321213718](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321213718.png)
由内对外连接，可拿着这个IP去查询到地区为国外的IP，如果说你没看到这个对外连接，说明连接超时了，因为对方本身就没开放这个端口，可重启环境后再次查看到，本身就为了模拟哈
玄机包括实战中可看到不少的对外连接，这时候怎么排查呢，以玄机为例
![Pasted image 20250321213406](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321213406.png)
可看到对外连接不少的互联网IP，排查思路如下
```
1. 根据外联IP地址进行排查，在情报平台进行查询
2. 根据端口进行排查，通常大端口或有特殊意义端口要确认
3. 依次根据PID进行排查，这个下面会讲到
```
flag:
```
flag{185.117.118.21:4444}
```
## flag7
各位应该发现了，开机就外联了，这个时候我们应该联想到自启动，一般排查以下
```
1. C:\Users\winlog\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
快捷命令：WIN+R shell:startup 将预自启动程序放入目录，会自启
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
注册表自启动：快捷键：WIN+R regedit 将绝对路径下程序进行字符串值保存会开机自启
搜索计划任务，进入任务计划程序，查看相关启动程序
```
当然了，在排查这些之前，我们需要知道在跑的程序是哪个，已知PID为**3676**(**注意：此处如果突然没有外连，则是超时，需重启环境，这个没办法，不能做到真上线**)
![Pasted image 20250321214034](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321214034.png)
```
tasklist | findstr "3676"
```
使用以上命令查看到启动的文件为xiaowei.exe，但是不知道绝对路径如何处置
![Pasted image 20250321213835](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321213835.png)
通过以上命令可获取PID的执行文件绝对路径，看到目录在
```
wmic process get name,executablepath,processid | findstr 3676
```
这里不知道为什么恶意软件断了，**PID也变了，打的时候换成自己查到的PID即可**
![Pasted image 20250321214749](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321214749.png)
绝对路径为：
```php
C:\Windows\system64\systemWo\xiaowei.exe
```
当然了，我们也可以使用 **netstat -naob**查看进程的启动程序和端口，记得用管理员CMD才行。
![Pasted image 20250321215047](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215047.png)
但是如需查看绝对路径的话还是按照上方方法，这里只是提供多一个方法排查参考
此时，我们继续按照上面思路排查自启动问题，看到自启动方法为注册表开机自启
![Pasted image 20250321215129](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215129.png)
将xiaowei.exe文件拖出来，拿到ida或者微步沙箱去看一下，明显的木马特征和Cobalt特征
![Pasted image 20250321215333](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215333.png)
![Pasted image 20250321215327](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215327.png)包括到最后面的网络行为特征也能看到外联IP地址、
![Pasted image 20250321215458](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215458.png)
flag
```
flag{xiaowei.exe}
```
## flag8
8. **黑客使用了计划任务来定时执行某shell程序，提交此程序名字**
上面已经说了关于计划任务的一些排查思路，按照实战中，攻击队或黑客为了权限维持，不会只放一个远控工具，一般会埋雷进行启动计划任务，根据上方思路排查到，计划任务程序中存在的计划
使用compmgmt.msc打开计算机管理
![Pasted image 20250321215855](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215855.png)
然后查看任务计划程序
![Pasted image 20250321215925](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321215925.png)
可以看出触发时间
![Pasted image 20250321220004](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321220004.png)
点进去查看执行的程序，可看到执行绝对路径
![Pasted image 20250321220047](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321220047.png)
查看这个bat脚本，最后得到执行全过程，确认下载了xiaowei.exe文件到相关目录，最后每次开机自启xiaowei.exe文件上线
![Pasted image 20250321220204](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250321220204.png)
flag:
```
flag{download.bat}
```