---
title: 【Hugo】hugo-stack主题魔改-固定代码块高度
date: 2025-08-28
categories:
  - 其他
image: images/17.webp
---
# 效果
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250829024714.png)
# 修改
高度限制在 20em，并隐藏滚动条。
增添到 ==/themes/hugo-theme-stack/assets/scss/partials/article.scss==
```
    .article-content {
        .highlight {
            padding: var(--card-padding);
            pre {
                margin: initial;
                padding: var(--card-padding);
                margin: 0;
                width: auto;
                max-height: 20em;
                scrollbar-width: none;
                /* Firefox */
                &::-webkit-scrollbar {
                    display: none;
                    /* Chrome Safari */
                }
            }
        }
    }
```
修改完成！