---
author: Slimer
title: 【Hugo】hugo-stack主题魔改-1
date: 2025-08-27
slug: 其他
categories:
  - 其他
image: images/2.webp
---

# 0x01 搭建方式
直接拉取我的github仓库文件即可：
```
https://github.com/SSlimes/Blog
```
# 0x02 stack主题修改
## 1. 字体修改：
1. 前往【[100font](https://www.100font.com/)】，下载自己想要的字体，字体文件为 **fusion-pixel-10px-monospaced-zh_hans.ttf**
2. 把字体文件放入`assets/font`下(文件夹自己创建)
3. 将以下代码修改并复制到`layouts/partials/footer/custom.html`文件中(文件不存在就自己创建)
```
<style>
  @font-face {
    font-family: '字体名';
    src: url({{ (resources.Get "font/字体文件名").Permalink }}) format('truetype');
  }

  :root {
    --base-font-family: '字体名';
    --code-font-family: '字体名';
  }
</style>
```
## 2. 背景图片
将以下代码复制到`layouts/partials/footer/custom.html`文件中(文件不存在则自行创建)
```
<style>
    body {
      background: url({{ (resources.Get "background/bz.png").Permalink }}) no-repeat center top;
      background-size: cover;
      background-attachment: fixed;
      backdrop-filter: blur(50px);
      -webkit-backdrop-filter: blur(50px); /* 注意：这里建议和上面的模糊值保持一致，避免兼容问题 */
    }
  </style>
```
## 3. 首页欢迎横幅
在 `/layouts/index.html` 的 `<section class="article-list">` 前添加以下代码：
```
<!-- 首页欢迎字幅 -->
<div class="welcome">
  <p style="font-size: 4rem; text-align: center; font-weight: bold; text-shadow: 0 2px 3px rgba(0,0,0,0.15);">
    <span class="shake">👋</span>
    <span class="jump-text1" style="color: #ffffff;"> Welcome</span>
    <span class="jump-text2" style="color: #ffffff;"> To </span>
    <span class="jump-text3" style="color: #ff85b3;">S</span><span class="jump-text4" style="color: #ff85b3;">l</span><span class="jump-text5" style="color: #ff85b3;">i</span><span class="jump-text6" style="color: #ff85b3;">m</span><span class="jump-text7" style="color: #ff85b3;">e</span><span class="jump-text8" style="color: #ff85b3;">r</span><span class="jump-text9" style="color: #ff85b3;">'</span><span class="jump-text10" style="color: #ff85b3;">s</span>
    <span class="jump-text11" style="color: #ffffff;">Blog</span>
  </p>
</div>

<!-- 首页欢迎字幅 -->
```
## 4. macOS风格的代码块
1. 准备一张macOS代码块的[**红绿灯图片**](https://letere-gzj.github.io/hugo-stack/p/hugo/custom-stack-theme/macOS-code-header.svg)(Ctrl+S保存), 放到`static/icons`文件夹下
2. 将以下代码复制进`assets/scss/custom.scss`文件中(不存在则自行创建)
```
.highlight {
  border-radius: var(--card-border-radius);
  max-width: 100% !important;
  margin: 0 !important;
  box-shadow: var(--shadow-l1) !important;
}

.highlight:before {
  content: "";
  display: block;
  background: url(../icons/macOS-code-header.svg) no-repeat 0;
  background-size: contain;
  height: 18px;
  margin-top: -10px;
  margin-bottom: 10px;
}
```
## 5. 头像旋转
在 `/assets/scss/custom.scss` 中加入以下代码：
```
// 头像旋转动画
.sidebar header .site-avatar .site-logo {
  transition: transform 1.65s ease-in-out; // 旋转时间
}

.sidebar header .site-avatar .site-logo:hover {
  transform: rotate(360deg); // 旋转角度为360度
}
```

