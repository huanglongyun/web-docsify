# 一、nextTick

是什么：就是一个异步操作（nextTick返回一个异步promise）

场景：异步加载执行一些代码，比如echarts或者其他，总之要解决的问题，就是一个插件要有dom才可以加载进入，放在一些生命周期中，加载不出来，使用异步加载，让他出来

# 二、keep-alive

是什么：vue内置组件，用来缓存组件的

场景：切换组件，可以缓存部分组件，来提供用户体验和性能，比如后台管理系统的标签页切换，A标签页填写很多东西切换到B，再回到A，A添加了很多数据还在。



优势：1.  提升用户体验 。 2. 提升性能

缓存不缓存组件：可以给keep-alive添加：include=[]，然后给组件添加name（vue3用插件），额外2个生命周期也可以判断。

# 三、动态组件

是什么：动态的组件

场景：tabs组件（切换不同的组件）



# 四、组件传值：父=》子

4.1  父亲==》 ：单项绑定 ， 子props接收

4.2  v-model传值 

```
父：<子组件 v-model='xxx'>
子：modelValue接收值，但是要注意，虽然是v-model传值，但是传过来的值是：
const props = defineProps({
    modelValue:{
        type:String,
        default:''
    }
})
接收，也是单向数据流。

场景：很多富文本编辑器都是modelValue接收值
```

4.3 如果层级比较多，可以通过依赖注入的形式(单纯的父子关系也行) 

4.4 attr传值（不常用）



# 五、组件传值：子=》父

```
自定义事件@xxx=xxx
```



# 六、兄弟传值：bus

```
vue2 : event bus
vue3 : event bus 、 插件（vue3建议使用插件完成：原理就是event bus）
```



# 七、mixin（混入）

```
混入有俩种方式：
	1. 全局 	main.js
	2. 组件内 某一个组件内
```

```
使用场景：
	1. uniapp：做微信小程序分享功能（生命周期），需要在某一个页面加入“生命周期”（每一个页面都加太麻烦了，所以弄一个全局混入文件，混入中可以写生命周期，这样就全局开启了）
	
	2. 后台管理系统：字典。
		字典：一个后台管理系统会配置很多，用来做固定值的，比如性别，男、女、未知，就可以弄成字典，在不同组件直接使用。
```

```
混入的缺点：在组件中使用混入，数据来源不明确，难以维护管理。
```

##### 面试官：

```
在vue中有哪些可以做成全局的？
	1. mixin
	2. Vue2中还可以(main.js)：Vue.prototype.run = function(){}
	3. store，他是use进去==》use进去的都是全局的
	4. 可以写一个插件，use进去
	
请问：vue.prototype和混入的区别是什么？
	1. mixin是直接在对象本身添加的属性和方法 。 Vue.prototype.run是在Vue原型中添加的
			对象查找属性规格：对象本身==》原型
  2. mixin({   这里可以写.vue文件中script所有内容：生命周期，数据，方法... })
```

##### 说明：

```
混入在vue3中不推荐使用，hooks可以代替。但是hooks里面也不行写生命周期。
```



# 八、directive（自定义指令）

```
场景：后台管理系统 ： 按钮级别的权限控制
```



# 九、use()

```
场景：写插件的（全局插件）可以控制dom，本身use的是js｜ts文件。 通过install来注册
```

面试题：use做了什么？

```
1. 是什么：在 Vue3 中，app.use() 是用来封装插件的方法。就是一个全局方法。
2. 原理：
	当你调用 app.use，会执行参数一的install方法，install方法中接收一个app参数（就是vue本身），可以在app这个参数扩展：全局组件、指令、混入...
	install内部可以，给vue对象添加一个全局的方法
		export default{
    	install ( app ) {
        //全局方法
        app.config.globalProperties.$countDown = downParams;
     	}
    }
```



# 十、冻结对象

```
Vue2没有官方提供api，只能通过ES6的：Object.freeze()
Vue3官方提供了api：markRaw()
```

##### 面试官问：

```
如何让一个对象中的值，不能修改？

回答：冻结！Object.freeze()
```



# 十一、setup纯语法糖形式拿到this（全局属性）

```
<script setup>
import { getCurrentInstance } from 'vue'
const { proxy } = getCurrentInstance();
console.log( proxy.$a )
</script>
```

##### 补充：给以上写法的组件添加name，使用插件：

```
//设置组件name
import VueSetupExtend from 'vite-plugin-vue-setup-extend'
```



# 十二、store原本state有一个a:1，在组件中使用改变成2，刷新又回到了1怎么办？

```
安插件持久化存储插件去。
```



# 十三、导航守卫

