# 一、uniapp优势

```
开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/飞书/QQ/快手/钉钉/淘宝）、快应用等多个平台。
```

# 二、uniapp缺点：性能

原生（ios、安卓） ==》单跨平台==》多跨平台

注意：如果使用uniapp只做app，那么可以使用nvue（性能要比.vue的文件高）

# 三、条件编译

在某端生效

```
<!-- #ifdef H5 -->
			1111
<!-- #endif -->
```

在某端 “不 ” 生效

```
<!-- #ifndef H5 -->
			1111
<!-- #endif -->
```



##### 注意：app系统 条件编译 是做不到区分ios、安卓的。

```
解决：通过js判断来解决：

const systemInfo = uni.getSystemInfoSync();
 
if (systemInfo.platform === 'android') {
    console.log('当前为安卓');
} else if (systemInfo.platform === 'ios') {
    console.log('当前为iOS');
} else {
    console.log('未知平台');
}
```

##### 什么时候判断ios或安卓呢？

```
在ios上虚拟产品需要走官方的充值支付：那么在页面就要判断，余额是否够，如果不够，进入充值页。（但是安卓不需要，安卓可以直接支付宝或者微信支付）
```



# 四、uniapp开发微信小程序

#### 4.1 开发流程：

```
1. 拿到 本小程序 的开发权限
2. 拿到 本小程序 的appid
3. 请求接口 ==> 
			1. 开发阶段可以不校验url来解决跨域的问题
			2. 生产阶段（不校验url不生效）。解决办法：后台设置request合法域名（https开头的）
```

![image-20240306203157613](/Users/zhangpang/Library/Application Support/typora-user-images/image-20240306203157613.png)



##### 面试官问：如果你在uniapp中，做了一个地图（腾讯地图），你完成这个效果的流程是什么？

```
1. 申请开发者密钥（key）：申请密钥

2. 开通webserviceAPI服务：控制台 ->应用管理 -> 我的应用 ->添加key-> 勾选WebServiceAPI -> 保存(小程序SDK需要用到webserviceAPI的部分服务，所以使用该功能的KEY需要具备相应的权限)

3. 下载微信小程序JavaScriptSDK，微信小程序JavaScriptSDK v1.1   JavaScriptSDK v1.2

4. 安全域名设置，在小程序管理后台 -> 开发 -> 开发管理 -> 开发设置 -> “服务器域名” 中设置request合法域名，添加https://apis.map.qq.com
```

#### 4.2 小程序 或 app都可以打包一个webView页面（打开一个网页）

```
场景：做活动（短期的），做h5页做，就不需要在每一个端都把代码重新写一遍，再发布了。

<web-view v-if='isShow' src="https://www.baidu.com/"></web-view>

***注意：需要拿v-if来做判断，v-show不行。web-view 会自动跳转页面
```

##### 4.3 样式布局

```
设计图尺寸「宽度」：750
调试：iphone6/7/8 : 375
布局单位：rpx
```

##### 4.4 小程序上线的流程

```
1. 拿到 本小程序 的开发权限
2. 拿到 本小程序 的appid
3. 小程序网页后台的接口等等都配置好了
4. 可能提示包过大：需要在开发的时候就把分包做好
5. 以上4步没问题，在开发者工具，直接点上传就可以了，然后进入网页后台到审核版本
```

##### 4.5 小程序包体积大小

```
微信小程序的分包大小有以下限制：

	单个分包/主包大小：不能超过2M。
	总体分包大小：不能超过16M。
	总包大小：不能超过20M。
```

##### 4.6 UI配置

```
1. 打开链接：https://ext.dcloud.net.cn/plugin?name=uview-plus
	导入到项目中
2. 配置参考：https://uiadmin.net/uview-plus/components/downloadSetting.html
```

##### main.js

```
// main.js
import uviewPlus from '@/uni_modules/uview-plus'

// #ifdef VUE3
import { createSSRApp } from 'vue'
export function createApp() {
  const app = createSSRApp(App)
  app.use(uviewPlus)
  return {
    app
  }
}
```

##### uni.scss

```
/* uni.scss */
@import '@/uni_modules/uview-plus/theme.scss';
```

##### App.vue

```
<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "@/uni_modules/uview-plus/index.scss";
</style>
```

##### 安装依赖库

```
npm i dayjs
npm i clipboard
```

##### pages.json配置

```
{	
	"easycom": {
		// 注意一定要放在custom里，否则无效，https://ask.dcloud.net.cn/question/131175
		"custom": {
			"^u--(.*)": "@/uni_modules/uview-plus/components/u-$1/u-$1.vue",
			"^up-(.*)": "@/uni_modules/uview-plus/components/u-$1/u-$1.vue",
			"^u-([^-].*)": "@/uni_modules/uview-plus/components/u-$1/u-$1.vue"
		}
	},
}
```



# 五、uniapp开发多端出现的跨域问题

##### 5.1 小程序

```
开发阶段：不校验url
生产阶段：后台配置request合法域名
```

##### 5.2 h5

和vue-cli去配置代理==》在项目跟目录新建vite.config.js

