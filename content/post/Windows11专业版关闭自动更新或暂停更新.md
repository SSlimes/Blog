---
categories:
  - 其他
title: Windows11专业版关闭自动更新或暂停更新
date: 2025-11-17
image: /images/113.webp
---
# 修改注册表，延长暂停20年更新时间
通过修改注册表，我们可以突破系统默认的5周限制，**甚至将更新暂停到20年以后！
具体操作如下：
1，点击开始菜单，输入regedit或注册表编辑器，以管理员身份运行。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251117150249.png)
2，依次点击：
```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251117150410.png)
3，如果没有，右键空白处，选择新建 > DWORD（32位）值。
4，如果存在FlightSettingsMaxPauseDays ，直接修改；
5，将键值名称改为视频中显示的值，双击键值。
6，
```
新建：FlightSettingsMaxPauseDays
将数值数据改为00001c84，点击确定。
```
7，修改完成后，重启计算机使设置生效。  
8，重启后，打开**设置 > **Windows更新**，点击**暂停更新**，选择下拉箭头，你会发现已经突破了5周限制！将滚动条拉到底部，选择延长**1041周**，更新时间直接推到2045年！
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251117150917.png)