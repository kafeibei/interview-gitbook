## JAVASCRIPT

### 1. 概念
#### 1.1. 介绍js基本数据类型。
* Undefined/ Null/ Boolean/ Number/ String/ Object (6个)
* 新增的Symbol

#### 1.2. 介绍js有哪些内置对象？
* 数据封装类：Object/ Array/ Boolean/ Number/ String
* 其他对象：Function/ Date/ Arguments/ Math/ RegExp/ Error

#### 1.3. JavaScript有几种类型的值？，你能画一下他们的内存图吗？

* 栈：原始数据类型(Undefined/ Null/ String/ Number/ Boolean)
* 堆：引用数据类型(Object/ Array/ Function)

* 两种类型的区别是：存储位置不同
	* 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据
	*  引用数据类型存储在堆(heap)中的对象，占据控件大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取到地址后从堆中获得实体

<a id="prototype">&nbsp;</a>
#### 1.4. Javascript原型，原型链，有什么特点。 <em style="color:red;">*</em>
* 每个对象都会在其内部初始化一个属性，就是propotype(原型)，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去prototype里找这个属性，这个propotype又会有自己的prototype。于是一直找下去，一直检索到Object内建对象，即原型链的概念。
* 特点：JavaScript对象是通过引用来传递的，我们创建的每一个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

#### 1.5. Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？
* hasOwnProperty
* javascript中hasOwnProperty函数方法返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性，该属性必须是对象本身的一个成员
* 使用方法
	* object.hasOwnProperty(proName)
	* 其中参数object是必选项。一个对象的实例。
 	* proName是必选项。一个属性名称的字符串值。
* 如果 object 具有指定名称的属性，那么JavaScript中hasOwnProperty函数方法返回 true，反之则返回 false。

<a id="inherit">&nbsp;</a>
#### 1.6. JavaScript继承的几种实现方式？
* 原型链继承
* 构造继承
* 实例继承
* 拷贝继承
* 组合继承
* 寄生组合继承

```
function Animal (name) {
	this.name = name || 'Animal'
	this.sleep = function () {
		// ...
	}
	this.features = []
}
function Cat(name) {

 }
Cat.prototype = new Animal();
var tom = new Cat('Tom');
var kissy = new Cat('Kissy');

tom.name	// Animal
kissy.name	// Animal
```

<a id="create">&nbsp;</a>
#### 1.7. javascript创建对象的几种方式？
* 工厂方式
* 构造函数方式
* 原型模式
* 混合构造函数，原型方式，常用
* 动态原型方式

#### 1.8. new操作符具体干了什么呢? <em style="color:red;">*</em>
* 新生成了一个对象
* 链接到原型
* 绑定this
* 返回新对象

eg.

```
function Foo() {
  return this;
}
Foo.getName = function () {
  console.log('1');
};
Foo.prototype.getName = function () {
  console.log('2');
};

new Foo.getName();   // -> 1
new Foo().getName(); // -> 2
```

<a id="this">&nbsp;</a>
#### 1.9. 谈谈This对象的理解。 <em style="color:red;">*</em>
* this 总是指向函数的直接调用者(而非间接调用者)
* 如果有new 关键字，this指向new出来的那个对象
* 在事件中，this指向触发这个事件的对象

eg.

```
function foo() {
	console.log(this.a)
}
var a = 1
foo()

var obj = {
	a: 2,
	foo: foo
}
obj.foo()

// 以上两者情况 `this` 只依赖于调用函数前的对象，优先级是第二个情况大于第一个情况

// 以下情况是优先级最高的，`this` 只会绑定在 `c` 上，不会被任何方式修改 `this` 指向
var c = new foo()
c.a = 3
console.log(c.a)

// 还有种就是利用 call，apply，bind 改变 this，这个优先级仅次于 new

执行结果：
1
2
3
```
* this一旦绑定了上下文，就不会被任何代码改变。

<a id="window">&nbsp;</a>
#### 1.10. 什么是window对象，什么是document对象？
* window对象是指浏览器打开的窗口
* document对象是html文档对象的一个只读引用，window对象的一个属性
* js的3个组成部分：
	* 核心(ECMAScript) - js的语法和基本对象
	* 文档对象模型(DOM) - 处理网页的内容
	* 浏览器对象模型(BOM) - 与浏览器交互

