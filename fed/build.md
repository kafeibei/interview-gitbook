## 构建工具

<a id="gulp"></a>
### 1. Gulp
#### 1.1. gulp概述
* 原理：利用node.js流的威力，可以快速构建项目并减少频繁的IO操作
* api：
	* gulp.src - 匹配文件
	* gulp.task - 定义任务
	* gulp.watch - 监视文件
	* gulp.dest - 目标文件
	* gulp.pipe - 分发
* 插件：
	* uglify/ autoprefixed/ sass/ imagemin/ htmlmin/ concat/ minifycss/ browser-sync
* 网址：https://www.gulpjs.com.cn/

<a id="webpack"></a>
### 2. Webpack
#### 2.1. webpack概述
* 原理：基于Javascript应用程序的静态模块打包器，将需要的模块打包成一个或者多个bundle
* api：
	* 入口 entry
	* 输出 output
	* loader - 处理非Javascript文件(webpack自身只理解JavaScript)。loader可以将所有类型的文件转换为webpack能够处理的模块，例如从不同语音(如TypeScript)转换为JavaScript，或将内联图像转换为data URL。然后利用webpack打包能力对它们进行处理 - css-loader/ style-loader/ ts-loader/ raw-loader
	* 插件 plugins - 解决loader无法实现的其他事
	* 模块热替换
* 网址：https://www.webpackjs.com/
