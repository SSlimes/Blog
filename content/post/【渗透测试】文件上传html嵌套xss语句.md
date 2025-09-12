---
title: 【渗透测试】文件上传html嵌套xss语句
date: 2025-09-12
categories:
  - 渗透测试
image: images/66.webp
---
# 0x01 漏洞描述
在我们文件上传的时候很有很可能会遇见无法上传马子的情况，我相信很多人在这个时候都会直接不管这一个上传点，然后在企业src里面这样的上传点可以试试上传一个html文件配合xss语句，导致一个挂黑页弹窗的危害，在众测里面达到了中危的标准
# 0x02 漏洞测试工具
Burp  html配合xss语句的文件
```go
<!DOCTYPE html>  
<html>  
<head>  
<title></title>  
<meta charset="utf-8">  
<script type="text/javascript">  
alert("testxss");  
</script>  
</head>  
<body>

  
</body>  
</html>
```
# 0x03 漏洞点及测试方法
漏洞点：存在文件上传的地方
测试方法： 在文件上传的时候上传Html嵌套xss语句
# 0x04 案例1
一次的众测活动中，看见一个营业职照的上传点，能上传马子但是不解析，于是随手改为html上传成功。赏金（800r)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912151103.png)
# 0x04 案例2
## 某src的文件上传xss（赏金100r）
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912151140.png)
同样的在页面找到文件上传的地方
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912151148.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912151153.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250912151159.png)