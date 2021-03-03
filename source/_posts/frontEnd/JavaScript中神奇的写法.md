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

这些可能实际业务中根本不会用到，但是可以扩充你的视野和帮你理解一些知识点。



连续调用call



try中return，finally中还会执行吗

```javascript
function foo(){
  try{
    return 0;
  } catch(err) {

  } finally {
    return 1;
  }
}

console.log(foo());
```



连续new

```javascript
class Cls{
  constructor(n){
    console.log("cls", n);
    return class {
      constructor(n) {
        console.log("returned", n);
      }
    }
  }
}

new (new Cls(1));
```

