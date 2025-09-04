---
title: 【Thinkphp】漏洞利用工具-thinkphp_gui_tools
date: 2025-09-04
categories:
  - Web攻防
  - 红队
  - 靶场
image: images/33.webp
---
# 介绍
ThinkPHP漏洞综合利用工具, 图形化界面, 命令执行, 一键getshell, 批量检测, 日志遍历, session包含,宝塔绕过。
# 下载地址
```
通过网盘分享的文件：ThinkPHP.jar
链接: https://pan.baidu.com/s/1eoU2Lf-ldHhy9W2jx8sRpA 提取码: bn8b 
--来自百度网盘超级会员v4的分享
```
# 项目地址
```
https://github.com/bewhale/thinkphp_gui_tools
```
本项目是采用 JDK8 + javafx 开发的 ThinkPHP 图形化综合利用工具， 参考了其他大佬项目的部分代码。 JDK8可以直接运行，JDK11 因为去除了javafx这个依赖，需要自己再加上参数加入模块
```
java -Dfile.encoding="UTF-8" --module-path "C:\Program Files\Java\javafx-sdk-11.0.2\lib" --add-modules "javafx.controls,javafx.fxml,javafx.web" -jar "xxx.jar"
```
```
支持大部分ThinkPHP漏洞检测,整合20多个payload
支持部分漏洞执行命令
支持单一漏洞批量检测
支持TP3和TP5自定义路径日志遍历
支持部分漏洞一键GetShell
支持设置代理和UA
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904105315.png)