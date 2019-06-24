## Vue扩展

### 1. 双向绑定

#### 1.1. MVVM
* View: 界面
* Model: 数据模型
* ViewModel: 作为桥梁负责沟通View和Model，只关心数据和业务的处理，不关心View如果处理数据

#### 1.2. Angular的脏数据监测
* 当触发了指定事件后会进入脏数据检测，这时会调用`$digest` 循环遍历所有的数据观察着，判断当前值是否和先前的值有区别，如果检测到变化的话，会调用`$watch`函数，然后再次调用`$digest`循环直到发现没有变化。循环至少二次，至多十次

#### 1.3. Vue的数据劫持
* Vue内部使用了`Object.defineProperty()` 来实现双向绑定，通过这个函数可以监听到`set`和`get`的事件
* 缺点：
	* 只能对属性进行数据劫持，所以需要深度遍历整个对象
	* 对于数组不能监听到数据的变化
* Proxy 原生支持监听数组变化，可以直接对整个对象进行拦截

### 2. 前端路由

#### 2.1. 原理：
* 监听URL的变化，然后匹配路由规则，显示相应的页面，并且无须刷新
	* hash模式，通过`hashchange`事件监听URL的变化
	* history模式
* 优缺点
	* 优点：用户体验好，不需要每次都从服务器全部获取，快速展现给用户
	* 缺点：
		* 使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存
		* 单页面无法记住之前滚动的位置，无法在前进，后退的时候记住滚动的位置

### 3. Virtual Dom
* 操作DOM是很耗费性能的一件事情，可以考虑通过JS对象来模拟DOM对象
* 实现流程：
	* 通过JS来模拟创建DOM对象
	* 判断两个对象的差异
	* 渲染差异

### 4. VUEX
* 有一处需要被多个实例间共享的状态，可以简单地通过维护一份数据实现共享

```
https://github.com/vuejs/vuex
```

#### 4.1. state
* 包含了所有应用层级状态的一个对象

```
computed: {
	count () {
		return this.$store.state.count
	}
}
// 每当store.state.count变化时，
都会重新求取计算属性，并且触发更新相关联的DOM
```
或

```
computed: mapState({
	count: state => state.count
})

```

#### 4.2. getter
* 从store中派生出的一些状态，如果有多个组件用到state方法运算，要么复制这个函数，或者抽取到一个共享函数然后在多处导入它。
* getter的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算
* mapGetters

#### 4.3. mutation
* 更改Vuex的store中的状态的唯一方法是提交mutation
* 每个mutation都有一个字符串的事件类型和一个回调函数
* 同步事务

```
mutations: {
	increment (state) {
		// 变更状态
		state.count++
	}
}
store.commit('increment')
```

#### 4.4. action
* 提交的是mutation，而不是直接变更状态
* Action可以包含任意异步操作

```
store.dispatch('increment')
```

#### 4.5. 模块 - module
* 每个模块拥有自己的state、mutation、action、getter、甚至是嵌套子模块 - 从上至下进行同样方式的分割
* namespaced 命名空间模块

### 5. Vue-router路由库
* 判断需要跳转的路由是否存在于记录中，然后执行各种导航守卫函数，最后完成URL的改变和组件的渲染

```
https://router.vuejs.org/
```

### 6. 单元测试
* vue/test-utils	// 官方的单元测试实用工具库

```
https://vue-test-utils.vuejs.org/zh/
```

* Jest 测试运行器，单元测试解决方案

```
$ npm install --save-dev jest @vue/test-utils
$ npm install --save-dev vue-jest		// `vue-jest` 预处理器，为了告诉Jest如何处理 `*.vue` 文件
```

### 7. Nuxt.js


### 11. 服务端渲染
* Nuxt.js 基于Vue生态的更高层的框架，为开发服务端渲染的Vue应用提供及其便利的开发体验。可以用它来作为静态生成器

```
https://ssr.vuejs.org/zh/
```

