# 第一部分 HTTP: Web 的基础

## 第一章：HTTP 概述

媒体数据类型：MIME（Multipurpose Internet Mail Extension）类型

统一资源标识符（Uniform Resource Identifier）

统一资源定位符（Uniform Resource Locator）

统一资源名（Uniform Resource Name）

HTTP 事务：有一条 HTTP 请求命令和一条 HTTP 相应结果组成。

HTTP 请求命令（HTTP 方法）：

+ GET
+ PUT
+ DELETE
+ POST
+ HEAD

HTTP 状态码：

+ 200: OK
+ 302: Redirection
+ 404: Not Found

HTTP 报文（请求报文，响应报文）：

+ 起始行：报文的第一行。在请求报文中说明要做什么，在响应报文中说明反馈信息。
+ 首部块：键值对格式的首部字段，以空行结束。
+ 主体：数据

HTTP 主要版本

+ 1.0：第一个广泛应用的版本
+ 1.1：持久连接
+ 2.0：

HTTP 重要的组件：

+ 代理
+ 缓存
+ 网管
+ 隧道
+ Agent 代理

## 第二章：URL 与资源

URL 完整结构如下

```url
<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>
```

URL 字符集：ASCII 码子集 + 转义字符集

## 第三章：HTTP 报文

请求报文格式：
```html
<method> <request-URL> <version>
<headers>

<entity-body>
```

响应报文格式：

```html
<version> <status> <reason-phrase>
<headers>

<entity-body>
```

状态码分类：

+ 1xx：信息提示
+ 2xx：成功
+ 3xx：重定向
+ 4xx：客户端错误
+ 5xx：服务器错误

首部分类：

+ 通用首部：
    * Connection
    * Date
    * MIME-Version
+ 请求首部：
    * Host
    * Referer
    * User-Agent
    * Accept
    * Cookie
    * Proxy-Authorization
+ 响应首部：
    * Age
    * Server
    * Title
+ 实体首部：
    * Content-Encoding
    * Content-Type
    * EXpires

方法分类：

+ GET：请求服务器资源
+ HEAD：请求资源的 **首部**，了解资源信息
+ PUT：向服务器写入数据（存入服务器）
+ POST：向服务器发送数据（不一定存入服务器）
+ TRACE：


## 第四章：连接管理

TCP 基础

HTTP 服务性能改善

+ 并行连接
+ 持久连接
