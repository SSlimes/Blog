---
title: 【渗透测试】启用不安全的SSL协议
categories:
  - 渗透测试
date: 2025-10-24
image: images/107.webp
---
# 0x00 漏洞级别
中危
# 0x01 漏洞及风险描述
```
当前服务器启用了过时的 TLS 1.0 和 TLS 1.1 协议，这两个版本存在已知安全漏洞（如 BEAST、POODLE 攻击），已被业界建议淘汰。
```
# 0x02 漏洞复现
使用工具对网址进行扫描发现该证书开启了不安全的TLSv1.0和TLSv1.1。建议只开启TLSv1.2，关闭TLS1.0和TLS1.1。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251024142251.png)
# 0x03 修复建议
```
1、在 HTTPS 应用服务器配置中，关闭 TLS 1.0 和 TLS 1.1 这两个存在安全漏洞的协议版本，仅保留 TLS 1.2 及更高版本（如 TLS 1.3）
```