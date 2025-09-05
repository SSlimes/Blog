---
title: 【玄机靶场】第一章 应急响应- Linux日志分析
date: 2025-09-05
categories:
  - 靶场
  - 蓝队
image: images/42.webp
---
# 简介
```php
账号root密码linuxrz
ssh root@IP
```
1.有多少IP在爆破主机ssh的root帐号，如果有多个使用","分割
2.ssh爆破成功登陆的IP是多少，如果有多个使用","分割
3.爆破用户名字典是什么？如果有多个使用","分割
4.登陆成功的IP共爆破了多少次
5.黑客登陆主机后新建了一个后门用户，用户名是多少
# 开启靶场
![Pasted image 20250319212211](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319212211.png)
# 过程
## flag1
**1.有多少IP在爆破主机ssh的root帐号，如果有多个使用","分割**
首先肯定是先找到日志的位置，一般来说，SSH登录尝试会记录在 `/var/log/auth.log.1`（这是固定的）
接着那既然是爆破，那肯定会有很多失败的次数对吧？
逻辑基本就是上面这样，如果日志少一些那还好，可以一条条进行分析，日志多的话那可能还要进行筛选；
命令如下：
```php
cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
可以看出已给出三个地址：
![Pasted image 20250319212440](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319212440.png)
```php
root@ip-10-0-10-6:/var/log# cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
      4 192.168.200.2
      1 192.168.200.32
      1 192.168.200.31
```
flag：
```php
flag{192.168.200.2,192.168.200.31,192.168.200.32}
```
## flag2
**2.ssh爆破成功登陆的IP是多少，如果有多个使用","分割**
其实思路也很简单，那既然上面我们都已经得到三个IP了，最多的次数4次，不用想肯定最可疑啊，直接提交就是：
```php
cat auth.log.1 | grep -a "Accepted " | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
> 简单来说就是分析`auth.log.1`日志文件，提取出所有包含"Accepted "字符串的行，然后使用`awk`命令提取每行的第11个字段（通常这个字段表示远程IP地址），之后对这些IP地址进行排序和统计，最后按照数量的降序排列，并通过`more`命令分页显示结果。

这个命令链条通过以下步骤统计每个IP地址成功登录的次数：
1. 读取日志文件并筛选出成功登录的记录。
2. 提取记录中的IP地址。
3. 对IP地址进行排序、去重和计数。
4. 按登录次数降序排列并逐页显示结果。
![Pasted image 20250319212812](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319212812.png)
```php
root@ip-10-0-10-6:/var/log# cat auth.log.1 | grep -a "Accepted " | awk '{print $11}' | sort | uniq -c | sort -nr | more
      2 192.168.200.2
```
flag2
```
flag{192.168.200.2}
```
## flag3
**3.爆破用户名字典是什么？如果有多个使用","分割**
那做这这种的思路是什么？
那首先我们得了解什么是**爆破用户名字典**？
简单来说指黑客在进行暴力破解攻击时使用的一系列用户名列表。黑客通过自动化工具逐个尝试这些用户名，结合常见或默认密码，试图找到有效的登录凭据。这个过程被称为“字典攻击”或“暴力破解攻击”。
具体操作步骤；
```php
cat auth.log.1 | grep -a "Failed password" | perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort -nr
```
![Pasted image 20250319213127](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319213127.png)
```php
root@ip-10-0-10-6:/var/log# cat auth.log.1 | grep -a "Failed password" | perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort -nr
      5  invalid user user
      5  invalid user hello
      5  invalid user 
      4  root
      1  root
      1  root
      1  invalid user test3
      1  invalid user test2
      1  invalid user test1
```
flag3
```php
flag{user,hello,root,test3,test2,test1}
```
## flag4
**4.成功登录 root 用户的 ip 一共爆破了多少次**
> 这个就更简单了，其实也和上面第一题重复了，因为问题问：有多少IP在爆破主机ssh的root账号？这里又问成功登录 root 用户的 ip 一共爆破了多少次？而且前面我们在统计IP的时候就已经顺便把次数统计出来了，所以PASS，这里没什么好说的；

（命令还是第一题的命令，这里只是重复一下，不在多做解释）
```php
cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
![Pasted image 20250319213422](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319213422.png)
```php
root@ip-10-0-10-6:/var/log# cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
      4 192.168.200.2
      1 192.168.200.32
      1 192.168.200.31
