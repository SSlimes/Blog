---
title: 【Hugo】hugo-stack主题魔改-归档标签颜色自定义
date: 2025-08-28
categories:
  - 其他
image: images/11.webp
---
# 最终效果：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828185235.png)
# 自定义修改
修改位置：`\content\categories\标签名\_index.md`
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250828185332.png)
index.md文件内容：
```
---
title: "Web攻防"
description: "介绍"
slug: "Web攻防"
image: "Web攻防.webp"
style:
    background: "#2a9d8f"
    color: "#fff"
---
```
修改：
```
style:
    background: "#2a9d8f"
    color: "#fff"
```
颜色如下，根据自己喜欢的颜色进行修改：

| 风格定位 | 背景色（Background） | 文字色（Color） | 色值说明                  |
| ---- | --------------- | ---------- | --------------------- |
| 清新自然 | #264653         | #ffffff    | 深青绿色背景 + 纯白文字，低饱和更显柔和 |
| 温柔治愈 | #e76f51         | #ffffff    | 暖橙色背景 + 纯白文字，适合温馨场景   |
| 高级极简 | #1d3557         | #f1faee    | 藏蓝色背景 + 米白文字，避免纯白刺眼   |
| 活力明亮 | #ffb703         | #212121    | 亮黄色背景 + 深灰文字，对比强烈不刺眼  |
| 复古优雅 | #8b5a2b         | #f5f5f5    | 棕褐色背景 + 米白文字，自带复古质感   |
| 冷静专业 | #3a86ff         | #ffffff    | 天蓝色背景 + 纯白文字，适合商务场景   |
| 森系柔和 | #43aa8b         | #f9fafb    | 浅草绿背景 + 近白文字，自然不突兀    |
| 神秘高级 | #2b2d42         | #edf2f4    | 深灰蓝背景 + 浅灰文字，低调有质感    |
| 甜美清新 | #ff6b6b         | #ffffff    | 浅粉色背景 + 纯白文字，适合女性向场景  |
| 沉稳大气 | #0077b6         | #ffffff    | 深海蓝背景 + 纯白文字，显专业且庄重   |

只需要修改：background和color就可以了。