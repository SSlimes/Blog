---
title: 【Web攻防】常见Web攻防登录框打法-01
date: 2025-08-28
categories:
  - Web攻防
  - 红队
image: images/12.webp
---
# 开局
首先先拿到一个站点：
```
whjwcm.cn
通过fofa，鹰图等等找资产：
domain="whjwcm.cn"
找到一个图文盒子后台登录页面：
https://wss.whjwcm.cn
```
这时候发现#号：
```
https://wss.whjwcm.cn/manage/#/login?redirect=%2Fplat
#/login?redirect=%2Fplat
```
# 思路一
## 0x01 进入后台首先尝试弱口令
一般就是最简单的admin
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630103827.png)

## 0x02 看数据包
### 修改error为0
通过抓包可以看到返回信息：
```
{
"error":1,
"msg":"\u7528\u6237\u540d\u6216\u5bc6\u7801\u4e0d\u6b63\u786e",
"data":""
}
一共三个参数，都可以试一下：

```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104121.png)

接触该请求，拦截一下相应信息对error进行修改。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104313.png)
这里可以看到：error和data数据都可以进行修改，然后进行测试。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104356.png)
发现进入了/admin/user/info，但Cookie认证是一个未知的，这里可以看出已经没什么用了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104448.png)
### 修改error为200
这里可以将error修改为200，试一下是否可以绕过。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104643.png)
发现还是不行，这里思路也没了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630104859.png)
### data进行测试
尝试再data参数中添加一下值，看一下是否可以绕过，
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630105024.png)
发现还是不行:
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630105124.png)
这时候就可以看一下js文件了，是否有关于这两个参数的代码逻辑问题。
### js文件查找参数
这里看出从js文件中找到了一个data对应的参数值，也是可以进行尝试的。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630105548.png)

但经过测试还是不行，但也是一个思路，后续遇到了可以进行尝试。
### 丢弃req数据包
通过劫持req的数据包，丢弃掉，有的时候也可以卡进去，但是这里发现，并不行，这时候这种思路也不行了。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630110131.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250630110246.png)

### 总结
```
这时候再尝试逻辑漏洞就不现实了，现在这种带#的这种漏洞就不多了。
```
# 思路二
如果遇到Cookie:admin-token=未知
如果遇到一个后台登录页面，抓包发现一个error返回参数为1，然后修改参数0，进行尝试发送后，遇到返回数据中有Cookie:admin-token=未知后，我们的思路是：
1. 找小程序的token/Cookie，抓取过去进行尝试
2. 找其他系统的弱口令进行尝试修改。
