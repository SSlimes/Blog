---
title: 【资产收集】ENScan_Go-信息收集工具
date: 2025-09-22
categories:
  - 红队
  - 渗透测试
  - 工具
image: images/82.webp
---
# 0x00 项目地址
项目为狼组团队的师傅开发，废话不多说，以下是项目地址：
```
https://github.com/wgpsec/ENScan_GO
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923121249.png)

我的是windows系统，直接下载这个即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923121310.png)
文件下载解压仅有exe文件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923121420.png)
# 0x01 使用
初次使用需要初始化
```
.\enscan-v1.3.1-windows-amd64.exe -v
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923121505.png)
生成`config.yaml`配置文件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923121551.png)
以爱企查为例导入cookie
```
链接：https://aiqicha.baidu.com/  
（注意：默认查询为aiqicha.baidu.com，请勿使用 aiqicha.com）
```
这里最好使用抓包工具进行抓取：
爱企查的cookie
![](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923122433.png)
天眼查的cookie
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923122924.png)
然后直接运行，即可！
```
 .\enscan-v1.3.1-windows-amd64.exe -n "汉堡王（上海）餐饮有限公司"
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250923124222.png)
