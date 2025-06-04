## Koa

#### Koa简介

koa通过使用async函数，丢弃回调函数，增强错误处理

`npm i koa` 

`yarn add koa`

实现hello world

```javascript
const koa = require('koa')
const app = new koa()
app.use(async (ctx)=>{
    ctx.body = 'Hello World'
})
app.listen(9988,()=>{
    console.log('http://localhost:9988  服务器已启动')
})
```

#### app.listen(...)

其中**app.listen**属于一种语法糖

Koa应用程序不是http服务器的一对一的展现。可以将一个或者多个Koa应用程序安装在一起形成具有单个HTTP吴福气的更大应用程序

```javascript
const http = require('http')
const https = require('https')
const koa = requrie('koa')
const app =  new koa()
//同时监听http和https端口
http.createServer(app.callback()).listen(9988)
https.createServer(app.callback()).listen(9988)
```

#### app.callback()

返回适用于http.createServe()方法的回调函数来处理请求。可以使用此回调函数将Koa应用程序挂到Connect、Express应用程序中

#### app.use(function)

将给定的中间件方法添加到此应用程序。app.use返回this，因此可以链式调用

```javascript
app.use(someMiddleware)
app.use(someOtherMiddleware)
app.listen(9988)

//等同于
app.use(someMiddleware).app.use(someOtherMiddleware).app.listen(9988)
```

#### 配置路由

