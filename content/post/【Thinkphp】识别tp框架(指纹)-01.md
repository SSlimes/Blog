---
title: 【Thinkphp】识别tp框架(指纹)-01
date: 2025-08-31
categories:
  - Web攻防
image: images/22.webp
---
# 1. 前言
服务框架是指某领域一类服务的可复用设计与不完整的实现， 与软件框架不同的是， 服务框架同时体现着面向服务， 一个服务框架可以分为两个主要部分： 服务引擎、 引入的外部服务。 ThinkPHP， 是为了简化企业级应用开发和敏捷WEB应用开发而诞生的开源轻量级PHP框架。 可想而知框架连接着网络和系统接触着越来越多的关键数据， 渐渐成为单位公共安全中最具有战略性的资产， 框架的安全稳定运行也直接决定着业务系统能否正常使用。 如果框架被远程代码执行攻破， 这些信息一旦被篡改或者泄露， 轻则造成企业经济损失，重则影响企业形象，甚至行业、社会安全。可见，数据库安全至关重要。
ThinkPHP是一个快速、 兼容而且简单的轻量级国产PHP开发框架， 诞生于2006年初， 原名FCS， 2007年元旦正式更名为ThinkPHP， 遵循Apache2开源协议发布， 从Struts结构移植过来并做了改进和完善， 同时也借鉴了国外很多优秀的框架和模式， 使用面向对象的开发结构和MVC模式， 融合了Struts的思想和TagLib（ 标签库） 、 RoR的ORM映射和ActiveRecord模式。 ThinkPHP可在Windows和Linux等操作系统运行， 支持MySql， Sqlite
和PostgreSQL等多种数据库以及PDO扩展， 是一款跨平台， 跨版本以及简单易用的PHP框架。
# 2. 识别tp框架（指纹）
## 2.1 icon判断
/favicon.ico
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022520.png)
## 2.1 报错
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022542.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022555.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022603.png)
## 2.3 错误传参
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022629.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022635.png)
## 2.4 特殊指纹出现logo
/?c=4e5e5d7364f443e28fbf0d3ae744a59a
/4e5e5d7364f443e28fbf0d3ae744a59a
/4e5e5d7364f443e28fbf0d3ae744a59a-index.html
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022713.png)
## 2.5 body特征
body里有"十年磨一剑" 或者"ThinkPHP"
## 2.6 插件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250901022824.png)