<a id="event">&nbsp;</a>
#### 1.11. 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？ <em style="color:red;">*</em>
* 事件：可以被JavaScript侦测到的行为，网页中的每个元素都可以产生某些可以触发JavaScript函数的事件
* 时间触发的三个阶段
	* `window`往事件触发处传播，遇到注册的捕获时间会触发
	* 传播到事件触发处时触发注册事件
	* 从事件触发处往`window`传播，遇到注册的冒泡事件会触发
* 事件传播
	* 事件捕获：鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击子元素，如果父元素通过事件捕获方式注册了对应的事件，会先触发父元素的绑定事件
	* 事件冒泡：事件冒泡是由内到外进行事件传播，直到根节点
* dom标准事件流的触发顺序：先捕获再冒泡
* `stopPropagation` 阻止冒泡

```
// IE11、Chrome、Firefox、Safari等浏览器支持
addEventListener("click", function () {
	console.log('----')
}, useCapture)

* useCapture 是否采用事件捕获，默认为false，即采用事件冒泡

// IE10及以下浏览器
attachEvent("click", function () {
	console.log('----')
})
```
* 事件代理：如果一个节点的子节点是动态生成的，那么子节点需要注册时间的话应该注册在父节点上。优点：
	* 节省内存
	* 不需要给子节点注销事件

<a id="function">&nbsp;</a>
#### 1.12. Javascript作用链域? <em style="color:red;">*</em>
* 全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节。
* 当需要从局部函数查找某一属性或方法时，如果当前作用域没有找到，就会上溯到上层作用域查找，直至全局函数，这种组织形式就是作用域链

<a id="closure">&nbsp;</a>
#### 1.13. 什么是闭包（closure），为什么要用它？<em style="color:red;">*</em>
* 闭包指有权访问另一个函数作用域中变量的函数
* 创建闭包最常见的方式是在一个函数内创建另一个函数，同一个另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部
* 闭包的特性
	* 函数内再嵌套函数
	* 内部函数可以引用外层参数和变量
	* 参数和变量不会被垃圾回收机制回收

```
function aa() {
	var number = 10;
	var bb = function () {
		alert(number)
	}
	number ++;
	return bb
}
var tt = aa()
tt() // 11
```
例如：for循环里面用settimeout
* setTimeout是从队列结束的时候开始计时的，如果前面有进程没有结束，那么它就等到它结束再开始计时。在for循环里面，任务队列就是自己所在的循环。循环结束setTimeout才开始计时，所以setTimeout里面的i都是最后一次循环。

```
function countDown () {
	for (let i = 10; i > 0; i--) {
		let a = function (v) {
			console.log('v', v)
		}
		setTimeout(a(i), 1000);
	}
}
# 给回调函数传一个实参缓存循环数据，即形参v缓存了i
```

#### 1.14. JavaScript中的作用域与变量声明提升？ <em style="color:red;">*</em>
* 变量作用域是指 这个变量存在的上下文，它指定了可以访问哪些变量以及你是否有权限访问某个变量
* 局部作用域：javascript没有块级作用域，但是它有函数级别的作用域。即在一个函数内定义的变量只能在函数内部访问或者这个函数内部的函数访问，闭包除外

> * 如果声明没有var关键字，那么这个变量将是一个全局变量
> * 局部变量优先级大于全局变量

* 全局变量：所有函数外面声明的变量都处于全局作用域中，在浏览器中，这个全局作用域就是window对象。这个变量可以在任何地方被使用

> setTimeout中函数在全局作用域中执行，即函数中this关键字指向全局对象(Window)

* 变量提升：所有变量的声明都会提升到函数的开头(如果这个变量在这个函数里面)或者全局作用域的开头(如果这个变量是一个全局变量)

> 函数声明会覆盖变量声明

eg.

```
b()
console.log(a)

var a = 'Hello world'

function b() {
	console.log('call b')
}
执行结果：
call b
undefined
```

