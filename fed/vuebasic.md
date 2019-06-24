## Vue基本概念

#### 1. 概念

<a id="lifecycle">&nbsp;</a>
#### 1.1. Vue生命周期
|钩子|作用|备注|
|:---:|---|:---:|
|beforeCreate|实例初始化之后，数据观测和event/watcher事件配置之前||
|created|实例创建完成之后立即调用，数据观测，属性和方法运算，watch/event事件回调|`$el`属性目前不可见|
|beforeMount|挂载开始之前被调用：相关的render函数首次被调用||
|mounted|el被新创建的`vm.$el`替换，并挂载到实例上去|`this.$nextTick`|
|beforeUpdate|数据更新时调用，发生在虚拟DOM打补丁之前|手动移除已添加的事件监听器|
|updated|由于数据更改导致的虚拟DOM重新渲染和打补丁|计算属性或watcher|
|activated|keep-alive组件激活时调用||
|deactivated|keep-alive组件停用时调用||
|beforeDestroy|实例销毁之前调用||
|destroyed|Vue 实例销毁后调用。所有都解绑定，事件监听器被移除，子实例也被销毁||
|errorCaptured|捕获来自子孙组件的错误时被调用|--|

<a id="slot">&nbsp;</a>
#### 1.2. 插槽slot：分发内容，提炼可复用的模板，这些模板可以基于输入的prop渲染不同的内容

 * 例如：项目的基础弹窗格式，通用的弹窗标题模式，右侧关闭按钮，底部按钮，基本只有内容不同，可以写一个基础dialog.vue组件，内容部分用slot。具体的业务弹窗直接用这个组件就可以。

* submit-dialog组件定义

```
<el-dialog>
	<slot name="header"></slot>
	<slot></slot>
	<slot name="footer"></slot>
</el-dialog>
```
* 应用submit-dialog组件

```
<submit-dialog>
	<template #header>
		头部
	</template>
	<p>登录弹窗</p>
	<template v-slot:footer>
		底部
	</template>
</submit-dialog>
```
* 作用域插槽
* 动态插槽名

#### 1.3. prop
  * 所有的prop使的父子prop之间形成了一个单向下行绑定：父级prop的更新会向下流动到子组件中，反过来不行。每次父级组件发生更新时，子组件中所有的prop都将会刷新为最新的值。
  * 注意：Js中对象和数组是通过引用传入的，所以对于一个数组或对象类型的prop来说，在子组件中改变这个对象或数组本身会影响父组件的状态

<a id="vmodel">&nbsp;</a>
#### 1.4. 自定义组件的v-model原理
 * 引入组件

 ```
 <base-checkbox v-model=“lovingVue"></base-checkbox>
 ```

 * 组件的定义

 ```
 Vue.component('base-checkbox', {
 	model: {
 		prop: 'checked',
 		event: 'change'
 	},
 	props: {
 		checked: Boolean
 	},
 	template: `
 		<input type="checkbox" :checked="checked" @change="$emit('change', $event.target.checked)" />
 	`
 })
 ```
 一个组件上的 `v-model` 默认会利用名为 `value` 的prop和名为 `input` 的事件，像单选框、复选框等类型的输入控件可能会将 `value` 特性用于不同的目的
 这里的 `lovingVue` 的值将会传入这个名为 `checked` 的prop。同时当 `<base-checkbox>` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的属性将会被更新。

#### 1.5. .sync 修饰符
* 对prop双向绑定，推荐以 `update:myPropName` 的模式触发事件。

```
this.$emit('update:title', newTitle)
```
父组件监控事件并根据需要更新一个本地数据属性。例如：

```
<text-document :title="doc.title" @update:title="doc.title=$event"></text-document>
```
缩写模式：

```
<text-document :title.sync="doc.title"></text-document>
```
* 当一个对象同时设置多个prop的时候，也可以将这个 `.sync` 修饰符和 `v-bind` 配合使用：

