---
title: 【Hugo】hugo-stack主题魔改-博客运行时间
date: 2025-08-27
categories:
  - 其他
image: images/3.webp
---
# 博客运行时间
## 1. 完成样式
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250827160323.png)
## 2. footer.html文件修改
在`layouts\partials\footer\footer.html`目录下新增以下代码：（没有该文件则自己创建）
```
    {{ with .Site.Params.footer.runtimeSince }}
    <section class="runtime" style="margin-top:8px;font-size:1.3rem">
        <span id="site-runtime">站点已运行计算中...</span>
    </section>
    <script>
      (function(){
        var sinceStr = '{{ . }}'.replace('T',' ').replace('t',' ');
        var start = new Date(sinceStr);
        function update(){
          var now = new Date();
          var diff = Math.max(0, now - start);
          var days = Math.floor(diff / (24*60*60*1000));
          var hours = Math.floor((diff % (24*60*60*1000)) / (60*60*1000));
          var mins = Math.floor((diff % (60*60*1000)) / (60*1000));
          var secs = Math.floor((diff % (60*1000)) / 1000);
          var el = document.getElementById('site-runtime');
          if(el){ el.textContent = '站点已运行 ' + days + ' 天 ' + hours + ' 小时 ' + mins + ' 分 ' + secs + ' 秒'; }
        }
        update();
        setInterval(update, 1000);
      })();
    </script>
    {{ end }}
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250827160253.png)
## 3. 修改config.yaml文件
在`config.yaml`文件下新增该内容即可
```
runtimeSince: 2025-08-025T00:00:00   # 站点开始运行的时间（本地时区），用于页脚运行时间
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250827160530.png)
## 4. 修改custom.scss文件按
让页脚运行时间颜色与版权一致。修改\assets\scss\custom.scss文件。（没有自己创建即可）
```
/* 页脚运行时间颜色与版权一致 */
footer.site-footer {
  .runtime {
    color: var(--accent-color);
    font-weight: bold;
  }
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250827160830.png)
