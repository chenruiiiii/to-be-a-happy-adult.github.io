# 数据类型的检测

❗️首先要明确常见的**八种**数据类型：

1. 基本数据类型
   - `string`
   - `number`
   - `boolean`
   - `null`
   - `undefined`
   - `bigint`
   - `symbol`
2. 复杂数据类型
   - `object`

## 1. **typeof**

- **typeof** 是一元运算符，用法：`typeof value`
- 输出值：上面所说的八种类型（除null之外加上个function）

**注意点**：

1. `typeof null` 输出值为Object，这个是历史遗留问题;
2. `typeof function(){}`输出值为`function`；
3. `[]`、`{}`、`function(){}`输出值都为`Object`，也就是`typeof`只能精准判断除了null之外的基本类型，无法判断复杂数据类型(除function外)；
   
   | 类型                           | 输出值      | 原因                   |
   |:----------------------------:|:--------:|:--------------------:|
   | **null**                     | object   | 历史遗留问题               |
   | 其他原始类型                       | 对应类型字符串  | 准确识别                 |
   | **function(){}**             | function | 函数属于function类型       |
   | **[] / {} / Date / RegExp等** | object   | -（无法具体区分object的具体类型） |

## 2. **instanceof**

- **instanceof** 是二元运算符，用法：`object instanceof type`
- 输出值：true 或 false

**注意点**：

1. `instanceof `是在检测对象的隐式原型链的任意位置是否存在构造函数的原型;
2. 能检测对**Object**的具体类型，如`Function`、`Array`、`Object`等；



## 3. **constructor**

- **constructor** 是对象的构造函数的显式原型上的属性，用法：`object.constructor`
- 输出值：对象所属的构造函数

**注意点**：

1. `constructor` 是属性，不是运算符，用法：`object.constructor`
2. `constructor` 可以进行修改

```javascript
let obj = []

console.log(obj.constructor) // Array (在obj的隐式原型链上寻找构造函数的原型，再从上面获取constructor属性)

obj.constructor = Function(){}

console.log(obj.constructor) // Function
```

## 4. **Object.prototype.toString.call()**

能准确判断数据类型，包括ES6+出的数据类型(如：`Map`、`Set`、`WeakSet`、`WeakMap`等)



🚌总结：

| 方法                             | 核心原理              | 优点                 | 缺点                                      |
|:------------------------------:|:-----------------:|:------------------:|:---------------------------------------:|
| typeof                         | 内部类型标志位           | 推测快速简单             | 无法检测出[] / {}以及对象的具体类型（如Date、RegExp第二个）  |
| instanceof                     | 检测显示原型是否在对象隐式原型链上 | 检测对象是否为构造函数实例，支持继承 | 无法检测原始类型                                |
| constructor                    | 显式原型的构造函数属性       | 检测是否是某个构造函数的实例     | 原始类型检测不准确；nul/undefined检测会出错；而且**易被修改** |
| Object.prototype.toString.call | 利用Object的固有属性     | 准确检测类型（包括ES6+新类型）  | 写法麻烦                                    |
