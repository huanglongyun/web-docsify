##### 安全总结：

```
1. 不被恶意消耗 “钱”
2. 不被恶意消耗 “资源”
3. 避免被当 “枪使”
```

# 一、防止短信被刷

##### ip不同、手机号不同、时间不同，都可以防止

```
1. 显示滑块：前端 发送 请求
		前端 给 后端传递的参数：uuid（随机字符串）、ts（现在的时间戳）
		后端 给 前端返回的数据：token、secretKey、图片路径
2. 当用户滑动成功：前端 发送 请求
		前端 给 后端传递参数：token（之前后端给的）、pointJson（secretKey[之前后端给的]，只不过再次用AES加密一下）
		后端 给 前端返回的数据：token（和之前的一样不变）、pointJson（和之前一样）

3. 滑动成功后==》父组件接受一个自定义事件==》在这里再次请求接口【发送短信验证码】
		前端 给 后端传递的参数：mobile（用户输入的手机号）、captchaVerification（就是使用：pointJson，可能会再次加密，由后端判断）。
		如果一切OK，后端对接短信云给用户手机发送短信。
```



# 二、跨站请求伪造（CSRF）

其实俩种方式都可以解决csrf的问题。

```
1. 通过referer来判断来源
	router.use((req,res,next)=>{
    const referer = req.get('Referer');
    if(!referer || !referer.startsWith('http://localhost:5173/') ){
      return res.status(403).send('来源不对');
    }
    next();
  })

2. 通过令牌来校验

	在正式请求之前，先请求获取令牌的接口（后端生成给的）
	请求当前接口：把之前拿到的令牌传递过去，后端会判断是否一致（如果不一致返回403）
```



# 三、点击劫持

```
攻击者弄一个透明的iframe潜入到【目标网站中，并且iframe是透明的：用户感知不到】。用户在目标网站按钮，可能点击点就是顶层的iframe了，这里就有安全的隐患。
```

前端防止

```
<script>
if( window != window.top ){
  window.top.location.href = window.location.href;
}
</script>
```



# 四、内容安全策略（CSP）

前端防护添加meta

```
<meta 
      http-equiv="Content-Security-Policy"
      content="default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:"
    >
```



# 五、XSS

```
1. 用户添加内容的时候（输入内容），最后生成内容到网页中，比如正则==》转义后再添加到网页中。
2. 可以通过内容安全策略的限制来做。
```











