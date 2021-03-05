---
title: JavaScript中神奇的写法
date: 2021-03-03 22:53:15
author: Twittytop
categories:
- 前端
tags:
- JavaScript
---

### 写在前面

### 提前声明

本文只是给你提供一个视角，让你觉得‘噢，原来还可以这么写’，但实际业务中一般不会用到。



#### 1. 连续调用call，到底是为谁打call

```javascript
function f1 () {
    console.log('f1');
}
function f2 () {
    console.log('f2');
}
console.log(f1.call.call.call(f2)) // f2
```

因为call本来也是一个函数，所以它也有call方法。前面不论调用多少个call，最终返回的都是call方法本身，最后传入f2参数之后相当于执行f2.call();



#### 2. try中return，finally中还会执行吗

```javascript
function foo () {
  try {
      console.log(1);
      return 'try';
      console.log(2);
  } catch (err) {

  } finally {
      console.log(3);
      return 'finally';
      console.log(4);
  }
}
console.log(foo());

// 1
// 3
// finally
```

在我们通常的理解中，return之后相当于这个函数就退出了，后面的代码都不会执行。但是这里我们可以看到finally最后确实是执行了, 而且里面的return覆盖了try中的return。这背后的执行机制与 Completion Record有关，关于Completion Record，这里就不展开讲了，有兴趣的可以去了解一下。

#### 3.  连续new

```javascript
class Person {
  constructor (n){
    console.log("Person", n); // Person 1
    return class {
      constructor (n) {
        console.log("returned", n); // returned undefined
      }
    }
  }
}
new new Person(1);
```

我们可以将 **new new Person(1)** 拆分成如下执行过程：

```javascript
let p = new Person(1);
new p; // 这里没有参数的时候括号是可以省略的
```

这里的p 相当于

```javascript
class {
    constructor (n) {
      console.log("returned", n);
  }
}
```

现在应该理解了吧。



### 写在后面

如果觉得对你有点帮助的话，请为我打call。另外还有哪些让你惊呼还可以这么写的用法，欢迎补充。



参考：  winter老师《重学前端》