```
flag4
```
flag{4}
```
## flag5
**5.黑客登陆主机后新建了一个后门用户，用户名是多少**
这个又是一个新的知识点，问我们黑客登陆主机后新建了一个后门用户，用户名是多少？
那这种我们怎么操作呢？（其实基本也就是这五个步骤）
**步骤1**：确定日志文件
通常与用户登录和用户管理活动相关的日志文件是 `/var/log/auth.log` 或其备份文件如 `/var/log/auth.log.1`。
**步骤2**：搜索创建用户的关键字
使用 `grep` 命令搜索与创建用户相关的关键字，如 `new user`。这样可以找到所有新建用户的日志条目。
```php
cat /var/log/auth.log.1 | grep -a "new user"
```
![Pasted image 20250319213702](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/Pasted%20image%2020250319213702.png)
此命令会列出所有包含 `new user` 的日志行，这些行通常记录了用户创建的详细信息。
**步骤3**：提取新用户信息
从日志中提取新用户的详细信息，包括用户名、创建时间等。
例如，假设你得到了如下输出：
```php
Jan 12 10:32:15 server useradd[1234]: new user: name=testuser, UID=1001, GID=1001, home=/home/testuser, shell=/bin/bash
```
这条日志显示了创建的新用户 `testuser`。
**步骤4**：分析执行上下文
确认新用户的创建是否由合法用户执行，或是否有可疑的远程登录记录。
可以使用以下命令查找所有用户登录的情况，以确定是否有可疑的登录行为：
```php
grep "Accepted" /var/log/auth.log.1
```
**步骤5**：进一步确认
结合其他日志文件，如 `/var/log/syslog`，查看是否有异常的命令执行或系统变更。
flag
```
flag{test2}
```
# 总结
1. **有多少IP在爆破主机ssh的root帐号:**
命令：
```php
cat auth.log.1 | grep -a "Accepted " | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
命令解释
```php
1. cat auth.log.1:
    - `cat` 命令用于显示 `auth.log.1` 文件的内容。
    - 这里 `auth.log.1` 是一个日志文件，通常是系统日志的备份文件。
2. |:
    - 管道符号，用于将前一个命令的输出作为下一个命令的输入。
3. grep -a "Failed password for root":
    - grep 命令用于在输入中搜索包含特定模式的行。
    - -a 选项表示将文件内容视为文本文件（此选项通常在处理二进制文件时使用，但对于纯文本文件，可以省略）。
    - `"Failed password for root"` 是搜索模式，即查找所有包含"Failed password for root"的行，这些行表示尝试登录root用户的失败尝试。
4. awk '{print $11}':
    - `awk` 是一个文本处理工具，用于按字段处理文本。
    - `{print $11}` 表示打印每行的第11个字段。假设日志格式为标准格式，第11个字段通常是IP地址。
5. sort:
    - `sort` 命令用于对输入行进行排序。
    - 这里是对提取的IP地址进行排序。
6. uniq -c:
    - `uniq` 命令用于删除重复的行。
    - `-c` 选项表示对每个唯一的行计数，即统计每个IP地址的出现次数。
7. sort -nr:
    - `sort` 命令再次用于排序。
    - `-n` 选项表示按数值进行排序。
    - `-r` 选项表示按降序排序。
    - 组合起来，即按出现次数从高到低排序。
8. more:
    - `more` 命令用于分页显示输出。
    - 由于输出可能很长，`more` 命令允许用户逐页查看结果。
```
2. **ssh爆破成功登陆的IP是多少？**
命令：
```php
cat auth.log.1 | grep -a "Accepted " | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
命令解释：
```php
	grep -a "Accepted ":
    - 作用：在日志文件中查找包含“Accepted ”的行。这些行记录了成功的SSH登录事件。
    - `-a`选项：通常用于处理二进制文件时将其视为文本文件，这里一般可以忽略，因为`auth.log.1`应该是纯文本文件
```
3. **爆破用户名字典是什么？**
命令：
```php
cat auth.log.1 | grep -a "Failed password" |perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort -nr
```
命令解释：
```php
- grep -a "Failed password":
    - 作用：在日志文件中查找包含“Failed password”的行。这些行记录了SSH登录失败的事件。
    - `-a`选项：通常用于处理二进制文件时将其视为文本文件，这里一般可以忽略，因为`auth.log.1`应该是纯文本文件。
- perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}':
    - 作用：使用Perl脚本从每一行提取出失败登录尝试的用户名。
    - `while($_=<>)`：逐行读取输入。
    - `/for(.*?) from/`：使用正则表达式匹配模式“for [username] from”，其中`[username]`是登录尝试的用户名。
    - `print "$1\n"`：将提取的用户名打印出来。
- uniq -c:
    - 作用：对提取出的用户名进行去重并计数。每个唯一的用户名会与其出现的次数一起输出。
    - `-c`选项：在每个唯一项的前面显示出现次数。
```
4. **成功登录 root 用户的 ip 一共爆破了多少次？**
代码：
```php
cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
这个就不解释了，跟第一步一样。
5. **黑客登陆主机后新建了一个后门用户，用户名是多少？**
命令：
```
cat auth.log.1 |grep -a "new user"
```
命令解释：
```php
简单来说就是在 grep 命令中
-a 选项的作用是将文件视为文本文件处理，即使文件可能包含一些二进制数据。
通常，grep 会将二进制文件视为二进制数据而不是文本数据，并可能不会显示预期的结果。
使用 -a 选项可以确保 grep 按文本模式处理文件中的内容。
```