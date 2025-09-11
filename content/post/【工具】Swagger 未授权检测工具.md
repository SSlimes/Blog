---
title: 【工具】Swagger 未授权检测工具
categories:
  - 工具
date: 2025-09-11
image: images/61.webp
---
# 介绍
用于探测 Swagger/OpenAPI 文档并尝试未授权访问接口，辅助安全自查。识别 swagger-ui 页面、swagger-resources 入口、Swagger 1.x/V2/V3 文档，在谨慎策略下对接口进行 GET/POST 探测并生成 Excel 报告。
# 下载地址
```
https://github.com/mk4no1/SwaggerAH
```
# 快速开始
```
# 单目标扫描（自动识别入口类型：swagger-ui / api-docs / swagger-resources）
python swagger.py -u https://example.com/swagger-ui/index.html


# 忽略文档内 servers，强制使用初始域名（遇到内网域名或IP直接使用此参数）
python swagger.py -u https://example.com/v3/api-docs --force-domain
```
```
# 批量扫描（每行一个 URL）
# urls.txt:
# https://a.com/v3/api-docs
# https://b.com/swagger-ui/index.html
python swagger.py -f urls.txt
```
红框或者error忽略即可
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911132342.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911132354.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911132402.png)
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250911132408.png)
# 安装与依赖
Python 3（本人Python 3.10.11）
依赖安装：
```
pip install requests loguru selenium openpyxl urllib3
```
# 使用说明
- 常用参数
    
    - `-u, --url`：单个目标（支持 swagger-ui、api-docs、swagger-resources）
    - `-f, --file`：批量目标文件（逐行 URL）
    - `--force-domain`：忽略文档内 `servers`，强制使用初始域名
- 请求与参数策略（宁可漏报也不可误删误改）
    
    - 仅尝试 `GET`、`POST`
    - 自动填充占位参数：
        - path 参数：用占位值替换路径变量（数字就填充1，字母填充a）
        - query 参数：填充简单占位值
        - body(JSON)：按 schema 尝试生成示例
    - 命中高风险关键字的 URL 自动跳过
# 报告说明
- 扫描结束生成 `YYYYMMDDHHMM_xxx.xlsx`：
    - `所有API`：记录发现的所有路径，标注“调用/过滤”
    - `已调用API`：记录请求方法、URL、参数、状态码、截断后的响应
    - `已过滤API`：记录被策略拦截的接口与原因。