---
title: 【SRC】Druid monitor页面之未授权登录
date: 2026-01-21
categories:
  - SRC
image: /images/114.webp
---
# 方式1
如果弱口令登陆不进去，可以尝试拼接url，登录进去
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111322.png)
```
/druid/index.html
/druid/Login.html
/prod-api/druid/Login.html
/prod-api/druid/index.html
/dev-api/druid/login.html
/dev-api/druid/index.html
/api/druid/Login.html
/api/druid/index.html
/admin/druid/Login.html
/admin-api/druid/login.html
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111348.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111355.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111359.png)
# 方式2
把/druid/login.html删除掉
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111425.png)
然后点击查看数据库连接，成功进去该界面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111445.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111450.png)
# 方式3
将login改为index
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111509.png)
登录成功
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260121111521.png)