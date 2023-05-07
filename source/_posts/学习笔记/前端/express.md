初始化node项目
```shell
// 初始化命令
npm init
// 安装express,并添加到依赖当中
npm install express --save
// 创建index.js

```
```javascript
// 引入模块
// commonjs的模块引入方式
const express = require('express')
// es6的引入方式
import express from 'express'
const app = express()
const port = 3000

// 接口
app.get('/', (request, response) => {
// 设置响应头
response.header("Access-Control-Allow-Origin", "*");

// 请求的参数
request.query

// 转为json并发送
response.json(Object对象)

// 发送文本
  response.send('Hello World!')
})

// 监听3000端口
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
```shell 
node index.js
运行

```