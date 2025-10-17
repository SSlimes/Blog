---
title: 【SRC】微信一键登录之session key三要素
categories:
  - SRC
date: 2025-10-17
image: images/100.webp
---
# xx招聘
**1：点击微信登录——一键关联手机号**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131659.png)
**2：依次点击数据包，发现session key三要素**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131732.png)
**3：有以下三要素**
```
sessionkey
encrytedData
iv
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131827.png)
**4:打开wx_sessionkey_crypt.exe工具**
**5：在之前的操作中我们已经拿到了后台，获得了大量的手机号**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131852.png)
**6：使用工具将三要素输入进去，点击解密**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131910.png)
**7：把手机号替换成被别人的，然后加密**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017131921.png)