---
title: 【SRC】phone参数值修改导致信息泄露
categories:
  - SRC
date: 2025-10-16
image: images/96.webp
---
# xxx助手（phone参数）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115436.png)
**接口***：已发货
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115455.png)
**使用bp进行动态劫持**
1：点击已发货，在bp的http history中查看包的信息
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115525.png)
2：选择一个包，查看详细信息
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115551.png)
3：发送至调试模块
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251017095358.png)
Openid:微信给小小程序发送的身份凭证
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115634.png)
4:依次删除里面的参数，点击send，查看reponse数据是否发生变化。最终确定是phone的参数对应决定数据变化
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115658.png)
比如在phone位置参数1，会收到大量信息（模糊搜索）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251016115732.png)