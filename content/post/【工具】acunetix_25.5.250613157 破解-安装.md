---
title: 【工具】acunetix_25.5.250613157 破解-安装
date: 2025-08-30
categories:
  - 工具
image: images/20.webp
---
# 前言：
Acunetix Premium 是一种 Web 应用程序安全解决方案，用于管理多个网站、Web 应用程序和 API 的安全。集成功能允许您自动化 DevOps 和问题管理基础架构。
# 更新内容：
```
改进
添加正则表达式以增强对 Django 应用程序中堆栈跟踪泄露的检测
改进了对使用弱密钥签名的 JWT 的检测
为暴露的 nginx.conf 和 .htaccess 文件添加了新的安全检查，以增强漏洞检测
添加了 LDAP 注入检测
增加了对 PII（个人身份信息）泄露漏洞的检测
JSON 响应中数据库连接字符串的新检测，以提高敏感数据暴露覆盖率
扫描仪已更新，支持从 Linux 使用 NTLM 身份验证扫描目标
更新了秘密令牌检测以增加覆盖范围
更新了 JSON 字段中的 DB 连接检测
更新了 DeepScan 以提取更多道具
添加了新的检查来检测原型污染（服务器端）
更新了 dompurify 以检测更多漏洞
更新了基于 dom 漏洞的 iframe 注入检测
更新了 XPath 注入以获得更好的覆盖范围了
修复了 Cleo Harmony/VLTrader/LexiCom RCE 检测的误报问题
修正了“Scripts\WebApps\drupal_3.script”中的版本比较逻辑

安全检查
为弱 ViewState 密钥添加了新的安全检查
添加了新的检查以检测 PAN-OS XSS ( CVE-2025-0133 )
添加了一项新检查，用于检测 Citrix NetScaler 内存泄露 (CitrixBleed 2) ( CVE-2025-5777 )
漏洞数据库（VDB）版本升级至20250708
更新了开放重定向以增加覆盖范围
为 API 添加了 JWT 身份验证绕过
添加了 SAP NetWeaver Visual Composer 无限制文件上传 ( CVE-2025-31324 )
增加了对 Craft CMS 远程代码执行 ( CVE-2025-32432 )的检测
添加了对缺失的 X-Content-Type-Options 标头的检查
检测 Craft CMS 远程代码执行漏洞 ( CVE-2025-32432 )
```
# 下载：
```
通过网盘分享的文件：Acunetix-v25.5.250613157.zip.apk
链接: https://pan.baidu.com/s/18gN5FnJ6KedX8ej1giI7pw 提取码: sk4s 
--来自百度网盘超级会员v4的分享
```
记得删除`.apk`,然后直接解压zip文件即可。
# 安装：
Download Zip File, password is on our post
建议： 安装完成后， 先登录一次账号在停止服务， 执行下面操作
在安装工具之前， 添加到 hosts 文件中 C:\Windows\System32\drivers\etc\hosts
```
127.0.0.1 erp.acunetix.com
127.0.0.1 erp.acunetix.com.
::1 erp.acunetix.com 
::1 erp.acunetix.com.

127.0.0.1 discovery-service.invicti.com 
127.0.0.1 discovery-service.invicti.com. 
::1 discovery-service.invicti.com
::1 discovery-service.invicti.com.

127.0.0.1 cdn.pendo.io 
127.0.0.1 cdn.pendo.io. 
::1 cdn.pendo.io
::1 cdn.pendo.io.

127.0.0.1 bxss.me 
127.0.0.1 bxss.me. 
::1 bxss.me
::1 bxss.me.

127.0.0.1 jwtsigner.invicti.com 
127.0.0.1 jwtsigner.invicti.com. 
::1 jwtsigner.invicti.com
::1 jwtsigner.invicti.com.

127.0.0.1 sca.acunetix.com 
127.0.0.1 sca.acunetix.com. 
::1 sca.acunetix.com
::1 sca.acunetix.com.

192.178.49.174 telemetry.invicti.com 
192.178.49.174 telemetry.invicti.com. 
2607:f8b0:402a:80a::200e telemetry.invicti.com 
2607:f8b0:402a:80a::200e telemetry.invicti.com.
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204308.png)
安装后， 让我们停止它的服务调用服务工具:(使用菜单或打开任务管理器， 转到服务选项卡)
- Acunetix
- Acunetix Database
管理员运行停止服务
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204338.png)
或者使用命令停止
```
net stop "Acunetix Supervisor" 
net stop "Acunetix Database"
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204407.png)
替换文件 wvsc.exe
```
C:\Program Files (x86)\Acunetix\25.1.250204093
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204427.png)
移动两个文件
license_info.Json 文件和 wa_data.dat 文件到
C:\ProgramData\Acunetix\shared\license 目录替换将 C:/ProgramData/Acunetix/shared/license/整个文件夹设置为只读
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204508.png)
安装完成后登录发现需要证书， 不能扫描， 先停止服务， 将 license 下文件全部删除， 复制license_info.Json 文件和 wa_data.dat 文件进去设置只读， 再次启动服务
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204529.png)
Now let's restart acunetix:
重启服务， 成功（再次启动 Acunetix 两个服务）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204559.png)
Acunetix
Acunetix Database
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204616.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250831204627.png)
Now login back to application, and you should be able to use it :)
Enjoy（完成）


