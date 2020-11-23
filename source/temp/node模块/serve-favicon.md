---
title: nodejs模块：serve-favicon
date: 2020-02-04 23:47:30 
categories: nodejs 
tags: [nodejs]
description: nodejs模块
---
翻译自[官网文档](https://www.npmjs.com/package/serve-favicon)。

该模块为Nodejs中用于支持favicon的中间件。

为什么使用这个模块？

* 用户代理经常随意加载favicon.ico，所以你可能希望将这些请求从你的日志中移除。此时可以将该中间将放置在日志中间件之前。
* 这个模块通过将icon缓存至内存中，避免去硬盘中读取icon，从而提高性能。
* 这个模块提供ETag支持。该ETag值基于icon的内容，而不是文件系统的特性。
* 这个模块将选用最合适的Content-type。

注意这个模块是专门为“默认的、隐式的favicon”服务的，即`GET /favicon.ico`。对于使用HTML标记的、供应商指定的icons，需要额外的中间件以支持该相关文件，例如：serve-static。

## 安装
这是一个nodejs的模块，可以利用npm进行安装。通过利用以下npm命令进行安装：
```shell
$ npm install serve-favicon
```

## API
### favicon(path, options)
创建新的中间件以支持给定路径的favicon文件。path可以是icon的Buffer。

#### Options
使favion接收options对象中所列举的属性。

#### maxAge
使用ms库的`cache-control`、`max-age`指令进行控制。默认为一年。同样可以使用能够被[ms](https://www.npmjs.com/package/ms#readme)模块所接收的字符串进行更改。

## 示例
通常情况下，这个中间件将会很早地出现在你的栈中（有时甚至是第一个）以避免针对favicon的请求被其他中间件处理。

### express框架
```js
var express = require('express')
var favicon = require('serve-favicon')
var path = require('path')
 
var app = express()
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')))
 
// 在这里添加你的路由等内容。
 
app.listen(3000)
```

### connect框架
```js
var connect = require('connect')
var favicon = require('serve-favicon')
var path = require('path')
 
var app = connect()
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')))
 
// 在这里添加你的中间件等内容。
 
app.listen(3000)
```

### vanilla http server
这个中间件可以在任何地方被使用，甚至在express/connnect之外。它需要`req`、`res`、`callback`三个参数。
```js
var http = require('http')
var favicon = require('serve-favicon')
var finalhandler = require('finalhandler')
var path = require('path')
 
var _favicon = favicon(path.join(__dirname, 'public', 'favicon.ico'))
 
var server = http.createServer(function onRequest (req, res) {
  var done = finalhandler(req, res)
 
  _favicon(req, res, function onNext (err) {
    if (err) return done(err)
    
    // 继续在这里处理请求等信息。
 
    res.statusCode = 404
    res.end('oops')
  })
})
 
server.listen(3000)
```