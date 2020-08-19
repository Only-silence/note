---
title: vue cli引入高德地图api
date: 2020-03-28 10:39:22
tags:
- vue
- api
- 高德地图
categories:
- vue
---

##### vue cli引入高德地图官方api接口<!--more-->

1. 在public文件夹下的index.html引入官方api

   ```javascript
   <script type="text/javascript" src="https://webapi.amap.com/maps?v=1.4.15&key=官方申请的Key"></script> 
   ```

2. 项目更目录下新建文件 vue.config.js

   ```javascript
   module.exports = {
     configureWebpack: {
      externals: {
       AMap: "window.AMap"
      }
     }
   };
   ```

3. 项目中引入地图

   ```javascript
   import AMap from "AMap"; // 引入高德地图
   ```

   ```javascript
   mounted() {
       this.init();
   },
   methods: {
   	init() {
         var map = new AMap.Map("container", {
           //center: [116.397428, 39.90923],  //地图默认显示位置
           zoom: 12
         });
         AMap.plugin(
           [
             "AMap.ToolBar",
             "AMap.Scale",
             "AMap.OverView",
             "AMap.MapType",
             "AMap.Geolocation"
           ],
           function() {
             // 在图面添加工具条控件，工具条控件集成了缩放、平移、定位等功能按钮在内的组合控件
             // map.addControl(new AMap.ToolBar());
             var toolbar = new AMap.ToolBar({
               ruler: false,
               offset: new AMap.Pixel(30, 10),
               position: "RB"
             });
             map.addControl(toolbar);
   
             // 在图面添加比例尺控件，展示地图在当前层级和纬度下的比例尺
             map.addControl(new AMap.Scale());
   
             // 在图面添加鹰眼控件，在地图右下角显示地图的缩略图
             // map.addControl(new AMap.OverView({ isOpen: true }));
   
             // 在图面添加类别切换控件，实现默认图层与卫星图、实施交通图层之间切换的控制
             // map.addControl(new AMap.MapType());
   
             // 在图面添加定位控件，用来获取和展示用户主机所在的经纬度位置
             // map.addControl(new AMap.Geolocation());
           }
         );
       }
   }
   ```

   