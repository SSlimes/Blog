---
title: 【SRC】访客预约敏感信息泄露
date: 2026-02-24
categories:
  - SRC
image: /images/118.webp
---
# 案例一
在小程序处进行测试
**在填写信息处存在上传图片，可以尝试Xss。**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224125945.png)
**点击进去，在bp的历史记录里抓包**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130105.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130116.png)
**在bp抓的数据包中存在经典的id号，（水平越权），查看别人数据**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130141.png)
**修改id值，可以查看别人的访客数据**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130206.png)
# 案例二
**填完数据之后，抓包依然存在id**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130246.png)
**点击进去，查看数据包，修改id数值**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130316.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130346.png)
# 案例三
**点击访客进入**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130411.png)
**再点击进校来访**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130429.png)
**点击搜索，将name的数值删除为空**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130529.png)
# 案例四
**点击添加进校人员**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130830.png)
**在上传图片位置，上传一张图片**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130851.png)
**查看数据包得知它的上传路径，根据特征得出它上传到云里面了**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130907.png)
**在浏览器里进行访问，然后一步一步的删除内容**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130935.png)
**删到一定程度，便会显示所有内容**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224130958.png)
**访问路径，敏感信息拿到**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260224131026.png)
# 总结
以上4个案例都是基于修改ID参数信息进行的越权操作。