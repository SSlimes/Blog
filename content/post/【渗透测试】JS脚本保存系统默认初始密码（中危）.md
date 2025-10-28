---
title: 【渗透测试】JS脚本保存系统默认初始密码（中危）
date: 2025-10-26
image: images/109.webp
categories:
  - 渗透测试
---
# 0x00 风险详情
攻击者可以通过前端页面的代码敏感信息搜索，了解系统的账户初始默认密码和内网IP之类的内部信息，增大账户破解和进一步攻击的可能性。
# 0x01 漏洞证明
访问js脚本https://xxIPxx/assets/izw9a.js，右击查看网页源代码，发现存在初始密码123456字样和IP等内网字样。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20251028105945.png)
# 0x02 漏洞修复
前端代码和JS文件中，不要写入各类敏感信息。