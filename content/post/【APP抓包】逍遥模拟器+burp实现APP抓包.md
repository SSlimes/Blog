---
title: 【APP抓包】逍遥模拟器+burp实现APP抓包
categories:
  - 渗透测试
  - 工具
  - Web攻防
date: 2025-10-29
image: /images/111.webp
---
# 0x00 burp证书配置
步骤一：首先导出burp的CA证书
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135228.png)
步骤二：导出CA证书
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135241.png)
步骤三：选择`Certificate in DER format`格式
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135256.png)
保存到桌面
步骤四：然后打开kali虚拟机，用虚拟机.0结尾的文件
导入1.der进kali里面
```
openssl x509 -inform DER -in 1.der -out bp.pem
openssl x509 -inform PEM -subject_hash_old -in bp.pem | head -1
cat bp.pem > 9a5ba575.0
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135430.png)
步骤五：生成之后，拉出来备用。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135456.png)
# 0x01 逍遥模拟器配置
下载地址：
```
https://www.xyaz.cn/
```
版本选择：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029134705.png)
步骤一：系统设置中，需要把磁盘共享改成独立系统磁盘
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029134814.png)
步骤二：root模式打开。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029134840.png)
步骤三：进入设置连续点击**版本号**进入开发者模式
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029134915.png)
步骤四：进入开发者模式后需要将USB调试打开。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135020.png)
步骤五：然后进入该逍遥模拟器的安装文件夹
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135135.png)
步骤六：将**9a5ba575.0**文件拉到该目录
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135545.png)
输入以下命令：
```
adb.exe connect 127.0.0.1:21503
adb root
adb remount
adb push 9a5ba575.0 /system/etc/security/cacerts/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135638.png)
在模拟器的设置中找到信任的凭据，找到证书。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135818.png)
# 0x03 设置代理
这里最好是使用你的网卡地址，如果是无线就用无线。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135901.png)
这里和burp的地址保持一致即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251029135944.png)
就可以看到抓到模拟器的包了，相要抓到APP和模拟器的包下载微信和APP直接打开就行。