<a id="es6">&nbsp;</a>
#### 1.15. 谈一谈你对ECMAScript6的了解？
* JS包含三个部分：ECMAScript(核心)、DOM(文档对象模型)、BOM(浏览器对象模型)
* ES6的新特性
	* 箭头函数 =>
	* 类的支持--class关键词
	* 字符串模板 `
	* let与const关键词
	* 增强的对象字面量
	* 参数默认值
	* for of值遍历
	* 模块
	* Math, Number, String, Object的新API
	* Promise
	* Generator	// 异步编程的方法
	* Proxy	// 用来自定义对象中的操作

```
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next());
console.log(b.next());
console.log(b.next());
执行结果：
{ value: 2, done: false }
{ value: 3, done: false }
{ value: undefined, done: true }
```

#### 1.16. ECMAScript6 怎么写class么，为什么会出现class这种东西?
* JS本身是面向对象的，ES6提供的类是对JS原型模式的包装。对象的创建，继承更加直观，并且父类方法的调用，实例化，静态方法和构造函数等概念更加形象化

```
// 类的定义
class Animal {
	// ES6中新型构造器
	constructor () {
		this.name = name
	}
	// 实例方法
	sayName () {
		console.log('My Name is' + this.name)
	}
}
// 类的继承
class Programmer extends Animal {
	constructor (name) {
		// 直接调用父类构造器进行初始化
		super (name);
	}
	program () {
		console.og('I am coding...')
	}
}
// 测试类
var animal = new Animal ('dummy'),
	wayou = new Programmer ('wayou');
