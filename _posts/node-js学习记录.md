---
title: node.js学习记录
date: 2020-05-20 22:22:19
tags:
- node.js
categories:
- node.js
---

##### node.js安装

1. 下载官方安装包进行安装
2. 使用nvm版本管理器安装（推荐）

<!--more-->

##### 第一个node.js程序

1. 先创建目录:

   `mkdir demo`

   `cd demo`

   `npm init -y`  // 快速创建package.json

   ```javascript
   //读取系统CPU信息
   const os = require('os');
   const cpus = os.cpus();
   console.log('CPU信息:',cpus.length + '核');
   
   //获取内存信息
   const total = os.totalmem();
   console.log('内存信息:',total/1024/1024/1024 + 'G');
   const free = os.freemem();
   console.log('剩余内存信息:',free/1024/1024/1024 + 'G')
   
   //web 服务
   
   //ajax--> api--> web server(node.js)
   
   const http = require('http');
   const server = http.createServer((req,res)=>{
       res.end('hello');
   })
   
   server.listen(3000,'127.0.0.1',()=>{
       console.log('服务已启动')
   })
   ```



##### web应用基础和第一个express应用

1. Express安装

   `npm install express -S`

2. 基础代码

   ```javascript
   const express = require('express');
   //express 实列
   const app = express();
   
   app.use((req,res)=>{
       res.json({
           name:"张三"
       })
   })
   
   //GET请求
   app.post('/name',(req,res)=>{
       // let {age} = res.params;
       res.send('post')
   })
   
   //接收参数
   app.get('/name/:age',(req,res)=>{
       let {age} = req.params;
       res.json({
           name:"张三",
           age
       })
   })
   
   app.listen(3000,()=>{
       console.log('启动成功')
   })
   ```



##### 路由及中间件

1. member.router.js

   ```javascript
   const express = require('express');
   
   const router = express.Router();
   
   router.get('/list',(req,res)=>{
       res.json({
           list:[
               {
                   id:001,
                   name:'张三'
               }
           ]
       })
   })
   
   module.exports = router;
   ```

2. 注册并引入路由

   ```javascript
   const express = require('express');
   const app = express();
   //注册路由
   const memberRouter = require('./member.router')
   // 引入路由
   app.use(memberRouter);
   
   // app.get('/demo',(req,res)=>{
   //     // req:请求对象
   //     // res：服务对象
   //     res.json({
   //         'message':'git'
   //     })
   // })
   
   // app.post('/demo',(req,res)=>{
   //     // req:请求对象
   //     // res：服务对象
   //     res.json({
   //         'message':'post'
   //     })
   // })
   
   app.listen(3000,()=>{
       console.log('服务已启动')
   })
   ```

   

