---
title: 06-javascript函数和对象增强
categories:
  - 分类
tags:
  - js高级
indexing: false
keywords: js函数
abbrlink: 2f741396
---

了解一下`js`函数和对象的相关增强知识。


# 1. javascript函数增强
## 1.1 函数基本概念掌握
### 1.1.1 函数对象的属性
现定义一个函数：
```javascript
function foo(a, b){
        
}
```
1. 自定义属性
    ```javascript
    foo.title = "foo title"
    console.log(foo.title) // foo title
    ```
2. 默认属性
    - `name`
    `console.log(foo.name) // foo`
    - `length` (指的是函数参数的个数,但不包括有默认值的参数和可变参数)
   `console.log(foo.length) // 2`
### 1.1.2 函数的`arguments`
1. 三种获取方式
    ```javascript
    function foo1(a, b, c) {
       // 获取arguments
       // 1. arguments是一个伪数组对象，可通过[]字面量继续进行获取
       console.log(arguments[0]);
    
       // 2.for循环遍历
       for (let i = 0; i < arguments.length; i++) {
           console.log(arguments[i]);
       }
    
       // 3. for of 遍历   （for of 可用来遍历可迭代对象）
       for (let item of arguments) {
           console.log(item);
       }
    }
    ```
   
2. 三种转换为数组的方式
    ```javascript
    function foo2(a, b){
       // 转换成数组
       // 1. 将数据放入新数组
       let arr1 = []
       for (let i = 0; i < arguments.length; i++) {
           arr1.push(arguments[i])
       }

       // 2. [].slice()
       let arr2 = [].slice.call(arguments)

       // 3. Array.from()
       let arr3 = Array.from(arguments)
       let arr4 = [...arguments]
       }
    ```
### 1.1.3 箭头函数的`arguments`
箭头函数没有`arguments`，若在箭头函数内部使用，那么`arguments`将会向外寻找父级作用域的`arguments`

### 1.1.4 函数的剩余参数`rest`
如果最后一个参数以...为前缀，那么该参数将以数组的形式接收剩余的参数。
```javascript
function rest(a, ...args){
    console.log(a)
    console.log(args)
}
rest(1,2,3,4,5) // 1 [2,3,4,5]
```

**注：剩余参数必须放在最后一个形参位置，否则会报错。**

剩余参数`rest`和`arguments`的区别：
1. `rest`以数组形式接收参数，`arguments`以伪数组对象形式接收参数；
2. `rest`接收没有对应形参的实参，`arguments`接收所有实参
3. `arguments`是早期`ECMAScript`为方便获取参数定义的一个属性，而`rest`是`ECMAScript6`新增的希望替代arguments的语法，。

## 1.2 纯函数
### 1.2.2 概念理解
> 简单来说，满足有相同输入时总是有相同输出，并且没有任何副作用的函数叫做纯函数。
 
1. 输入相同时有相同输出
   ```javascript
    let b = 1;
    function add1(a){
        return a + b
    }
   ```
   这个函数就不是一个纯函数，因为函数内部存在外部变量b，输出可能不一致。
2. 没有产生任何副作用
   ```javascript
   let a,b = 1;
   function add1(a,b){
       b = 2;
       return a + b
   }
   ```
   这个函数修改了外部变量b，也就是产生了副作用，所以这个函数也不是纯函数。

### 1.2.2 纯函数案例
> let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

- `slice()`
> console.log(arr.slice(0, 5), arr);   //截取数组