```
<text-document .sync="doc"></text-document>
```
这样会把 `doc` 对象中的每一个属性(如 `title`) 都作为一个独立的prop传进去，然后各自添加用于更新的 `v-on` 监听器

#### 1.6. 动态组件
* 可以用 `is` 特性来切换不同的组件：

```
<component :is="currentTab"></component>
```

* 用 `keep-alive` 缓存动态组件

```
<keep-alive>
	<component :is="currentTab"></component>
</keep-alive>
```

#### 1.7. 异步组件
* 以工厂函数的方式定义组件，异步解析定义的组件。Vue只有在组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。

```
Vue.component('async-example', function (resolve, reject) {
	setTimeout(function () {
		// 向 `resolve` 回调传递组件定义
		resolve({
			template: '<div>I am async!</div>'
		})
	})
})
# 调用 `reject(reason)` 表示加载失败，`setTimeout` 是为了演示用的
```

* 结合 `webpack`的`code-spliting` 功能一起使用：

```
Vue.component('async-example', function (resolve) {
	// 特殊的 `require` 语法将会告诉webpack
	// 自动将构建代码切割成多个包，这些包会通过 Ajax 请求加载
	require(['./async-component'], resolve)
})
```

* 也可以在工厂函数中返回一个 `Promise`

```
Vue.component('async-example', () => import('./async-component'))
```

#### 1.8. 访问元素&组件
* 访问根实例：$root

```
this.$root.foo
```

* 访问父级组件实例：$parent

```
this.$parent.map
```

* 访问子组件实例或子元素：ref

```
# 通过 `ref` 特性为子组件赋予一个ID引用
<base-input ref="usernameInput"></base-input>

# 在定义了这个 `ref` 的组件里面，可以使用
this.$refs.usernameInput.focus()
```

#### 1.9. 依赖注入 `provide ` 和 `reject`
* `provide` 选项可以指定提供给后代组件的数据/方法

```
provide: function () {
	return {
		getMap: this.getMap
	}
}
```

* 在任何后代组件里，可以使用 `inject` 选项来接收指定的想要添加在这个实例上的属性：

```
inject: ['getMap']
```

#### 1.10. 程序化的事件侦听器

```
$on(eventName, eventHandler)  	// 侦听一个事件
$once(eventName, eventHandler)	// 一次性侦听一个事件
$off(eventName, eventHandler)		// 停止侦听一个事件
```

* 程序化侦听器，每个实例都程序化地在后期清理自己：

```
mounted: function () {
	this.attachDatepicker('startDateInput')
	this.attachDatepicker('endDateInput')
},
methods: {
	attachDatepicker: function (refName) {
		var picker = new Pikaday({
			field: this.$refs[refName],
			format: 'YYYY-MM-DD'
		})

		this.$once('hook:beforeDestroy', function () {
			picker.destory()
		})
	}
}
```

#### 1.11. 单元素/组件的过渡动态
* `transition` 的封装组件，在下列情形，可以给任何元素和组件添加进入/离开过渡
	* 条件渲染(使用 `v-if`)
	* 条件展示(使用 `v-show`)
	* 动态组件
	* 组件根节点

