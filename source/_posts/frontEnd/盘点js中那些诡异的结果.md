---
title: 盘点 js 中那些诡异的结果
date: 2021-01-28 23:57:57
author: Twittytop
categories:
- 前端
tags:
- JavaScript
keywords: JavaScript
---

	本文中涉及到的知识，很多都是比较冷门，在实际编码中你可能用不到，但保不准有些面试官可能会问到，或者你可以拿来zb。

### 	1. typeof null
这个是历史遗留的bug，typeof null 值为"object"

### 	2. null == undefined
值为true

### 	3. -0 === +0
值为true，但是Object.is(-0, +0)为false

### 	4. Infinity * 0
值为NaN，类似的还有Infinity / Infinity = NaN，Infinity % 0 = NaN，Infinity + (-Infinity) = NaN， 

### 	5. ‘a’ < 3
值为false，这是因为字符串和数字作比较时，字符串会转化为数值，而'a'转化后的值为NaN，而NaN < 3的值为false，但你以为这样'a' >= 3就为true了吗？no，'a' >= 3也为false，这是因为有一个规则，即任何关系操作符在涉及比较NaN时都返回false。

### 	6. NaN == NaN
值为false，NaN不是表示某一个具体的数。

### 	7. window.isNaN != Number.isNaN
值为true，window.isNaN在早期判断时存在一个bug，比如window.isNaN('a') = true，后来在es6中修复了这个bug，Number.isNaN('a') = false。

### 	8. typeof Function.prototype
值为"function"，因为Function.prototype是原生方法。

###	        9. Array.isArray(Array.prototype) 
值为true，数组的原型也是数组

### 	10. function fun () {}; fun instanceof Function
值为true，那**fun instanceof Object**呢？值也为true。关于Object和Function的前世今生，后面会再写一篇文章来说明。

### 	11. Math.max('a', 'b')
值为NaN

### 	12. [] == ![]
值为true，这个题目有一次在笔试的时候被考到过。分析一下，这里涉及到隐式类型转换，首先![]=false,变为[] == false，如果有一操作数为布尔值时，将其转化为number，则变为[] == 0，然后将左边也转换为number，左边是对象，先调用valueOf方法，转换的值为[]，不是原始值，所以继续调用toString方法，得到的值为空字符串""，转换为number为0，最终得到0 == 0。

暂时想到这些，后面有补充再更新。如有疑问，欢迎评论区讨论。