输出：[1, 2, 3, 4, 5]   [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

- `splice()`
> console.log(arr.splice(0, 5), arr);

输出：[ 1, 2, 3, 4, 5 ] [ 6, 7, 8, 9, 10 ]

根据输出结果可以知道slice()是纯函数，但splice()不是纯函数，因为splice()修改了arr，产生了外部影响。

**纯函数在函数式编程中很重要，它保证了函数的纯度，只单纯实现自己的业务逻辑，不会对外部变量产生影响。**

## 1.3 函数柯里化
## 1.3.1 基本了解
> `柯里化（Currying）`是把接受多个参数的函数变换成接受一个参数的函数，并且返回接受多个参数的函数的函数。

```javascript
function foo(a, b, c){
    return a + b + c;
}
foo(1, 2, 3)
```
柯里化后：
```javascript
function foo(a){
    return function (b){
        return function (c){
            return a + b + c;
        }
    }
}
foo(1)(2)(3)
```

箭头函数写法：
```javascript
const foo = a => b => c => a + b + c;
```

柯里化优势：
- 职责单一
- 参数复用

## 1.4 with语句和eval函数的使用（了解即可）
1. `with`语句  --  可以访问对象属性
   ```javascript
        const obj = {
            name: 'zf',
            age: 18,
            height: 1.88,
            address: '北京'
        }
        
        with (obj) {
            console.log(name, age, height, address);
        }
   ```
2. `eval`函数  --  可将字符串转换成可执行的js语句
   ```javascript
        const testStr = '全局变量'
        const str = "const msg = 'hello world';console.log(msg);console.log(testStr);"
        eval(str)
   ```
不建议在开发中使用`eval`函数：
- 可读性差
- 字符串容易被篡改，可能造成被攻击的风险；
- eval的执行需要经过javascript解释器，不能优化

## 1.5 严格模式

### 1.5.1 严格模式的理解
长久以来，javascript不断发展，且没有出现兼容性问题，新特性的加入也会兼容旧代码，但是一些错误或者不完善的地方仍然被保留；
在ECMAScript5中，`严格模式`被提出，在严格模式下：
- 严格模式下，一些静默错误会被抛出；
- js解释器会对代码进行更多的优化；
- （禁用一些可能在ECMAScript未来版本中的语法）

**严格模式的开启**
在`script`标签、js文件或函数中使用`use strict`开启严格模式
```javascript
"use strict"

function foo(){
    "use strict"
}
```
注：
- 在`class`、`module`等模块中默认启用严格模式；
- 打包过后的文件默认也是严格模式；

### 1.5.2 严格模式的一些限制
1. 严格模式无法意外创建全局变量
   ```javascript
   "use strict"
   
   function foo(){
       message = 'xuexi javascript'
   }
   
   console.log(message) // 报错：message is not defined
   ```
2. 静默操作抛出异常
   ```javascript
   "use strict"
   const obj = {
       name: 'zf',
   }
   
   Object.defineProperty(obj, "name",{
       writable:false,
       configurable:false
   })
   
   obj.name='xuexi'  //  Cannot assign to read only property 'name' of object '#<Object>'
   
   delete obj.name   //  Cannot delete property 'name' of #<Object>
   ```

3. 函数形参名称不能相同
   ```javascript
   function foo1(num, num) {
     return num + num
   }
   console.log(foo1(1, 1));  // Duplicate parameter name not allowed in this context
   ```
4. 严格模式不允许函数参数有相同的名称
5. 不允许0的八进制语法
6. 在严格模式下，不允许使用with
7. 在严格模式下，eva不再为上层引用变量
8. 严格模式下，this绑定不会默认转为对象

# 2. javascript对象增强
## 2.1 属性描述符
| 属性描述符   | configurable | enumerable | writable | value | get | set |
|---------|--------------|------------|----------|-------|-----|-----|
| 数据属性描述符 | 可以           | 可以         | 可以       | 可以    | -   | -   |
| 存取属性描述符 | 可以           | 可以         | -        | -     | 可以  | 可以  |

1. 数据属性描述符 （默认都为true）
   - `[configurable]`  -- 是否可以删除属性，是否可以修改属性，或者是否可以修改为存取属性描述符;
   - `[enumerable]`  -- 是否可以枚举，即是否可以通过for..in 或者Object.keys()获取到属性;
   - `[writable]` -- 是否可以修改数据;
   - `[value]` -- 是否可以返回属性值;
   
   ```javascript
   const obj = {
    name:'xuexi',
   }

   Object.defineProperty(obj, "name", {
        configurable: false,
        writable: false,
        enumerable: true,
        value: 'xuexi'
   })
   ```
2. 存取属性描述符
   - `[configurable]`  -- 是否可以删除属性，是否可以修改属性，或者是否可以修改为存取属性描述符;
   - `[enumerable]`  -- 是否可以枚举，即是否可以通过for..in 或者Object.keys()获取到属性;
   - `[get]` -- 获取属性值,默认为undefined;
   - `[set]` -- 设置属性值,默认为undefined;

   ```javascript
   Object.defineProperty(obj, "name", {
     configurable: false,
     writable: false,
     get: function () {
       console.log('getter');
     },
     set: function () {
       console.log('setter');
     }
   })
   ```

**同时定义多个属性**
```javascript
Object.defineProperties(obj, {
    name: {
        configurable: false,
        writable: false,
        get: function () {
            console.log('getter');},
        set: function () {
            console.log('setter');
        }
    }})
```

**其他属性**(了解即可)
1. 获取对象属性的属性描述符
   - `Object.getOwnPropertyDescriptor(obj, 'name')`  -- 获取对象的单个属性的属性描述符
         ```javascript
         console.log(Object.getOwnPropertyDescriptor(obj, 'name'));
         // 输出：{
         //    value: 'xuexi',
         //            writable: true,
         //         enumerable: true,
         //         configurable: true
         // }
         ```
   - `Object.getOwnPropertyDescriptors(obj, 'name')`  -- 获取对象全部属性的属性描述符
2. 阻止对象的扩展
   ```javascript
   // 阻止扩展
   Object.preventExtensions(obj)
   obj.address = 'beijing'
   console.log(obj);
   ```
3. 密封对象 -- 禁止删除操作
   ```javascript
   // 密封对象
   Object.seal(obj)
   delete obj.name
   console.log(obj);
   ```
4. 冻结对象 -- 禁止修改操作
   ```javascript
   // 冻结对象
   Object.freeze(obj)
   obj.name = 'renzhenxuexi'
   console.log(obj);
   ```






