##### 5.3 app和h5一样

和vue-cli去配置代理==》在项目跟目录新建vite.config.js



***注意：小程序不支持【vite.config.js】文件的配置，所以代理不生效。



# 六、uView2.x

##### 6.1 引入和使用

```
参考这里：https://www.uviewui.com/components/button.html
```

##### 6.2 http请求的配置

```
参考链接：https://www.uviewui.com/js/http.html
```



# 七、分包

注意：在项目一开始就需要配置。

```
7.1 源码视图
		"mp-weixin" : {
        "appid" : "",
				"optimization":{"subPackages":true},
		},
		
7.2 pages.json 
    {	
      pages:[
        ...
      ],
      "subPackages": [
        {
          "root": "login",
          "pages": [{
            "path": "login",
            "style": {
              "navigationBarTitleText": "登录页"
            }
          }]
        }
      ]
    }
```



# 八、微信一键登录

##### 8.1 uniapp做微信小程序一键登录流程的几种方式

```
一、 点击登录按钮 ===》 绑定手机号【选择｜不选择】 ==》 我的页面
二、 点击登录按钮 ===》 我的页面 【有一个设置，你可以设置你的手机号来绑定】
三、 点击登录按钮 ===》 跳转自己的一个页面，进行手机号和验证码的操作 ==》绑定手机号
```

```
面试中回答：
	1. 调用uni.login方法
			会返回：code码--》用户登录临时凭证
	2. 调用后端提供的接口：微信一键登录接口
		前端给后端传递：code码--》用户登录临时凭证
		这时候后端给前端返回的数据会有“2种结果”
	3. 结果一：如果返回的code为200，那就证明这个用户在本小程序，之前注册过或登录过，那么就会直接返回token和用户的信息等内容（当然现在的小程序已经不能直接显示用户的头像和昵称了：小程序官方的修改，如果用户需要修改昵称之类的，需要在本小程序我们做一个修改用户的操作，用户自己来进行修改）
	4. 结果二：如果返回的code为60003，那么就证明这个用户在本小程序，之前没有注册过 或 登录过，那么就会走下面的流程：
		4.1 调用后端提供的接口：注册微信用户
			需要把rawData , signature , iv , encryptedData ,openid , sessionKey , unionid传递过去
			
			其中：rawData , signature , iv , encryptedData通过uni.getUserInfo()方法获取
			其中： ,openid , sessionKey , unionid是在之前请求的【微信一键登录接口】接口中返回的。
		4.2 当把rawData , signature , iv , encryptedData ,openid , sessionKey , unionid参数传递过去请求成功时【注册微信用户】接口会返回：member , token  ,wechat ，则代表注册成功。
		
	5. 另外【小程序】需要绑定手机号，因为业务不同，所以绑定手机号的方式不同，比如可以通过open-type形式或者自己写一个input手机号和给用户发验证码的功能来自定义的去做也可以。
```

#### 注意：在微信小程序中，没有cookie也没有localStorage，只有 : wx.Storage 或 uni.Storage



##### 8.2 在登录时会弹出，【隐私xx保护】

```
在 2023年9月15日之前，在 app.json 中配置 __usePrivacyCheck__: true 后，会启用隐私相关功能，如果不配置或者配置为 false 则不会启用。

在 2023年9月15日之后，不论 app.json 中是否有配置 __usePrivacyCheck__，隐私相关功能都会启用。
```

#####  

# 九、uniapp跳转页面的方式和传值的方式

##### 9.1 跳转页面的方式

```
uni.navigateTo() ==>可以返回上一页
uni.redirectTo() ==>不可以返回上一页
uni.reLaunch() ==> 关闭所有页（不可以返回）
uni.switchTab() 跳转tabbar页面
uni.navigateBack() ==>返回上一页，可以设置返回第几个页面
```

##### 面试题一：

```
问：跳转的层级最多是多少？

回答：最多10层
```

##### 面试题二：

```
问：如果超过10级怎么办？

回答：getCurrentPages()方法获取栈中的页面数，返回上一页就是 length-2，但是跳转也要redirectTo跳转所以整体的写法就是：
				uni.redirectTo({
						url: '/pages/news/index?id=3'
				})
```

##### 补充：

```
uni.navigateBack(); //返回上一页，如果要做返回上一页，并且传值（那么可以以下方法，其实是给上一个组件重新在data中添加了一个属性，如果相同就覆盖）
				//获取页面的实例栈
				let currentPage = getCurrentPages();
				//获取当前页的上一个栈（上一个页面)
				let parent = currentPage[currentPage.length-2];
				//修改上一个页面的path值
				parent.$vm.path = item;
				//返回上一页
				uni.navigateBack();
```



# 十、小程序的分享（全局分享）

##### 10.1 可以通过混入来完成

```
全局开启混入，里面写生命周期！
```

##### 10.2 实现流程

```
//1. main.js  ==> 开启全局混入

import share from '@/mixins/share.js'
Vue.mixin(share);

//2. mixins文件
export default{
	//分享给好友
	onShareAppMessage( res ){
		return {
			title:'123',
			imageUrl:'../static/logo.png',
			path:'/pages/my/my'
		}
	},
	//分享到朋友圈
	onShareTimeline( res ){
		console.log( res );
	}
}
```



