## CSS

<a id="css3"></a>
### 1. CSS3

#### 1.1. CSS3新属性
* 边框：
	* border-radius
	* box-shadow: [水平 垂直 模糊 阴影大小 颜色 内部还是外部阴影]
	* border-image
* 背景
	* background-image
	* background-size
	* background-origin
	* background-clip
* 渐变
	* 线性渐变：linear-gradient
	* 径向渐变：radial-gradient
* 文本
	* text-shadow
	* box-shadow
	* text-overflow
	* word-wrap
	* word-break
* 字体
	* @font-face
* 2D转换
	* transform
	* transform-origin
	* translate
	* rotate
	* scale
	* skew
	* matrix
* 3D转换
	* transform
	* transform-origin：改变被转换元素的位置
	* transform-style：被嵌套元素如何在 3D 空间中显示
	* perspective：透视视图
	* perspective-origin：底部位置
	* backface-visibility：定义元素在不面对屏幕时是否可见
* 过渡
	* transition：[属性 过渡时长 过渡曲线 延迟时间]
	* animation：[动画名称 动画时长 过渡曲线 延迟时间]
		* keyframes
* 多列
	* column
* 用户界面
	* resize
	* box-sizing
	* outline-offset
* 图片
* 按钮
* 框大小
* 弹性盒子
	* flex
	* flex-direction: row | row-reverse | column | column-reverse
	* justify-content: flex-start | flex-end | center | space-between | space-around
	* align-items: flex-start | flex-end | center | baseline | stretch
	* flex-wrap: nowrap| wrap| wrap-reverse
	* align-content:  flex-start | flex-end | center | space-between | space-around | stretch
	* order: 1
	* align-self: auto | flex-start | flex-end | center | baseline | stretch
	* flex: none | [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
* 多媒体查询
	* @media

#### 1.2. 媒体查询的原理是什么？
* 不同条件下使用不同的样式，使页面在不同的终端设备下达到不同的渲染效果
* @media
	* screen max-width/ min-width

<a id="other"></a>
### 2. OTHER

#### 2.1. CSS中的长度单位
* px
* pt
* rem
* em
* ex
* vw
* vh
* vmin
* vmax

#### 2.2. CSS选择器的优先级是怎样的？
* 不同级别
	* 浏览器默认属性
	* 继承
	* 通配符
	* 标签选择器 - 1
	* 类选择器 - 10
	* ID选择器 - 100
	* 行内样式 - 1000
	* important
* 注意：
	* important的优先级最高
	* 优先级相同时，就近原则，选择最后出现的样式
	* 继承得来的属性，优先级最低