animal.sayName() // My Name is dummy
wayou.sayName() // My Name is wayou
wayou.program()	// I am coding...
```

<a id="ajax"></a>
#### 1.17. Ajax是什么？如果创建一个Ajax? *
* 异步传输+js+xml
* 异步：向服务器发送请求的时候，不必等待结果，而是可以同时做其他事情，等到有了结果它自己会根据设定进行后续操作，与此同时，页面不会发生整页刷新，提高了用户体验
	* 创建XMLHttpRequest对象，也就是创建一个异步调用对象
	* 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息
	* 设置响应HTTP请求状态变化的函数
	* 发送HTTP请求
	* 获取异步调用返回的数据
	* 使用JavaScript和DOM实现局部刷新

```
const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
```

#### 1.18. Ajax解决浏览器缓存问题？
* 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")
* 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")
* 在URL后面加上一个随机数："fresh=" + Math.random()
* 在URL后面加上一个时间戳："nowtiem=" + new Date().getTime()
* 如果使用jquery，直接设置 ajaxSetting({cache: false})

<a id="json"></a>
#### 1.19. JSON的了解？
* JSON是一种轻量级数据交换格式
* 它是基于JavaScript的一个子集。数据格式简单，易于读写，占用带宽小
* 如: {"age": "12", "name": "back"}
* JSON字符串转换为JSON对象：

	```
	str.parseJSON()
	或 JSON.parse(str)

	```
* JSON对象转换为JSON字符串：

	```
	obj.toJSONString()
	或 JSON.stringify(obj)
	```

<a id="promise"></a>
#### 1.20. promise
* 概念：promise对象用于表示一个异步操作的最终状态(完成或者失败)，以及其返回值
* 本质：一个promise是某个函数返回的对象，可以把回调函数绑定在这个对象上，而不是把回调函数当做参数传进函数
* 可以看成一个状态机。初始是`pending`状态，可以通过函数`resolve`和`reject`，将状态转变为`resolved`或者`rejected`状态，状态一旦改变不能再次变化
* 链式调用
	* 连续执行两个或者多个异步操作，每个后来的操作都在前面的操作执行成功之后，带着上一步操作所返回的结果开始执行。可以通过创造一个promise chain来完成这种需求
	* ES7添加了语法糖 - async/await

```
const promise = new Promise((resolve, reject) => {
	console.log(1)
	resolve()
	console.log(2)
})
promise.then(() => {
	console.log(3)
}).catch((err) => {
	console.log('catch:', err)
})
console.log(4)
// 执行结果
1
2
4
3
```
* Promise构造函数是同步执行的，promise.then中的函数是异步执行的。
* 构造函数中的 resolve 或 reject 只有第一次执行有效，多次调用没有任何作用。promise 状态一旦改变则不能再变。
* `.catch`是`.then`第二个参数的简便写法。`.then`的第二个处理错误的函数捕获不了第一个处理成功的函数跑出的错误，后续的`.catch`可以捕获之前的错误。

<a id="async">&nbsp;</a>
#### 1.21 async和await
* 一个函数如果加上 `async`，那么该函数就会返回一个 `Promise`，可以把`async`看成将函数返回值使用`Promise.resolve`包裹了下。
* `await` 只能在`async`函数中使用

```
function sleep() {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log('finish')
      resolve("sleep");
    }, 2000);
  });
}
async function test() {
  let value = await sleep();
  console.log("object");
}
test()
执行结果：
finish
object
# `await`会等待`sleep`函数`resolve`
```
* 优点：处理`then`的调用链，能够更清晰明确的写出代码
* 确定：滥用`await` 可能会导致性能问题，因为`await`会阻塞代码，也许之后的异步代码并不依赖前者，但仍然需要等待前者完成，导致代码失去了并发性

#### 1.22 事件循环
* JavaScript是单线程的，同一个时间只能做一件事。JS的主要用途是与用户互动，以及操作DOM。
* 单线程意味着，所有任务都需要排队，前一个任务结束了，才会执行后一个任务。
* 所以任务分成两种：
	* 同步任务：主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务。
	* 异步任务：不进入主线程、而进入“任务队列”的任务，只有“任务队列”通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。
* 只要主线程空了，就会去读取“任务队列”，这是JS的运行机制。这个过程不断重复。所以整个机制称为“事件循环”

```
1) 所有同步任务都在主线程执行，形成一个执行栈。
2) 主线程之外，还存在一个“任务队列”。只要异步任务有了运行结果，就在“任务队列”之中放置一个事件。
3) 一旦“执行栈” 中所有的同步任务执行完毕，系统就会读取“任务队列”，看看里面哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
4) 主线程不断重复上面的第三步。
```
例如：
```
setTimeout(function(){console.log(1);}, 0);
console.log(2);
// 执行结果：
2
1
```
* setTimeout(fn, 0);的含义是，指定某个任务在主线程最早可得的空闲事件执行，它在“任务队列”的尾部添加一个事件，因此要等到同步任务和“任务队列”现有的事件都处理完，才会得到执行

<a id="tasks"></a>
#### 1.23 macrotasks(宏任务tasks)和microtasks(微任务)的区别。
* 宏任务macrotasks：为了让浏览器能够从内部获取JS/ dom内容并确保执行栈能够顺序进行。例如：解析HTML，获得鼠标点击事件回调等。
```
script
setTimeout
setInterval
setImmediate
I/O
UI rendering
```
* 微任务microtasks：通常用于在当前正在执行的脚本之后直接发生的事件，比如对一系列的行为做出反应，或者做出一些异步的任务，而不需要新建一个全新的tasks。
```
process.nextTick
Promises
Object.observe
MutationObserver
```
* 每一次事件循环中，macrotask只会提取一个执行，而microtask会一直提取，直到microtasks队列清空
* 事件循环每次只会入栈一个macrotask，主线程执行完该任务后，又会先检查microtasks队列并完成里面的所有任务后再执行macrotask

DEMO：

```
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0);
new Promise(resolve => {
	console.log('Promise')
	resolve()
}).then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});
console.log('script end');
执行结果：
"script start"
"Promise"
"script end"
"promise1"
"promise2"
"setTimeout"
# Promise的立即返回的异步任务会优先于setTimeout延时为0的任务执行
```

### 2. 细节
#### 2.1. null, undefined的区别
* null 表示一个对象是"没有值"的值，也就是值为"空"
* undefined 表示一个变量声明了没有初始化(赋值)
* undefined不是一个有效的JSON，而null是
* undefined的类型(typeof)是undefined
  * 是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是没有定义。当尝试读取时返回undefined
	* 例如变量被声明了，但没有赋值时，就等于undefined
* null的类型(typeof)是object
	* 是一个对象(空对象，没有任何属性和方法)
	* 例如作为函数的参数，表示该函数的函数不是对象

#### 2.2. js延迟加载的方式有哪些？
* defer和async
* 动态创建dom方式
* 按需异步载入js

#### 2.3. 如何解决跨域问题？
* 浏览器出于安全考虑，有同源策略。即如果协议、域名或者端口有一个不同就是跨域，Ajax请求会失败。
* 常用解决方法：
	* jsonp：利用`<script>`标签没有跨域限制的漏洞。通过`<script>`标签指向一个需要访问的地址并提供一个回调函数来接收数据。

	```
	<script src="http://domain/api?param1=a&param2=b&callback=jsonp"></script>
	<script>
	    function jsonp(data) {
	    	console.log(data)
		}
	</script>
	# 使用简单且兼容性不错，只限于`get`请求
	```

	* CORS：需要浏览器和后端同时支持。浏览器自动进行CORS通信，只要后端实现了CORS，就实现了跨域
		* 服务端设置`Access-Control-Allow-Origin` 就可以开启CORS。该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。
	* documen.domain：只能用于二级域名相同的情况下
		* 只要给页面添加 `document.domain='test.com'`，表示二级域名都相同就可以实现跨域
	* window.postMessage：通常用于获取嵌入页面中第三方页面数据。一个页面发送消息，另一个页面判断来源并接收消息

		```
		// 发送消息端
		window.parent.postMessage('message', 'http://test.com')
		// 接收消息端
		var mc = new MessageChannel()
		mc.addEventListener('message', event => {
		  var origin = event.origin || event.originalEvent.origin
		  if (origin === 'http://test.com') {
		    console.log('验证通过')
		  }
		})
		```

#### 2.4. 模块化开发怎么做
* 模块化是指js文件按照功能分离，根据需求引入不同的文件。
* 可通过自执行函数，不暴露私有成员

```
var module1 = (function () {
	var _count = 0
	var m1 = function() {
		// ...
	}
	var m2 = function () {
		// ...
	}
	return {
		m1: m1,
		m2: m2
	}
})()
```

#### 2,5. DOM操作——怎样添加、移除、移动、复制、创建和查找节点?
* 创建新节点
	* createElement // 创建具体的元素
* 添加、移除、替换、插入
	* appendChild()
	* removeChild()
	* replaceChild()
	* insertBefore() // 在已有子节点前插入一个新的子节点
* 查找
	* getElementsByTagName()	// 通过标签名称
	* getElementsByName()		// 通过元素的name属性
	* getElementById()			// 通过元素id，唯一性

#### 2.6. call、apply, bind的区别？
* 相同：都是为了改变函数体内部this的指向
* 不同：接收参数的方式不同
	* call 需要把参数按顺序传递进去, add.call(sub, 5, 3)
	* apply 需要把参数放在数组中, add.apply(sub, [5, 3])
	* bind	返回一个新函数，供以后调用

```
	function add (a, b) {
		alert(a+b)
	}
	function sub (a, b) {
		alert(a-b)
	}
	add.call(sub, 3, 1)
