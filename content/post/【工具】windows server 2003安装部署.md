---
title: 【工具】windows server 2003安装部署
date: 2025-08-28
categories:
  - 工具
image: images/5.webp
---
# 部署安装网址：
```
https://blog.csdn.net/weixin_45950429/article/details/107069404
```
# 下载地址：
```
通过网盘分享的文件：WindowsServer 2003.iso
链接: https://pan.baidu.com/s/1_VmXyNxI5kyouSAMjMl5wQ 提取码: 1234 
--来自百度网盘超级会员v2的分享
```
# 步骤一：
打开VMware,点击我的主页，点击创建新的虚拟机：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828151903.png)

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828151927.png)
这里选择稍后安装操作系统，然后点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828151941.png)
客户机操作系统选择“Microsoft Windows(W)"，版本的话，你下载的是什么版本你就安装什么版本，我最前面发的是Windows Server 2003 Standard Edition版本的额，也就是32位的。然后点击”下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152000.png)
虚拟机名称，我这里就默认了，位置一定要自己选择，把它放在内存空间充足的磁盘。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152011.png)
这里你可以默认40GB，我避免要做的东西多，所以设了“60GB”，下面选择“将虚拟磁盘拆分成多个文件（M），然后点击下一步
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152023.png)
点击自定义硬件，里面的打印机极少用，可以选择移除，然后点击CD，浏览放入映像文件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152036.png)
点击关闭之后会自动回到这页，点击“完成”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152046.png)
完成之后就会生成一个虚拟机
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152057.png)
# 步骤二：
放入镜像之后的安装过程
点击“开启此虚拟机",随后就会进入这个页面，开始等待
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152148.png)
点击"Enter"键进行安装，再等待
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152156.png)
点击选择”用NTFS文件系统格式化磁盘分区（快），随后点击"Enter"键继续
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152210.png)
安装格式化，等待
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152224.png)
等待
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152233.png)
出现这个画面，点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152243.png)
点击我接受这个协议，点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152255.png)
继续等待，出现这个画面，点击“下一步”，自定义输入名称，点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152309.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152316.png)
这里服务器修改为“500”，然后点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152326.png)

一定记得设置自己记得住的密码，等一下安装好后开机需要使用
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152334.png)
默认点击下一步，等待
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152342.png)
出现这个画面，点击“典型设置”，点击“下一步”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152352.png)
选择“不，此计算机不在网络上，或者再没有域的网络上，把此计算机作为下面工作组的一个成员（W)", 点击”下一步“
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152402.png)
# 步骤三：
出现这个画面时，点击"完成”，随后点击“是”
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152412.png)
“x"掉
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152421.png)
在 开始–我的电脑–属性-自动更新–点击关闭自动更新(T)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152431.png)
# 完成
你可以在桌面点击右键–属性–设置–修改分辨率即可修改屏幕大小
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828152440.png)
结束完成！！！
