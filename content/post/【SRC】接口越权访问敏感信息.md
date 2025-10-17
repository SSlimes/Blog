---
title: 【SRC】接口越权访问敏感信息
date: 2025-10-17
categories:
  - SRC
image: images/98.webp
---
# xx大学教育培训中心
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120534.png)
1：登入进去获得cookie
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016120610.png)
2:点击某个包，然后复制host地址，在浏览器里进行访问
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094206.png)
3：点击进入管理平台
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094247.png)
4：输入信息，抓包，并查看它的返回包。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094311.png)
5：将code改为200,点击forward
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094338.png)
6:发现没成功
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094403.png)
7：使用findsomething获得路径
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094419.png)
8：使用bp批量跑
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094444.png)
使用小程序时它自带cookie
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094518.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094531.png)
10：取消URL编码
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094546.png)
11：开始批量扫描
**(先点击add,复制findsomething里面获得的路径，粘贴进去，然后点击paste即可）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094604.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094610.png)
12：点击特定接口,比如list,获得以下response信息
**（其他的信息都是客户端发送给服务器时，封装的数据）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094634.png)
13：打开资源管理器，复制特殊的js,然后用fofa搜索具有相同js信息的网站
![](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094652.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094720.png)
14：依次点击进去，全部是同一套系统
![](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094734.png)
15：然后测试相同接口，进行漏洞复现
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094803.png)
将上面的接口路径拼接到地址后面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017094959.png)
