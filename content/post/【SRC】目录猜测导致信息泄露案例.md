---
title: 【SRC】目录猜测导致信息泄露案例
categories:
  - SRC
date: 2025-10-17
image: images/97.webp
---
# xxx创业担保贷款（猜测目录名词）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115909.png)
1：点击下面的几个按钮，查看拦截包的特征，根据下面的blade，得知它使用的blade开发框架
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115931.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115954.png)
**接口**：我的申请，我的授权，个人认证
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120028.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120048.png)
**1：发送至调试模块，修改接口名称**
（以下是开发常用的接口）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120118.png)
**2：修改接口数**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120149.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120207.png)
**3：复制链接，浏览器访问**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120230.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120300.png)
**3：查看其他类似界面，进行漏洞复现**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120329.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120344.png)
