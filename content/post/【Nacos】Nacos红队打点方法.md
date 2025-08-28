---
title: 【Nacos】Nacos红队打点方法
date: 2025-08-28
categories:
  - 红队
  - Web攻防
image: images/10.webp
---
# 0x00 前置知识
## 1. 平台语法
```
hunter语法：
app.name="Nacos"
fofa语法：
title="nacos"
app="nacos"
```
端口：port="8848"
## 2. 信息收集
**资产少的情况下：**
如果没有明显特征，那就通过被动扫描器，如：burp的插件TsojanScan，HAE，logger++等
```
https://github.com/Tsojan/TsojanScan
```
**资产多的情况下：**
那我们就直接使用指纹识别工具EHole_magic
```
https://comm.pgpsec.cn/54.html
```
可以特定⼀些⽬录如nacos、/webroot/decision/login等等，进⾏更加精确的扫描进⾏精准的识别。（但网站收费）
**批量漏洞检测工具：**
NacosExploit
```
https://github.com/h0ny/NacosExploit
```
漏洞检测：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250704212254.png)
## 3. 漏洞利用点
Nacos 默认帐户名密码：
```
nacos/nacos
```
# 0x01 工具
反序列化漏洞利⽤⼯具
```
https://github.com/c0olw/NacosRce/releases/tag/v0.5
```
哥斯拉nacos后渗透插件
```
https://github.com/pap1rman/postnacos
```
**综合利⽤,且gui版本**
```
https://github.com/charonlight/NacosExploitGUI
```