# 十一、数据的优化

```
1. 第一次进入：请求10条数据
2. 等滑动完10个：再请求10条数据
3. 不管数据多少个，dom永远都是少量的，比如3个
```



# 十二、规格

```
sku是规格，是数组结构
	[
		{name:'颜色'},
		{name:'尺寸'},
	]
	

spu是规格的子项
	{
		'id':['黑色','白色'],
		'id':['小码','大码']
	}
```

面试题一：

```
问：商品是单规格还是多规格。
回答：多规格
```

面试题二：

```
问：那么多规格的数据结构是什么样子的
回答：很麻烦，很复杂，有一个数组，存放规格，例如一个数组，成员是对象，比如3个规格，就是3个对象，然后还有一个skus是当前商品所有的规格子项，例如颜色：黑，尺寸：小，另外一个对象可能是，颜色：百，尺寸：大。反正就是数组中有对象，有对象有数组，有的返回的还是字符串，还需要json.parse转换成对象，可麻烦了。
```

面试题三：

```
问：你们的库存是怎么判断的？
回答：啊？选中某一个规格，对象里面有一个key就是库存呀，这个肯定是后端返回的呀。
```



# 十三、微信小程序支付流程

```
1. 点击【提交订单】按钮：判断用户是否选择了收货地址。

2. 前端请求后端接口：下单接口
	前端传递参数 ： [商品的spuid、skuid、购买数量]、[时间戳]、[收货地址id]、[备注]
	后端返回数据 ： 订单编号（不重要）、小程序支付参数（重要）:nonceStr,paySign,signType,timeStamp
	
3. 接着调用：uni.requestPayment()方法，把小程序支付参数传递过去
	支付成功返回：requestPayment:ok
	支付失败返回：requestPayment:fail cancel

4. 不过是支付成功或者失败，最后都跳转到订单页

5. 如果未支付==》订单页 ：待支付 
6. 如果支付成功==》订单页：待发货  ===> 申请退款（无理由退款）
	退款流程：前端请求后端接口==》传递订单id号，直接退回到原来的地方。
```



# 十四、微信小程序上线流程

```
1. 保证有appid和开发者身份
2. 做好分包
3. 在开发者工具点击上传即可
4. 上传后，后台需要提交审核，到审核阶段，审核完成就可以了
```



# 十五、开发管理=》接口设置

```
15.1 地理位置、视频、直播
	各种都需要开通，包括收货地址
15.2 getSetting、getUserInfo、login
	有调用额度设置：1000次/30元人民币
```



# 十六、聊天API

```
环信、腾讯IM

微信小程序接入客服：<button open-type="contact">咨询</button>

文章：https://blog.csdn.net/FleeMeng/article/details/125178623

面试题：1. 你们的客服功能怎么做的？
			 2. 你们这个客服，如果A客服下班了，B客服怎么接入（这个问题，在腾讯IM上需要设置，在小程序上不需要程序员管）
```



# 十七、收到货：退款/退货流程

```
1. 订单已完成==》点击【退款/退货】
2. 进入【退款/退货】页面，传递对应数据
    {
      "orderNumber": "",//订单号
      "returnsType": "",//类型（1：退货；2：换货）
      "isReceived": "",//是否收到货（1：已收货；0：未收货）
      "receiveName": "",//收货人姓名（寄回地址，已收货时必填）
      "receiveAddress": "",//收货人地址（寄回地址，已收货时必填）
      "receiveMobile": "",//收货人电话（寄回地址，已收货时必填）
      "expressCompanyId": "",//快递公司ID，已收货时必填
      "expressNumber": "",//快递单号，已收货时必填
      "reasonType": "",//原因类型
      "supplementDesc": "",//补充描述
      "voucherImages": "",//凭证截图（多个用英文逗号分隔）
  }
 
 3. 提交申请==》后台管理系统（客户沟通）
 4. 沟通完毕（退款/退货：后台操作==》小程序展示） ： 退款/退货详情页展示
```



# 十八、关于项目的细节问题

##### 项目名称（写在简历中的名称）：胖子手办社

##### 项目开发周期：

```
产品和ui：1个多月
后端：2个人（工作：40天）
前端：2个（小程序：工作日30天，后台管理系统工作日：40天）

整体项目：预算：5W、工作日完成：50天。
```

##### 项目开发人员：

```
产品：1（不是全天干一个）
ui：1（不是全天干一个）
前端：2（后台管理系统、小程序）
后台：2
```

##### 项目的优化：

```
首屏的优化（和虚拟列表思想一样）
```

##### 项目上线时间：

```
22年5月
```

##### 项目的兼容：

```
uniapp开发的，都是在处理条件编译的事情，不过只是做小程序，也不涉及到。
```

##### 项目的源码：

```
面试问：可以看看吗？你打开让他看。
面试问：源码，我有全部的（sql、后端（java）、小程序、后台管理系统），我可以拿着电脑，让你看，但是我不能给你，因为有保密协议！
```



