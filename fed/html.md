## HTML

<a id="html5"></a>
### 1. HTML5

#### 1.1. HTML5新特性，语义化
* 语义化：更清晰，更直观的理解语音所要表达的含义；除了当事开发者能快速熟悉代码外，让机器更方便的读懂代码
* 语义化的标签
	* header/ footer/ section/ article/ aside/ time等
	* 提升可访问性/ seo更友好/ 结构清晰，利于维护
* input类型添加
	* color/ date/ datetime/ email/ month/ number/ range/ search/ tel/ time/ url/ week
* 表单属性加强
	* autocomplete/ autofocus/ novalidate/ max/ min/ multiple/ pattern/ required/ placeholder
* canvas
* 新多媒体元素
	* audio/ video/ source/ embed/ track
* 拖放
	* drag/ drop
* 地理定位
	* geolocation
* web存储
	* localStorage/ sessionStorage
* web SQL
	* openDatabase/ transaction/ executeSql
* 应用程序缓存
	* manifest
* websocket
	* 双向通信，浏览器和服务器只需要做一个握手动作，浏览器和服务器就形成了一条快速通道。两者之间可以数据互相传送
* Webworker：为JavaScript创造多线程环境，运行在后台的JavaScript，独立于其他脚本，不会影响页面的性能。


#### 1.2. Application Cache配置文件
* 概念：h5新特性，允许浏览器在本地存储页面所需要的资源，使得页面离线也可以访问
* 实现：
	* 在服务器上建立一个.appcache类型的文件，即缓存清单，里面的内容确定了哪些文件需要缓存，哪些文件不需要缓存，如果资源无法访问会使用什么页面等
	* 在html文档中引入manifest文件
* 原理：
	* 第一次访问时，浏览器加载完HTML文档后，会查看其是否有引入manifest文件。若引入，则加载manifest文件，然后根据manifest的文件内容进行资源的缓存，并缓存当前文档
	* 之后访问，浏览器首页会查看manifest文件是否被修改(无论内容还是注释)，如果被修改，将当做第一次访问，重新根据manifest文件内容进行缓存
	* 如果应用缓存存在，且manifest没有被修改，浏览器直接从缓存中加载文档和资源，不会访问网络(无论联网与否，都不会访问网络)

#### 1.3. canvas
* 定义图形，比如图标和其他图像
* 只是图形容器，必须使用脚本来绘制图形

### 2. OTHER

#### 2.1. xhtml和html的区别
* xhtml是符合xml标准的改进型HTML，本质上没什么区别，更加规范
* 区别：
	* XHTML 元素必须被正确的嵌套
	* XHTML 元素必须被关闭
	* 标签名必须用小写字母
	* XHTML 文档必须拥有根元素

#### 2.2. 使用data-的好处
* 自定义数据属性
* data-*实际上是data-前缀加上自定义的属性名，使用这样的结构可以进行数据存放
* 读写方式：
	* 直接在HTML元素标签上写
	* 通过js对其操作/ dataset属性
* 如果属性名称中还包含连字符(-)，需要转换成驼峰命名方式
* 可以通过css添加一些样式

<a id="meta"></a>
#### 2.3. meta标签
* 提供有关页面的原信息，比如针对搜索引擎和更新频度的描述和关键词。
* 位于文档的头部，不包含任何内容。定义了与文档相关联的名称/ 值对
* name属性：文档关键词

```
<meta name="keywords" content="卡卡杯" />
<meta name="description" content="卡卡杯" />
<meta name="viewport" content="width=device-width">
```
* http-equiv属性：指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME 文档头部包含名称/值对
	* 过期时间：

	```
	<meta http-equiv="expires" content="31 Dec 2008">
	```
	* 重定向：

	```
	<meta http-equiv="Refresh" content="5;url=http://www.w3school.com.cn" />
	<meta http-equiv="x-dns-prefetch-control" content="on">
	```

#### 2.4. meta viewport原理
* 手机浏览器页面的虚拟"窗口"
* 让网页开发者控制viewport的大小和缩放

```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
```
* 如果不显示的设置viewport，那么width的默认为980
* 如果页面的所有元素宽度都小于980，此时width为980，如果页面最宽的位置超过980，那么width等于最大宽度。总之，默认能将整个页面从左到右显示出来

#### 2.5. HTML废弃的标签
* b/ u/ i/ s ：单纯的改变字体样式，没有任何语义
* strong/ ins/ em/ del：有语义，定义什么样的文字。是删除的，强调的，重要的，还是插入的

#### 2.6. SVG
* 可缩放矢量图

#### 2.7. audio在ios下无法自动播放
* wx.ready之后播放
* 不支持动态创建audio标签自动播放
