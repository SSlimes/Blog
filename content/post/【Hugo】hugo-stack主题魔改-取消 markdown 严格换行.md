---
title: 【Hugo】hugo-stack主题魔改-取消 markdown 严格换行
date: 2025-08-29
categories:
  - 其他
image: images/18.webp
---
# 引言：
**不修改之前一定要在markdown换两行才能在博文里换行，实在是太烦了。**
# 修改：
其实只要在 `hugo.yaml` （也就是`config.yaml`）里面对应的选项里添加： `hardWraps: true`
```
markup:
  goldmark:
    renderer:
      hardWraps: true
```
即可！！！

