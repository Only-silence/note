---
title: '纬度数组转换为POLYGON和MULTIPOLYGON格式'
date: 2020-06-10 09:39:08
tags:
- map
- POLYGON
- MULTIPOLYGON
categories:
- map
---

##### 将高德地图返回的经纬度转换为POLYGON和MULTIPOLYGON格式

<!--more-->

###### 1. 将获取到的经纬度`push()`到一个数组中

```javascript
var arr = [];
arr.push(
    {
        latitude: latitude,
        longitude: latitude
    },
    {
        latitude: latitude,
        longitude: latitude
    },
    // ......
);
```



###### 2. 转换格式

1. 转换为 `POLYGON` 格式

   ```javascript
   let result = 'POLYGON ((';
   arr.forEach(point => {
   	result += (point[0].longitude + ' ' + point[0].latitude + ',')
   })
   let content = result.substr(0, result.lastIndexOf(','));
   return content + '))'
   ```

   * 将传进来的经纬度拼接为 `POLYGON` 格式

   * `forEach()` 循环遍历 `arr` 数组，将数组转换为  `longitude latitude,`  格式

   * 拼接后变为 `POLYGON ((longitude latitude, longitude latitude))` 格式

     

2. 转换为 `MULTIPOLYGON` 格式

   ```javascript
   let result = 'MULTIPOLYGON (';
   let left = '((';
   let right = ')), ';
   for(let i = 0; i < arr.length; i++){
   	result += left;
   	for(let j = 0; j < arr[i].length; j++){
   		result += (arr[i][j].longitude + ' ' + arr[i][j].latitude + ',')
   	}
   	result=result.substring(0,result.length-1)
   	result += right;
   }
   let content = result.substr(0, result.lastIndexOf(','));
   return content + ')'
   ```

   * 将传进来的经纬度拼接为 `MULTIPOLYGON` 格式

   * `forEach()` 循环遍历 `arr` 数组，将数组转换为  `longitude latitude,`  格式

   * 拼接后变为 `MULTIPOLYGON (((longitude latitude, longitude latitude), (longitude latitude, longitude latitude)))` 格式

     

3. 解析为经纬度数组

   1. 解析 `POLYGON` 格式

   ```javascript
   let result = [];
   let arrStr = mapString.substring(10,(mapString.length-2))
   let arr = arrStr.split(',')
   arr.forEach(item =>{
   	let str = item.replace(/^\s*|\s*$/g,"");
   	let obj = str.split(' ')
   	result.push({
   		latitude: parseFloat(obj[1]),
   		longitude: parseFloat(obj[0])
   	})
   })
   ```

   

   2. 解析 `MULTIPOLYGON` 格式

   ```javascript
   let lonlat = [];
   let arrStr = mapString.substring(16,(mapString.length-3))
   let arr = arrStr.split(')), ((');
   arr.forEach(item =>{
   	let result = [];
   	let str = item.replace(/^\s*|\s*$/g,"");
   	let obj = str.split(', ');
   	obj.forEach(list =>{
   		let strs = list.replace(/^\s*|\s*$/g,"");
   		let objs = strs.split(' ')
   		result.push({
   			latitude: parseFloat(objs[1]),
   			longitude: parseFloat(objs[0])
   		})
   	})
   	lonlat.push(result)
   })
   ```

   

* 删除特殊符号正则表达式 `url.replace(/\:/g,'')` ，将 `:` 改为你要删除的符号