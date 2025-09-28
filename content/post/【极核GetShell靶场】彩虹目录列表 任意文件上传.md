---
title: 【极核GetShell靶场】彩虹目录列表 任意文件上传
date: 2025-09-27
categories:
  - 靶场
image: images/87.webp
---
# 靶机信息
可以通过访问极核官方靶场开启靶机实验：[极核靶场](https://hackhub.get-shell.com/) -> 渗透测试靶场 -> 彩虹目录列表
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928150921.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928150943.png)
# 靶机实战
首先访问靶机首页，可以找到后台管理，说明存在后台。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151008.png)
第一种获取账号密码的方式：访问题目信息给出的Github源码地址，默认账号密码已经写在README
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151207.png)
第二种方式可以通过对登录请求的数据包进行密码爆破。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151222.png)
登录成功后返回首页，可以上传文件。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151309.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151327.png)
其中从GITHUB可以得知这是个PHP程序，尝试直接上传PHP木马，这里使用[冰蝎WebShell工具](https://get-shell.com/1537.html)的WebShell木马进行上传。可以发现并未限制PHP文件的上传。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151352.png)
使用冰蝎对上传的木马进行连接，可以连接成功，并且可以读取文件，读取根目录Flag文件即可获得Flag。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151439.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250928151625.png)
flag:
```
GetShell{Hanasaki-amenmachi}
```