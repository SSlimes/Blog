---
title: 【src】某某大学-水平越权
date: 2026-03-23
image: /images/125.webp
categories:
  - SRC
---
# 漏洞描述
某某大学工会工作服务平台存在逻辑漏同，可利用逻辑漏洞登陆任意用户任意账号，可获取大量校内人员姓名，身份证号，出生年月，手机号，校内职务，并且可以对用户信息进行删除修改，申请工会等操作。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323114738.png)
# 漏洞详情
(1)平台允许任意用户注册账号:
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323114813.png)
(2)点击登录，抓包:
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323114906.png)
(3)拦截相应报文，直到出现下面的这个响应报文。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323115007.png)
(4)修改如下内容。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323115043.png)
(5)下一个报文也进行响应拦截。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323121033.png)
(6)再次出现这个报文。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323121058.png)
(7)然后继续修改
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323121130.png)
(8)修改完成后，一直放包即可，成功越权登录系统
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260323121236.png)
# 修复建议
建议联系网站管理员/网站开发相关单位，对登陆请求进行添加鉴权验证，以免攻击者恶意利用登入账号进入系统，造成大量个人信息泄露。
