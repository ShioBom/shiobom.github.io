---
title: vue学习笔记三
date: 2019-10-20 16:27:14
tags:
---

> 1. `@`指向src根目录
> 2. 采用require方式引入的路由，这种写法可以实现按需加载。

### css预处理

** stylus css 预处理器**

- 括号和分号可省略，冒号也可省略，但一般不省略冒号
- 以缩进表示子选择器
- `&`是父级选择器的引用

**安装**

```
yarn add stylus
yarn add stylus-loader
```

**例子**

```stylus
$background-color = lightblue
ellipsis()
	overflow: hidden;
	text-overflow:ellipsis;
	white-space: nowrap;
add (a, b = a)
    a = unit(a, px)
    b = unit(b, px)
    a + b

.list-item
.text-box
    span
	    ellipsis()
        background-color: $background-color
        margin: add(10)
        padding: add(10, 5)
    &:hover
        background-color: powderblue
```

编译后生成的代码

```css
.list-item span,.text-box span {
  background-color: #add8e6;
  margin: 20px;
  padding: 15px
}
.list-item:hover,.text-box:hover {
  background-color: #b0e0e6;
}
```

> vue2.0 需要自己配 webpack，3.0 则只需要安装相关依赖就好。


### vuex

核心是 store，包含着应用中的大部分状态  
Mutations 存放的是改变 state 的方法，更改store的状态的唯一方法是提交 mutation
Actions 存放的是一些业务逻辑，通常是异步任务。

**安装**

```
yarn add vuex
```

**使用**

```
import Vue from ‘vuex’
import Vuex from 'vuex'
Vue.use(Vuex)
```

==推荐使用辅助函数==
- state
- Getter
可以认为是store的计算属性，getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。
- Mutation
mutation必须是同步函数
- Action
通过提交 mutation 实现状态更新，而不是直接更新状态
可以包含异步操作
- Module
模块化状态,对每个模块添加命名空间(重要)

### transition

Vue在插入、更新或者删除时，提供多种不同方式的应用过渡效果

```
<transition
	enter-class=""
	enter-active-class="animated fadeInLeft"
	enter-to-class=""
	leave-class=""
	leave-active-class="animated fadeOutLeft"
	leave-to-class=""
>
	<div class="tmp" v-if="isShow">div</div>
</transition>
```

动画钩子函数

```
<transition
	v-on:before-enter="beforeEnter"
	v-on:enter="enter"
	v-on:after-enter="afterEnter"
	v-on:before-leave="beforeLeave"
	v-on:leave="leave"
	v-on:after-leave=“afterLeave”
	v-on:leave-cancelled="leaveCancelled"
>
methods:{
	...
	enter:function(el,done){
		done() // 表示完成动画
	}
}
```

### vue-router

**安装**

```
yarn add vue-router
```

**使用**

- 在main.js里
```
import VueRouter from 'vue-router'
Vue.use(VueRouter)
new Vue({
	el:"#app",
	router,
	render:h=>h(App)
})
```
- 配置路由
```
var routes =[
	{path:'/',component:ind},
	{path:'/msg',component:msg},
	{path:'/hi',component:hi}
]
```
界面
```
<div>
	<router-link to="/">ind</router-link>
	<router-link to="/msg">msg</router-link>
	<router-link to="/hi">hi</router-link>
	<router-view></router-view>
</div>
```
**路由模式**

- history：常用的模式
- hash：哈希，网址含`#`
- abstract：网址不变

**路由传参**

- 通过params

```
{path:"/msg/:id/:msg",component:msg}
<router-link to="/msg/1/2">msg</router-link>
```
这样的传参通过`this.$route.params.age获取`

- 通过query
```
{path:"/msg",component:msg}
<router-link to="/msg?a=1&b=2">msg</router-link>
```

**编程式导航**

```
this.$router.push("/")
this.$router.push({path:'/msg',query:{a:1,b:2}})
```
这样的传参通过`this.$route.query获取`

- 导航方式总结

|  声明式导航 | 编程式导航 |   说明     |
| :--------  | --------:| :------: |
| `<router-link to="/">`         |  this.$router.push('/') | 向history中添加记录 |
| `<router-link to="/" replace>` |this.$router.replace('/')| 不会向history中添加记录|
| ————|  this.$router.go(1)  |  历史记录中前进一页  |

> params和query传参的区别，params 刷新页面，数据会丢失，解决办法是采用浏览器缓存（sessionStorage和localStorage）或者cookie将数据缓存下来。而query会把数据暴露在地址栏中， 类似get请求。

**嵌套路由规则**

```
{
	path:"/list",
	component:List,
	children:[
		{path:"/list/a",component:PageA},
		{path:"/list/b",components:PageB}
	]
}
```

### keep-alive

`<keep-alive> `是一个抽象组件，包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们
- **props**

	- `include`：字符串或正则表达式。只有名称匹配的组件会被缓存。
	- `exclude`：字符串或正则表达式。任何名称匹配的组件都不会被缓存。
	- `max`：最多可以缓存多少组件实例。

- **keep-alive 提供了两个新的生命周期钩子函数**
	- `actived`:keep-alive 组件激活时调用。
	- `deactived`:keep-alive 组件停用时调用。

## axios

可以添加拦截器，请求拦截器和响应拦截器。在里面可以做 toast 提示

