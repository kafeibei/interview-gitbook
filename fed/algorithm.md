## 算法

#### 1.1. ['1', '2', '3'].map(parseInt)
* 解析：
	* parseInt() 函数能解析一个字符串，并返回一个整数，需要两个参数 (val, radix)
	* 其中 radix 表示要解析的数字的基数。【该值介于 2 ~ 36 之间，并且字符串中的数字不能大于radix才能正确返回数字结果值】;
		* parseInt('1', 0)	-> 1
		* parseInt('2', 1)	-> NaN
		* parseInt('3', 2)	-> NaN
* 概述：
	* Map 生成一个新数组，遍历原数组，将每个元素拿出来做一些交换，然后append到新的数组中
	* FlatMap 作用和map一致，对于多维数组来说，会将原数组降维
	* Reduce 数组中的值组合起来，最终得到一个值

#### 1.2 为什么0.1+0.2!=0.3？
* JS双精度问题
* 解决方法：

```
parseFloat((0.1 + 0.2).toFixed(10))
```