<a id="mixin"></a>
#### 1.12. 混入(mixin)
* 分发Vue组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被"混合"进入该组件本身的选项。
* 数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。
* 同名钩子函数将合并为一个数组，因此都将被调用。混入对象的钩子将在组件自身钩子之前调用
* 值为对象的选项，例如`methods`、`components`和`directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。
* `Vue.extend()` 也使用同样的策略进行合并。

<a id="directive"></a>
#### 1.13. 自定义指令
* 全局注入

```
Vue.directive('focus', {
	bind: () => {
		// 指令第一次绑定到元素时调用，主要是进行一次性的初始化设置
	},
	inserted: (el) => {
		el.focus()
	},
	update: () => {
		// 所在组件的VNode更新时调用，但有可能发生在其子VNode更新之前
		// 指令的值可能发生改变，也可能没有。可以通过比较更新前后的值来忽略不必要的模板更新
	},
	componentUpdated: () => {
		// 指令所在组件的VNode及其子VNode 全部更新后调用
	},
	unbind: () => {
		// 指令与元素解绑时调用
	}
})
```

* 局部注入

```
directives: {
	focus: {
		inserted: (el) => {
			el.focus()
		}
	}
}
```

#### 1.14. 渲染函数
* render、createElement

```
Vue.component('anchored-heading', {
	render: (createElement) => {
		return createElement(	// 建立虚拟DOM来追踪自己要如何改变真实DOM
			'h' + this.level,	// 标签名称
			this.$slots.default		// 子节点数据
		)
	},
	props: {
		level: {
			type: Number,
			required: true
		}
	}
})
```

<a id="plugin"></a>
#### 1.15. 插件
* 通常用来为Vue添加全局功能：
	* 添加全局方法或者属性。如：`vue-custom-element`
	* 添加全局资源：指令/ 过滤器/ 过渡等。如：`vue-touch`
	* 通过全局混入来添加一些组件选项。如：`vue-router`
	* 添加Vue实例方法，通过把它们添加到 `Vue.prototype`上实现
	* 一个库，提供自己的API，同时提供上面提到的一个或多个功能。如`vue-router`

* 使用插件

```
MyPlugin.install = function (Vue, options) {
	Vue.myGlobalMethod = function () {
		// 添加全局方法或属性
	}

	Vue.directive('my-directive', {
		bind (el, binding, vnode, oldVnode) {
			// 添加全局资源
		}
	})

	Vue.mixin({
		created: function () {
			// 注入组件选项
		}
	})

	Vue.prototype.$myMethod = function (methodOptions) {
		// 添加实例方法
	}
}
Vue.use(MyPlugin)
```

#### 1.16. 过滤器
* 被添加在JS表达式的尾部，由"管道"符号指示
* 全局定义过滤器：

```
Vue.filter('capitalize', function (value) {
	// ...
})
```

* 本地过滤器

```
filters: {
	capitalize: function (value) {
		// ...
	}
}
```

#### 1.17. 响应式原理

* 当把一个普通的 JavaScript 对象传入 Vue实例作为 `data` 选项，Vue将遍历此对象所有属性，并使用 `Object.defineProperty` 把这些属性全局转为 `getter/setter`。

```
getter: 是一个获取某个特定属性的值的方法
setter: 是一个设定某个属性的值的方法
```

* 这些getter/setter 对用户来说是不可见的，但是在内部他们让Vue 能够追踪依赖，在属性被访问和修改时通知变更。
* 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据属性记录为依赖。之后当依赖项的 setter触发时，会通知watcher，从而使它关联的组件重新渲染。
* 由于Vue会在初始化实例时对属性执行getter/setter 转化，所以属性必须在 `data` 对象上存在才能让Vue 将它转换为响应式的。

* 注意：Vue无法检测到对象属性的添加和删除，可使用：

```
Vue.set(vm.someObject, 'b', 2);
或 this.$set(this.someObject, 'b', 2);
```
* 提前声明所有响应式属性：
	* 依赖项追踪系统
	* 更好地配合类型检查系统工作
	* 可维护性强，更易理解

#### 1.18. 异步更新队列
* Vue在更新DOM时 是异步执行的。只要侦听到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个watcher被多次触发，只会被推入到队列中一次。然后，在下一个事件循环"tick"中，Vue刷新队列并执行实际(已去重的)工作。

```
Vue.nextTick(callback)
或this.$nextTick(callback)
```

#### 1.19. 组件注册
 * Vue.component
 * 或者 import导入组件，然后在Components选项中引入

#### 1.20. 循环引用
* 递归组件
* 组件之间的循环引用

#### 1.21. 控制更新
* 强制更新 $forceUpdate
* 通过 `v-once` 创建低开销的静态组件
