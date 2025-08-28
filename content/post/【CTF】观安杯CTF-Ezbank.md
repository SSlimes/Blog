---
title: 【CTF】观安杯CTF-Ezbank
date: 2025-08-27
categories:
  - 其他
  - 蓝队
image: images/9.webp
---
# 题目解题
1、注册一个账户，登陆并添加一个账户
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183439.png)
2、通过转账功能，向ID为6的账户转账
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183456.png)
3、转账时抓包
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183505.png)
4、抓包后通过测试并发获取更多钱。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183524.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183530.png)
5、两个ID相互转账并多次并发，得到最后的金额购买最后一个礼物，得到flag。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828183546.png)
