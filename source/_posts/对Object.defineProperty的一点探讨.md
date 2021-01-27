---
title: 对Object.defineProperty的一点探讨
date: 2021-01-15 15:43:04
categories:
- 前端
tags:
- JavaScript
---

### 引子

最近读高程4的时候，发现里面有一句话“一个属性被定义为不可配置之后，就不能再变回可配置的了。再次调用Object.defineProperty()并修改任何非writable属性会导致错误”，亲自实践之后，发现这句话的后半段说得有点问题。



1. ##### 当writable为true时，修改value属性不会导致错误

```javascript
let person = {};
Object.defineProperty(person, "name", {
 configurable: false,
 writable: true,
 value: "Nicholas"
});

Object.defineProperty(person, "name", {
 value: "Twittytop"
});
```

2. ##### 当writable为true时将它修改为false不会导致错误（enumerable 属性不行）

```JavaScript
let person = {};
Object.defineProperty(person, "name", {
 configurable: false,
 writable: true
});

Object.defineProperty(person, "name", {
 writable: false
});
```

3. ##### 当writable为false时将它修改为true也会导致错误

   ```javascript
   let person = {};
   Object.defineProperty(person, "name", {
    configurable: false,
    value: "Nicholas"
   });
   
   Object.defineProperty(person, "name", {
    writable: true
   });
   ```

   

### 关于writable

当writable为false时，value值如果是一个数组或对象，修改里面的值将不会报错

```javascript
let person = {};
Object.defineProperty(person, "friend", {
 configurable: false,
 writable: false,
 value: ["Twittytop", "Nicholas"]
});
person.friend.push("Matt");
```

或者

```javascript
let person = {};
Object.defineProperty(person, "friend", {
 configurable: false,
 writable: false,
 value: {
  name: "Twittytop"
 }
});
person.friend.name = "Nicholas";
```



### get 和 Object.defineProperty

当时用get关键字时，它和通过Object.defineProperty定义的get属性有一些细微的差别。

在class中，当使用get关键字时，属性将被定义在实例的原型上，而使用Object.defineProperty时，属性将被定义在实例上。

```javascript
class Person {
 get name () {
  return 'Twittytop';
 }
}
var p = new Person();
console.log(p.hasOwnProperty('name')); // false
console.log(Object.getPrototypeOf(p).hasOwnProperty('name')); // true

Object.defineProperty(p, 'job', {
 value: 'engineer'
});
console.log(p.hasOwnProperty('job')); // true
```







