---
title: 【SRC】xx图书馆漏洞挖掘的过程
date: 2025-10-20
categories:
  - SRC
image: images/103.webp
---
# 0x00 资产收集
首先使用fofa收集一下信息：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111100.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111128.png)
# 0x01 复现
**1：首先查看findsomething里面的路径有哪些**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111216.png)
**2：访问某个特定的路径，将上面的网址复制到txt文本里面后，尝试访问这些路径**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111236.png)
**3：输入特定信息，查看是否返回信息(如果发现它是空白界面就可以尝试参数fuzz)**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111259.png)
**4：使用arjun对参数进行fuzz**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111328.png)
**5:获得以下参数**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111339.png)
**使用nmap探索它的系统，由查询结果得出它是windows的**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111414.png)
在 Nmap 中，参数 -Pn 的作用是‌完全跳过主机发现阶段‌，默认所有目标主机在线并直接进行端口扫描
**7：存在file可知，可能存在任意文件下载**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111503.png)
**8：使用bp进行抓包**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111524.png)
**9：在网上搜索一些windows的配置文件路径，放到txt文件里**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111558.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111611.png)
**10：使用爆破模块对指定位置file的参数值处进行fuzz(如果要加../../的话，直接在数据包里写更好一点)**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111639.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111653.png)
# 0x02 如何设置并发
**11：在资源池中设置请求数量50，然后延迟就设置为1**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111710.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111717.png)
**根据路径信息判断它存在上面漏洞：**
# 0x03 str2漏洞复现
**12:点击后缀名是jsp的文件路径**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111759.png)
**13：打开该漏洞的漏洞检测工具（根据后缀名是jsp可得）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111823.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111843.png)
**14：使用该工具检测发现存在漏洞**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020111905.png)
**如果该路径它存在该漏洞或许它的其他路径也是存在的，我们可以测试一下**
**15：使用工具检测其他路径，发现是存在漏洞的，（后缀名是action do jsp都可能存在）**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112034.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112044.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112114.png)
**16：如果存在waf,可以尝试给这些路径后面加#绕过**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112134.png)
# 0x04 文件上传漏洞
**17：进入该路径点击“文献提交”**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112214.png)
**18：根据编程java可知它可以上传jsp文件**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112224.png)
**19：点击上传文件——选择文件——bp抓包，一开始希望抓jsp的数据包，但是没上传过去**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112236.png)
**20：bp成功抓包，将文件名由jpg改为jsp，修改里面的内容，发包显示401**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112251.png)
#  0x05 fofa寻找c端检测
是否其他网址存在漏洞
**1:使用fofa语句寻找c端**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020112308.png)
**2：对ip地址进行微步解析，获得域名**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020114034.png)
**3：对域名进行模糊匹配**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251020114053.png)