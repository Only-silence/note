---
title: 'uniapp实现在线更新'
date: 2020-04-28 16:48:01
tags:
- uniapp
- 更新
categories:
- uniapp
---

##### 在App.vue页面onLaunch生命周期函数里加入以下代码<!--more-->

```javascript
onLaunch: function() {
	console.log('App Launch');
	uni.removeStorageSync('userSelect');
	//#ifdef APP-PLUS  
	var server = "http://tengrinshar.api.orkhonsoft.com/api/config/appversion/field/last"; //检查更新地址  
	var versionCode = 0;
	var version = 0;
	plus.runtime.getProperty(plus.runtime.appid,function(inf){
        //当前App版本
		versionCode = inf.versionCode;
		version = inf.version;
	});
	setTimeout(function() {
		uni.request({
			url: server,
			success: (ress) => {
				console.log('最新版本号:'+ress.data.result.androidVersion,'版本名称:'+version,'当前版本号:'+versionCode)
				if (ress.statusCode == 200 && ress.data.result.androidVersion > versionCode) {
					uni.showModal({ 
						title: "版本更新",
						content: '有新的版本发布',
						success: function(res) {
							if (res.confirm) {
								uni.showToast({
									title: '程序已启动自动更新，\n新版本下载完成后将自动弹出安装程序，\n这个过程可能需要几分钟，\n请您耐心等待',
									mask: true,
									duration: 5000,
									icon: "none"
								});
								uni.request({
									//请求地址，设置为自己的服务器链接
									url: 'http://tengrinshar.api.orkhonsoft.com/api/file/file/' + ress.data.result.fileId,
									//method: 'GET',
									data: {},
									success: req => {
										console.log('res===>', req);
										var downloadApkUrl = req.data.result.url;
										var dtask = plus.downloader.createDownload( downloadApkUrl, {}, function ( d, status ) {
												// console.log('status===>',status)
												// 下载完成  
												if ( status == 200 ) {
													// console.log('下载成功')
													plus.runtime.install(plus.io.convertLocalFileSystemURL(d.filename),{},{},function(error){  
														uni.showToast({  
															title: '安装失败',
															duration: 1500  
														});
													})
												} else {
													uni.showToast({
														title: '下载失败，请您检查网络或稍后再试',
														duration: 1500
													});
												}
											});
										dtask.start();
									},
									fail: () => {},  
									complete: () => {}  
								});
							} else if (res.cancel) {
				
							}
						}
					});
				}
			}
		})
	},1000)
	//#endif 
}
```

###### 注：必须要加条件编译，否则小程序报错