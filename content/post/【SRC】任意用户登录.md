---
title: 【SRC】任意用户登录
categories:
  - SRC
date: 2025-10-21
image: images/104.webp
---
# 0x00 实现原理
将生成的openid改为别人的。
# 0x01 案例
xxx校友资源服务平台
# 0x02 复现
**1：输入用户名和验证码登录**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024121926.png)
**观察一键登录的数据包**
**2：第一个数据包是获得手机号，生成openid**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024121955.png)
**3：第二个数据包就是获得手机号和openid,进行登录**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122020.png)
**根据观察的数据包，构想渗透思路**
**4：只需获得用户的手机号即可登录他人的账户**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122056.png)
**5：使用bp，查看与手机号相关的数据包即可**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122123.png)
**6：获得别人的手机号之后，进行base64加密，然后传入phone参数后面，得到该账号的openid**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122143.png)
**7:对该openid进行base64解码**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122155.png)
**8：重新登录，抓包，将该openid粘贴到对应数据包的位置，成功登录他人账户**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122210.png)
**不断点击forward，替换openid，发包即可**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122238.png)
**9：持续发包，成功登录他人账户**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024122255.png)