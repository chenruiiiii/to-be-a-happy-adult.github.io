---
title: 08-手写apply/call/bind
categories:
  - js高级
tags:
  - apply/call/bind
abbrlink: 5be1151a
date: 2025-07-16 20:44:55
---

手撕apply/call/bind


- apply()
```javascript
Function.prototype.myApply = function (context) {
  // 判断调用对象是否是函数
  if (typeof this !== 'function') {
      throw new TypeError('type error');
  }

  // 判断this要绑定的对象是否为空
  context = context || window

  context.fn = this
  let result = null, args = [...arguments].slice(1)
  context.fn(...args)

  delete context.fn
  return result
}

// 测试

```

- call()
```javascript
Function.prototype.myCall = function (context) {
  // 判断调用对象是否是函数
  if (typeof this !== 'function') {
    throw new TypeError('type error');
  }

  // 判断this要绑定的对象是否为空
  context = context || window

  // context.fn = this   
  // 如果不想fn()被展示出来就使用Object.defineProperty()
    Object.defineProperty(context, 'fn', {
        value: this,
        writable: true,
        enumerable: false,
        configurable: true
    })
    
  let result = null, args = [...arguments].slice(1)

  //  此处fn的调用方式与apply()不同，这个更适合call()风格
  if (arguments[1]) {
    context.fn(...args)
  } else {
    context.fn()
  }

  delete context.fn
  return result
}
```

- bind()
```javascript
Function.prototype.myBind = function(context) {
    // 判断调用对象是否为函数
    if (typeof this !== "function") {
        throw new TypeError("Error");
    }
    // 获取参数
    var args = [...arguments].slice(1),
        fn = this;
    return function Fn() {
        // 根据调用方式，传入不同绑定值
        return fn.apply(
            this instanceof Fn ? this : context,
            args.concat(...arguments)
        );
    };
};
```












