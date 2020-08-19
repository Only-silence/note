---
title: 给高德地图marker添加ID和点击事件
date: 2020-03-28 10:59:20
tags:
- vue
- marker
- 高德地图
categories:
- vue
---

##### 高德地图官方api接口里的marker没有ID，只好手动添加ID并为每个marker添加点击事件

<!--more-->

如何引入高德地图上一篇已经写过，现在看代码

方法一：

```javascript
init() {
      var map = new AMap.Map("container", {  //引入地图
        zoom: 12
      });
      var i = 0; //给i一个初始值
      var clickHandler = function(e) {
        this.getLng = e.lnglat.getLng();  //获取点击位置的经纬度
        this.getLat = e.lnglat.getLat();
        var marker = new AMap.Marker({
          position: new AMap.LngLat(this.getLng, this.getLat), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
          clickable: true,  //标记后的坐标点是否可选中
          draggable: true,
          animation: "AMAP_ANIMATION_DROP"  //坐标点选中动画 坠落消效果
        });
        i = i+1 //每次点击i加1
        marker.id = i;  //手动给marker添加id对象
        if(marker.id == 1){  //判断每个marker的ID
          console.log(marker.id)
        }
        map.add(marker);  //地图添加坐标点
        AMap.event.addListener(marker, 'click', function() {
          //给marker绑定单击事件
          alert("123")
        });
      };
      
      //下面和上面marker绑定单击事件相同，都可实现
      // var markerClick = function(){
           //marker的点击事件
      //   alert("111")
      // }
      // marker.on("click", markerClick);  //给marker绑定点击事件
      
      // 绑定事件
      map.on("click", clickHandler);  //给地图绑定点击事件函数
    }
```

方法二：

```javascript
init() {
      //创建地图
      var map = new AMap.Map("container", {
        zoom: 12
      });
      var i = 0;
      var markers = [];
      var positions = [];
      //添加标记点
      var clickHandler = function(e) {
        this.lnglatXY = [e.lnglat.getLng(), e.lnglat.getLat()];
        console.log(this.lnglatXY);
        positions.push(this.lnglatXY);
        let marker = new AMap.Marker({
          position: new AMap.LngLat(
            positions[positions.length - 1][0],
            positions[positions.length - 1][1]
          ),
          clickable: true,
          draggable: true,
          animation: "AMAP_ANIMATION_DROP"
        });
        marker.id = i;
        i++;
        markers.push(marker);

        //添加标记点
        map.add(marker);

        //逆地理编码
        var geocoder = new AMap.Geocoder({
          radius: 1000,
          extensions: "all"
        });
        var that = this;
        geocoder.getAddress(this.lnglatXY, function(status, result) {
          if (status === "complete" && result.regeocode) {
            that.address = result.regeocode.formattedAddress;
            markers[marker.id].setTitle(that.address);
            console.log(result.regeocode.formattedAddress);
          } else {
            console.log("根据经纬度查询地址失败");
          }
        });

        //标记点点击事件绑定
        AMap.event.addListener(marker, "click", function() {
          markers[marker.id].setMap(null); ///删除点击的标记点
        });
      };

      //绑定地图点击事件
      map.on("click", clickHandler);

```