```
三大类：全局、组件内、路由独享

场景：后台管理系统（ 判断token 、 动态添加路由 ）
```



# 十四、Vue + Vite获取文件路径

```
const modules = import.meta.glob("../views/**/*.vue");
```



# 十五、双向绑定原理

```
单项绑定：v-bind
双向绑定：v-model
```

1. Object.defineProperty 对比 new Proxy() 区别

   ```
   循环情况：
   	new Proxy不需要循环就可以拿到原对象的所有属性和方法
   	Object.defineProperty需要循环原对象，才可以拿到对应的所有属性和方法
   ```

   ```
   关于后添加属性的问题：
   	new Proxy：即使原对象后添加的属性，也可以添加到new proxy中，还可以劫持到属性的变化
   	Object.defineProperty ：原对象的属性是后添加的，那么该属性虽然也在Object.defineProperty对象中，但是劫持不到他的变化。
   		***只要是后添加的
   ```

2. 关于以上区别，vue2解决数据更新，视图不更新使用：$set

   ```
   注意：vue3，不需要$set
   ```

##### 面试回答双向绑定原理：

```
首先Vue2的双向绑定中核心API是：Object.defineProperty
Vue3的核心API是：new Proxy

那么这里先说一下，这俩API的区别。其实有几点区别，但是最重要的是：Object.defineProperty后添加的属性的不会被劫持到，而new Proxy可以。当然还有其他区别，比如Object.defineProperty是3个参数，第二个参数是属性，而proxy是2个参数，所以Object.defineProperty要原对象的所有属性就需要循环。

好

那么Object.defineProperty 和 new Proxy解释清楚后，那么就具体说双向绑定的原理：

首先我认为，双向是先有单项，所谓的单项简单来说就是把 数据 通过初始化 模版解析到视图中 展示数据，那么在vue中。

第一步：通过Object.defineProperty或new Proxy 来把data中定义的数据劫持到。
第二步：通过compile方法来做模版解析的操作。
	***当然，要考虑到双向绑定，那么就要在compile中执行watcher，那么watcher中包含了更新视图的方法。
第三步：通过给某节点添加v-model属性，先去改变数据，那么就是把value赋值给对象的某属性等于的值。
第四步：当值发生改变时，在之前（第一步）的API方法中的set方法，是可以得到通知的，那么在set中，执行（第二步）watcher中的update方法，来更新视图。从而完成双向绑定的操作。
```



# 十六、nextTIck原理

```
nextTick接收一个回调函数的参数，这个参数（函数），只要是异步执行即可!
```



# 十七、keep-alive原理



# 十八、computed原理



# 十九、diff算法



# 二十、:key是干什么的？需要注意哪些？



```
2. 需要注意哪些？
	如果当前元素是循环，并且添加了:key，如果这个元素 后期 可能涉及到删除，修改等等操作，那么key的只就不要是index。
```



# 二十一、Vue2 和 Vue3区别

1. ##### 双向绑定使用api不同

   ```
   Vue2用的是：Object.defineProperty 
   Vue3用的是：new Proxy() 区别
   
   ***然后展开说这2个api的区别（文档上面有写）
   ```

   所以在vue2为了解决Object.defineProperty 后添加值监听不到的问题（数据更新、视图不更新）所以出了一个api：$set，而vue3不存在这个问题，所以就没有$set。

2. ##### Vue3提出了弱化混入，强化hooks

   ```
   展开说：混入的缺点 和 hooks的封装使用
   ```

3. ##### 举例很多api的不同

   ```
   v-if和v-for优先级调整了
   ref调整了
   ...
   ```

   

# 二十二、数据更新了，视图不更新

总结：就是数据没有响应式（没有被劫持监听到）

```
Vue2 : 
	数据层面：$set、$forceUpdate（强制更新，不推荐使用）
Vue3 : 
	$forceUpdate（强制更新，不推荐使用）
	
$nextTick（异步操作）
```



1. ##### Vue3

   ```
   <script setup>
   import { ref , reactive } from 'vue'
   let form = reactive({
     name:'',
     age:0
   })
   
   const btn = ()=>{
     //失去响应式
     // form = {
     //   name:'李四',
     //   age:19
     // };
     //浅拷贝
     Object.assign( form , {
       name:'李四',
       age:19
     })
     console.log( form );
   }
   </script>
   ```

2. ##### Object.defineProperty的问题导致没有劫持监听到（是api本身的问题）

   ```
   <script>
   export default {
     name: 'App',
     created(){
       this.b = 2;
     },
     methods:{
       btn(){
         this.b = 3333;
         console.log( this.b );
       }
     }
   }
   </script>
   ```

   
