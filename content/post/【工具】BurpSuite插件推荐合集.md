---
title: 【工具】BurpSuite插件推荐合集
date: 2025-09-24
categories:
  - 工具
image: images/83.webp
---
# 概述
BurpSuite是渗透测试人员常用的安全工具，它提供了一个功能**扩展**，也就是插件。在这个功能里面可以安装许多功能性插件，从而达到如虎添翼的效果。
但是BurpSuite的扩展存在一个问题，就是插件安装过多会导致**卡顿、内存溢出**等。所以插件的选择就显得十分重要！本文就笔者本人的经验，推荐一些不错的BurpSuite插件，达到即实用又兼顾性能的效果！
# 插件安装
在推荐插件之前，先讲述一下如何安装插件，我们一般下载的插件是一个扩展名为`.jar`的文件。
安装方式：打开BurpSuite -> 扩展 -> 添加 -> 选择文件 -> 安装
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924144437.png)
# 插件推荐
## HAE
Github下载：
```
https://github.com/gh0stkey/HaE/releases/tag/4.3.1
```
HAE可以发现HTTP数据包的相关重要信息，能够**有效减少**测试时间，将更多精力集中在**有价值且有意义**的报文上，从而**提高漏洞挖掘效率**。
安装完插件后，使用Burpsuite代理访问一些网页产生一些HTTP数据包，在BurpSuite模块代理 -> HTTP记录里面，即可发现一些被特殊标记的数据包！例如：**Shiro、Swagger UI、Ueditor、身份证、IP地址等等**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924145816.png)
## BurpAPIFinder
Github下载
```
https://github.com/shuanx/BurpAPIFinder/releases/tag/v2.0
```
攻防演练过程中，我们通常会用浏览器访问一些资产，但很多未授权/敏感信息/越权隐匿在已访问接口过html、JS文件等，通过该BurpAPIFinder插件我们可以：  
1、发现通过某接口可以进行未授权/越权获取到所有的账号密码、私钥、凭证  
2、发现通过某接口可以枚举用户信息、密码修改、用户创建接口  
3、发现登陆后台网址  
4、发现在html、JS中泄漏账号密码或者云主机的Access Key和SecretKey  
5、自动提取js、html中路径进行访问，也支持自定义父路径访问 …
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924150127.png)
## BurpFingerPrint
Github下载
```
https://github.com/shuanx/BurpFingerPrint/releases/tag/v1.5
```
攻击过程中，我们通常会用浏览器访问一些资产，该BurpSuite插件实现被动指纹识别+网站提取链接+OA爆破，可帮助我们发现更多资产。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924150459.png)
## RVScan
Github下载
```
https://github.com/XF-FS/RVScan/releases/tag/RVScan2.4
```
核心功能
- **📊 实时监控** – 扫描进度和结果实时显示
- **🔍 被动扫描** – 自动检测敏感路径和接口通过对访问接口递归对每一层路径进行路径探测，探测是否存在敏感信息漏洞接口
    - 💡 访问/admin/auth/api，递归探测会访问/*、/admin/*、/admin/auth/*、/admin/auth/api/*
    - 💡 一般可探测出如Swagger接口、登陆后台、漏洞路径、配置文件、spring env等泄露信息
- **🎯 指纹识别** – 支持关键词、favicon hash等多种识别方法
    - 💡 通过识别访问接口是否存在指定指纹，判断指纹信息，功能参考Ehole
- **🚀 路径绕过** – 内置多种绕过技术和payload
- **⚡ 多线程扫描** – 可配置并发数和速率限制
举个例子：比如在 **`https://get-shell.com/`** 这个网站下面有WEB备份文件 **`root.zip`**，那么当插件被动检测开启的时候，即可通过内置规则进行匹配，然后展示在VulDisplay界面。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924150627.png)
## TsojanScan
Github下载
```
https://github.com/Tsojan/TsojanScan/releases/tag/v1.4.6
```
TsojanScan是一个集成的BurpSuite漏洞探测插件，它会以最少的数据包请求来准确检测各漏洞存在与否，你只需要这一个足矣，其他的单个漏洞扫描插件就可以卸载了。包含了：**Nacos、SpringBoot、Log4j、Shiro、Fastjson、Weblogic、ThinkPHP、SQLI等主动/被动扫描。**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924150851.png)
## Autorize
下载方式：**BurpSuite -> 扩展 -> BApp商店 -> Autorize -> 安装**
autorize是一个可以发现未授权漏洞的工具，举个例子：有一个接口例如 **`https://get-shell.com/api/adduser`** ，这个接口的本意是留给管理员权限（高权限）使用，普通用户（低权限）无法使用，但是如果这个接口没有做鉴权处理，导致了低权限用户也可以使用，那么这就是一个**未授权访问漏洞**。
而这个插件就可以发现这个问题，**需要在插件页面的右面窗口放入低权限用户（未登录）状态下的`cookie`**或者点击**`Fetch Cookies Filters`**自动填充获取最近获得的Cookie，然后点击**`Autorize is off`**开启检测，插件会有三种状态：
- 绕过（Bypassed）！- 红色（可能存在未授权）
- 强制执行！- 绿色（不存在未授权）
- 强制执行???（请配置强制检测器） – 黄色
根据需要，重点查看**`绕过（Bypassed）！- 红色（可能存在未授权）`，如果这是一个高权限API或者一个高权限的页面，在去掉cookie后仍然可以访问，那么这就是一个未授权漏洞！
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924150950.png)
## xia_Yue (瞎越)
Github下载
```
https://github.com/smxiazi/xia_Yue/releases/tag/1.4
```
本质上与上面的Autorize是同样类型的插件，是一个可以发现未授权漏洞的工具，举个例子：有一个接口例如 **`https://get-shell.com/api/adduser`** ，这个接口的本意是留给管理员权限（高权限）使用，普通用户（低权限）无法使用，但是如果这个接口没有做鉴权处理，导致了低权限用户也可以使用，那么这就是一个**未授权访问漏洞**。
使用的时候需要在插件右面替换一下低权限的token。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250924151046.png)