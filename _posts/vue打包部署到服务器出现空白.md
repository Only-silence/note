---
title: 'vue打包部署到服务器出现空白'
date: 2019-12-31 04:36:58
tags:
- vue
- vue-cli
- 服务器
- 空白
categories:
- vue
---

##### 问题：

- vue-cli本地运行`npm run build`打包后 部署到服务器，浏览器访问一片空白，没有404等报错

<!--more-->

##### 解决方法（vue-cli3以下版本）：

1. 因为index.html里边的内容都是通过script标签引入的，而你的路径不对，打开肯定是空白的。
2. 找到vue.config.js下的assetsPublicPath，默认的是  ‘/’  也就是根目录。而我们的index.html和static在同一级目录下面。  所以改为  ‘./ ’。
3. 重新执行`npm run build`就可以了。



#### 扩展

##### vue.config.js配置详情：

- vue-cli3配置文件

``````javascript
module.exports = {
    publicPath: './',//publicPath取代了baseUrl
    outputDir: 'dist',
    lintOnSave: true,
    runtimeCompiler: true, //关键点在这  原来的 Compiler 换成了 runtimeCompiler
    // 调整内部的 webpack 配置。
    // 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli/webpack.md
    chainWebpack: () => {},
    configureWebpack: () => {},
    // 配置 webpack-dev-server 行为。
    devServer: {
      open: process.platform === 'darwin',
      host: '0.0.0.0',
      port: 5000,
      https: false,
      hotOnly: false,
      // 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli/cli-service.md#配置代理
      proxy: null, // string | Object
      before: app => {}
    }
  }
``````

