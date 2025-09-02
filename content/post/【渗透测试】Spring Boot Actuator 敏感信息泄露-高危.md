---
title: 【渗透测试】Spring Boot Actuator 敏感信息泄露-高危
date: 2025-09-02
image: images/27.webp
categories:
  - 渗透测试
---
# 0x00 什么是Actuator
Spring Boot Actuator 模块提供了健康检查，审计，指标收集，HTTP 跟踪等，是帮助我们监控和管理Spring Boot 应用的模块。这个模块采集应用的内部信息，展现给外部模块，可以查看应用配置的详细信息，例如自动化配置信息、创建的Spring beans信息、系统环境变量的配置信息以及Web请求的详细信息等。
如果没有正确使用Actuator，可能造成信息泄露等严重的安全隐患（外部人员非授权访问Actuator端点）。其中heapdump作为Actuator组件最为危险的Web端点，heapdump因未授权访问被恶意人员获取后进行分析，可进一步获取敏感信息。
SpringBoot 1.x 和 2.x 的 Actuator模块设置有差别，访问功能的路径也有差别，但现在多使用的SpringBoot版本为2.x，这篇文章只讲SpringBoo 2.x Actuator模块带来的信息泄露。
# 0x01 Actuator 使用
如果要使用 SpringBoot Actuator 提供的监控功能，需要先加入相关的 maven dependency：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
    <version>2.7.0</version>
</dependency>
```
只要加上了这个actuator依赖，SpringBoot 在运行时会自动开启/actuator/health和/actuator/info这两个 endpoint。
为了更方便漏洞利用，当前环境在一个CMS中加入了该依赖，因为自己新建的Springboot项目没有配置数据库之类的信息。
# 0x02 Endpoints 介绍
Spring Boot 提供了所谓的 endpoints （下文翻译为端点）给外部来与应用程序进行访问和交互。
打比方来说，/health 端点 提供了关于应用健康情况的一些基础信息。metrics 端点提供了一些有用的应用程序指标（JVM 内存使用、系统CPU使用等）。
这些 Actuator 模块本来就有的端点我们称之为原生端点。根据端点的作用的话，我们大概可以分为三大类：
- 应用配置类：获取应用程序中加载的应用配置、环境变量、自动化配置报告等与Spring Boot应用密切相关的配置类信息。
- 度量指标类：获取应用程序运行过程中用于监控的度量指标，比如：内存信息、线程池信息、HTTP请求统计等。
- 操作控制类：提供了对应用的关闭等操作类功能。
    
需要注意的就是：
每一个端点都可以通过配置来单独禁用或者启动
不同于Actuator 1.x，Actuator 2.x 的大多数端点默认被禁掉。Actuator 2.x 中的默认端点增加了/actuator前缀。默认暴露的两个端点为/actuator/health和 /actuator/info
原生端点如下：

| 请求方法 | 端点                         | 描述                                                                                                          |
| ---- | -------------------------- | ----------------------------------------------------------------------------------------------------------- |
| GET  | /actuator                  | 查看有哪些 Actuator端点是开放的。                                                                                       |
| GET  | /actuator/auditevent       | auditevents端点提供有关应用程序审计事件的信息。                                                                               |
| GET  | /actuator/beans            | beans端点提供有关应用程序 bean 的信息。                                                                                   |
| GET  | /actuator/conditions       | conditions端点提供有关配置和自动配置类条件评估的信息。                                                                            |
| GET  | /actuator/configprops      | configprops端点提供有关应用程序@ConfigurationPropertiesbean的信息。                                                       |
| GET  | /actuator/env              | 查看全部环境属性，可以看到 SpringBoot 载入哪些 properties，以及 properties 的值（会自动用*替换 key、password、secret 等关键字的 properties 的值）。 |
| GET  | /actuator/flyway           | flyway端点提供有关 Flyway 执行的数据库迁移的信息。                                                                            |
| GET  | /actuator/health           | 端点提供有关应用程序运行状况的health详细信息。                                                                                  |
| GET  | /actuator/heapdump         | heapdump端点提供来自应用程序 JVM 的堆转储。(通过分析查看/env端点被*号替换到数据的具体值。)                                                     |
| GET  | /actuator/httptrace        | httptrace端点提供有关 HTTP 请求-响应交换的信息。（包括用户HTTP请求的Cookie数据，会造成Cookie泄露等）                                          |
| GET  | /actuator/info             | info端点提供有关应用程序的一般信息。                                                                                        |
| GET  | /actuator/integrationgraph | integrationgraph端点公开了一个包含所有 Spring Integration 组件的图。                                                        |
| GET  | /actuator/liquibase        | liquibase端点提供有关 Liquibase 应用的数据库更改集的信息。                                                                     |
| GET  | /actuator/logfile          | logfile端点提供对应用程序日志文件内容的访问。                                                                                  |
| GET  | /actuator/loggers          | loggers端点提供对应用程序记录器及其级别配置的访问。                                                                               |
| GET  | /actuator/mappings         | mappings端点提供有关应用程序请求映射的信息。                                                                                  |
| GET  | /actuator/metrics          | metrics端点提供对应用程序指标的访问。                                                                                      |
| GET  | /actuator/prometheus       | 端点以prometheusPrometheus 服务器抓取所需的格式提供 Spring Boot 应用程序的指标。                                                   |
| GET  | /actuator/quartz           | quartz端点提供有关由 Quartz 调度程序管理的作业和触发器的信息。                                                                      |
| GET  | /actuator/scheduledtasks   | scheduledtasks端点提供有关应用程序计划任务的信息。                                                                            |
| GET  | /actuator/sessions         | sessions端点提供有关由 Spring Session 管理的应用程序 HTTP 会话的信息。                                                          |
| GET  | /actuator/startup          | startup端点提供有关应用程序启动顺序的信息。                                                                                   |
| POST | /actuator/shutdown         | shutdown端点用于关闭应用程序。                                                                                         |
# 0x03 漏洞利用
前面介绍过了Actuator一些基础后，现在来研究一下如果目标站点存在这个漏洞该如何利用。
首先访问一下/actuator/env该文件目录，寻找一下敏感信息等等
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902094313.png)
/actuator/heapdump文件可以直接下载下来。使用工具进行分析：
工具下载地址：
```
通过网盘分享的文件：JDumpSpider-1.1-SNAPSHOT-full.jar
链接: https://pan.baidu.com/s/1tL0gY9Xm4jBrXUxVj-g8og 提取码: pkb9 
--来自百度网盘超级会员v4的分享
```
使用：
```
java -jar JDumpSpider-1.1-SNAPSHOT-full.jar 绝地地址（heapdump） >>222.txt
```
![image.png](https://blogslimer.oss-cn-shanghai.aliyuncs.com/blog/20250902094616.png)
这里直接泄露了Cookie信息。
# 0x04 漏洞修复
1、屏蔽actuator路径
```
location ~ .*actuator.* {
   deny all;
}
```

