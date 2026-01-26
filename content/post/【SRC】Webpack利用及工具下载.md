---
title: 【SRC】Webpack利用及工具下载
date: 2026-01-26
categories:
  - SRC
image: /images/117.webp
---
# 介绍
Webpack 是一款**前端静态模块打包工具**，核心作用是将前端项目中分散的各类资源（JavaScript、CSS、图片、字体等）视为**模块**，通过分析模块间的依赖关系，将其按规则打包合并为浏览器可直接识别的静态文件（如 bundle.js、bundle.css 等），解决前端项目资源分散、依赖复杂、浏览器兼容性差等问题，是现代前端工程化的核心工具之一。
# 利用
1：当我们打开插件时，会发现存在webpack
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260126112703.png)
2:正常情况下，后台会存在webpack目录
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260126112723.png)
3：使用专业的工具packerfuzzer.py提取漏洞
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260126112742.png)
4：它会自动生成word文档
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260126112806.png)
5：得出敏感信息泄露
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20260126112832.png)
# 工具地址
```
https://github.com/rtcatc/Packer-Fuzzer
```