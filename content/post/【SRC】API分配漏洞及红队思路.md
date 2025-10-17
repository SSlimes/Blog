---
title: 【SRC】API分配漏洞及红队思路
categories:
  - SRC
date: 2025-10-15
image: images/102.webp
---
# 案例：xxx馆通
**1：点击我的——授权用户信息——手机号一键登录**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133823.png)
**观察数据包**
**2：观察数据包，根据findOpenid得知，它是寻找该用户的openid,另外还得到了session_key**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133858.png)
**3:观察第二个数据包为登录数据包，小程序拿到openid后,用它登录进去，然后返回了access_token**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017133934.png)
## 思路1：
**在openid处输入admin，尝试返回管理员的access_token**
### 做法：
**将openid后面的值改为admin,发包，如果成功，说明就是**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134031.png)
**这里除了admin外，其他的都可以获得access_token**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134056.png)
## 思路2：
### 做法
**观察下面的数据包得到了encryptData和iv,至此我们已经得到了登录三要素，在获得网址的手机号信息之后可以实现任意用户登录**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134142.png)
## 思路3：
猜测路径，访问敏感数据
**1：随意点击下面的几个按钮**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134233.png)
**2：查看数据包**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134255.png)
**3：试图将上面的接口中的userResult改为list,如果post不行，就改为get请求方式**
**（它提示需要管理员权限，说明有接口，但缺乏一些东西）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017134316.png)
**4:如果上面的登录数据包中得到了admin的access_token，就可以把authorizaton的值给替换掉，就可以成功看到所有信息了**