---
title: 【渗透测试】CORS 配置不当漏洞
categories:
  - 渗透测试
date: 2025-10-23
image: images/106.webp
---
# 0x00 漏洞级别
低危
# 0x01 漏洞及风险描述
```
攻击者可制作一个恶意 HTML 文件（如malicious.html），内容包含跨域 AJAX 请求，利用Origin: null的特性向目标服务器发送DELETE请求（例如删除用户数据）。当用户双击打开该本地文件时，请求会携带用户在目标服务器的登录凭证（如 Cookie），因服务器允许null Origin 跨域，请求会被执行，导致非预期操作。
```
# 0x02 漏洞复现
服务器允许 Origin: null 跨域访问（Access-Control-Allow-Origin: null），且开放了多种 HTTP 方法（DELETE、PUT、PATCH等有修改能力的方法）。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024141931.png)
# 0x03 修复建议
```
1、移除对 Origin: null 的允许，仅在 Access-Control-Allow-Origin 中配置可信域名白名单。
2、收紧 Access-Control-Allow-Methods，仅保留业务必需的方法（例如仅允许GET、POST、OPTIONS，移除DELETE、PUT等高危方法，除非明确必要）。
3、若需允许凭据（Allow-Credentials: true），务必确保Allow-Origin不包含null或*，仅指定具体可信域名。
```