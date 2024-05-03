# 关于项目的细节问题

##### 项目名称（写在简历中的名称）：CRM客户关系管理系统

##### 项目开发周期：开始时间：2022-12月

```
产品和ui：2个多月
后端：2个人
前端：2个

从2022-12月 开发至今 一直在调整
```

##### 项目开发人员：

```
产品：1（不是全天干一个）
ui：1（不是全天干一个）
前端：2（分模块干的）
后台：2
```

##### 项目的难点和亮点：软件自动更新

```
1. 业务逻辑
	打包软件：检查更新（是否有新版本）==》去下载（之前打包的网址的内容进行本地更新）==》更新完毕（windows没有问题，mac os：需要上架app store）==》再次打开软件。

2. 怎么检查更新
	A>第一次打包：npm run build:mac 
	会生成：dist文件夹中
				1. electron-app-1.0.0.dmg ==》 就是本软件的安装包
				 需要给后端｜运维 ==》 上传到服务器上，弄一个链接
				
				2. electron-app-1.0.0-arm64-mac.zip
						当前软件的所有文件
						
				3. latest-mac.yml
						当前软件的最新信息（版本、字节）
						
				4. builder-effective-config.yaml
						软件的各种配置项说明
						
		B> 用户本地的软件（latest-mac.yml携带进去的，每次用户打开软件，文件都会执行，执行里面url去拿最新的版本【本地和url的线上版本是否一致】)，如果不一致，就会走对应更新操作（执行更新代码）。
		
3. 前端 做 自动更新，怎么做的？
	a> 要开启这个更新操作（如果在开发阶段测试，需要使用Object.defineProperty把他内置app对象中的key修改成true（看他源码，结果app的isPackaged字段值是只读没有set）
	b> 弄一个文件。需要后端包装给一个url：这个url就是本地和线上的软件版本对比。
	c> 下面就是api就不说了（自动更新的api）直接看官网调用就行了。
	d> 运行这个更新操作，他开发环境和生产环境的api又不同（这个又搞了好久）
```

##### 项目的安全：

```
1. saas的登录有短信（做了防刷）
2. 本身就自带内容安全策略
3. 上传文件限制
```

##### 项目上线时间：

```
2023年7月——至今（差不多10版）
```

##### 项目的兼容：

```
问题性兼容：
	锁定窗口（windows和mac os）...
唯一的兼容：全屏和恢复页面
	 var element = document.documentElement;
   var isFull = !!(document.webkitIsFullScreen || document.mozFullScreen || document.msFullscreenElement || document.fullscreenElement);
      if(isFull){
        if(document.exitFullscreen) {
          document.exitFullscreen();
        }else if (document.msExitFullscreen) {
          document.msExitFullscreen();
        }else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen();
        }else if (document.webkitExitFullscreen) {
          document.webkitExitFullscreen();
        }
      }else{
        if(element.requestFullscreen) {
          element.requestFullscreen();
        }else if(element.msRequestFullscreen) {
          element.msRequestFullscreen();
        }else if(element.mozRequestFullScreen) {
          element.mozRequestFullScreen();
        }else if(element.webkitRequestFullscreen) {
          element.webkitRequestFullscreen();
        }
      }
```

##### 项目的源码：

```
面试问：可以看看吗？你打开让他看。
面试问：源码，我有全部的（sql、后端（java）、electron），我可以拿着电脑，让你看，但是我不能给你，因为有保密协议！
	注意：这是一个管理系统，不能给面试官登录进去看的，但是他是一个saas系统，给客户演示也没毛病。
```