```

* 不需要关系具体多少参数被传入函数，选用apply();
* 确定函数可接收多少个参数，并想一目了然表达形参和实参的对应关系，用call();
* 想要将来再调用方法，不需立即得到函数返回结果，则使用bind();

#### 2.7. 数组和对象有哪些原生方法，列举一下？
* 数组 - Array

```
 - 赋值方法：直接修改数组自身
	* pop/ push
	* shift/ unshift
	* splice
	* reverse
	* sort
- 访问方法：返回相应结果，不修改数组本身
	* concat
	* json
	* slice
	* toString
	* indexOf/ lastIndexOf
- 迭代方法：
	* forEach
	* map
	* filter
	* every/ some
	* reduce/ reduceRight
```
* 对象 - Object
```
 * Object.assign()
 * Object.keys()
 * hasOwnProperty()
 * isPropotypeOf()
```
* 数字 - Number

```
* toFixed()
* toPrecision()
* isNaN()
```
* 字符串 - String

```
* charAt(index)	// 返回指定位置的字符
* charCodeAt(index)	// 返回指定字符的ASCII码
* concat()
* indexOf()
* lastIndexOf()
* match()
* replace()
* slice(start, end)		// 可为负数
* split()
* substr(start, length)
* substring(start, end)		// 非负整数
```
* Date对象
```
* getDate()
```