* 服务端渲染应用框架，更关注UI渲染
* 异步数据加载、中间件支持、布局支持等
* 使用Webpack和vue-loader、babel-loader来处理代码的自动化构建工作(如打包、代码分层、压缩等等)
* 特性：
	* 基于Vue.js
	* 自动代码分层
	* 服务端渲染
	* 强大的路由功能，支持异步数据
	* 静态文件服务
	* ES6/ES7 语法支持
	* 打包和压缩JS和CSS
	* HTML头部标签管理
	* 本地开发支持热加载
	* 继承ESLint
	* 支持各种样式预处理器：SaSS、LESS、Stylus等等

### 8. Vue优化

#### 8.1. webpack打包配置，使vue编译后的文件尽可能的小
* 通过 `webpack-bundle-analyzer` 工具，执行 `npm run build --report`，浏览器会打开一个页面，展示编译后的文件大小及各部分内容大小
* 按需加载优化
* 使用CDN外部加载
	* 直接在index.html引入，在webpack配置中告知编译器，在 `webpack.base.config.js`中：

	```
	module.exports = {
		externals: {
			"echarts": "echarts"
		}
	}
	```
* 针对性的对库优化，例如简单交互自己写或者换成更加轻量级的
* 服务器端开启gzip
	* 使用gzip进一步压缩文件，使得服务器传递给浏览器的文件是经由压缩之后的，待浏览器收到之后再解压缩。需要服务器端的支持
* 服务端渲染
	* vendor.js 第三方库部门

#### 8.2. 改善少数营销页
* 预渲染：无需使用web服务器实时动态编译HTML，在构建时简单地生成针对特定路由的静态HTML文件

#### 8.3. 浏览器渲染和服务端渲染
* 浏览器渲染：在浏览器中输出Vue组件，进行生成DOM和操作DOM。
* 服务端渲染：将同一个组件渲染为服务端的HTML字符串，将他们直接发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序
* vue-server-renderer

* 优缺点
  * 更好的SEO，抓取工具不会等待ajax异步请求完成之后再抓取页面内容
  * 更快的内容到达时间，不需要等到KS完成下载并执行，才显示服务端渲染的标记
  * 开发条件所限，有些生命周期钩子函数只能在浏览器中执行；一些外部扩展库可能需要特殊处理，才能在服务器中运行
  * 涉及构建设置和部署更多要求，需要处于Nodejs server运行环境
  * 更多的服务器负载。在Node.js中渲染完整的应用程序，比仅仅提供静态文件的server更加大量占用CPU资源。


### 9. 框架比较

#### 9.1. React和Vue
* 相似处：
  * 使用Virtual DOM，是对由Vue组件树建立起来的整个VNode(虚拟节点)树的称呼
  * 提供了响应式(Reactive)和组件化(Composable)的视图组件
  * 将注意力集中保持在核心库，将其他功能如路由和全局状态管理交给相关的库
  * React比Vue有更丰富的生态系统

* 差异性：
  * JSX和Templates
  	* React中所有组件的渲染功能都依靠JSX。是使用XML语法编写JavaScript的一种语法糖
  	* Vue的templates更符合规范的HTML
  * 组件作用域内的CSS
  	* React中一切都是JavaScript，CSS也纳入JavaScript中处理
  	* 单文件组件的形式，可以在同一个文件里完全控制CSS，将其作为组件代码的一部分

#### 9.2. ReactNative和Weex
* React Native 使用相同的组件模型编写有本地渲染能力的APP(IOS和Android)。能同时跨多平台开发。
* Weex：阿里的跨平台用户界面开发框架，允许使用Vue语法开发不仅仅运行在浏览器端，还用于iOS和Android上原生应用的组件
* 成熟度不够


### 10. 生产环境部署(再研究)
* vue-cli

```
https://cli.vuejs.org/zh/config/
```

* webpack

```
https://webpack.js.org/configuration/
```

* Browserify
* Rollup
* vue-loader

```
https://vue-loader.vuejs.org/zh/
```
