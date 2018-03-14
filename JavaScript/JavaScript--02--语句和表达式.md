# 2.语句和表达式

语句与表达式的区别是，表达式会产生一个值，语句不会。

`var a = 1 + 1`是语句，`1 + 1`是表达式。

控制语句：`if` `switch` `while` `for` `do` `break` `return` `continue` `throw` `try` `catch`。

一个 `try` 只能有一个 `catch`，捕获所有（各种）异常。

## 假值

下列值被当作假：

- false
- null
- undefined
- 空字符串
- 数字0
- 数字NaN

其他值都被当作真，包括`true`、字符串`"false"`、空数组 `[]`，空对象 `{}`。

## 相等

这里有两组运算符 `===`、`!==` 和 `==`、`!=`。

`===` 只有在两个操作数类型相同且值相同时才会返回 true。但 `==` 和 `!=` 在做比较时，如果操作数类型不同，会尝试做转换，转换规则非常奇怪。一些例子：

```javascript
'' == '0'          // false
0 == ''            // true
0 == '0'           // true

false == 'false'   // false
false == '0'       // true

false == undefined // false
false == null      // false
null == undefined  // true

' \t\r\n ' == 0    // true
```

建议，始终使用 `===`、`!==`。永远不要使用 `==`、`!=`。

只有一种情况用`==`，`jquery`源码中就是这么用的

```javascript
var  obj = {};

//看一个对象的属性存不存在的时候用==

if(obj.a == null){}

//在一个函数中看a的参数是否存在的时候用==

function (a,b){
   if(a == null){}
 }

 //这两种情况的共同点：
 //要保证这个==前面的不管是a还是是obj.a，它是定义的

```

其他情况一律使用`===`。

## 强制类型转换

由于上面说`==`时说到了`==`会对数据进行类型转换，这里就将所有强制类型转换的情况例举出来：

#### 1.字符串拼接的时候会有强制类型转换

```javascript
var a = 100 +10  //110
var b =100 +'10'  //'10010'
```

#### 2.==

```javascript
100=='100'  //true

''==0  //true

null == undefined  //true
```

#### 3.===

```javascript
100==='100'  //false

''===0  //false

null === undefined  //false
```

#### 4.if的强制类型转换

```javascript
if(100){console.log(110)} // 110

if(''){console.log(110)} // undefined
```

#### 5.逻辑运算

```javascript
10 && 0  // 0

'' || 'abc'  // 'abc'
```

#### 6.用一个简单的方式知道它在if里是true还是false

```javascript
var a = 100
!!a  //true

var b = ''
!!b  //false

//  1.在常见的变量中那些会在if里边当false用

if(0){}  //false

if(NaN){}  //(nots a number)false

if(''){} //false

if(null){}  //false

if(false){}  //false

if(undefined){}  //false

2.在if中当true用：

!!100   //true

 !!'abc'   //true

```

#### 7.内置函数

```javascript
Object

//function object(){native code}

Array

//function Array(){native code}

Function

//function Function(){native code}

```

#### 8.内置对象

```javascript
Math

JSON

```

#### 9.== 和 ===

```javascript
var  obj = {}

//看一个对象的属性存不存在的时候用==

if(obj.a == null){}

//在一个函数中看a的参数是否存在的时候用==

function (a, b){
   if(a == null){}
 }
```

## 赋值

赋值运算符的结合性是从右至左，也就是说，如果一个表达式中出现了多个赋值运算符，运算顺序是从右到左。因此，可以通过这种方式来给多个变量赋值：`i=j=k=1`

## 解构赋值(ES6)

解构（Destructuring）能将数组或对象匹配到变量。解构是错误容忍的：若找不到赋值 `undefined`。

```javascript
// 数组
var [a,  ,b] = [1, 2, 3]
a === 1
b === 3

// 对象
var { op: a, lhs: { op: b }, rhs: c } = getASTNode()

// 对象，缩写
var {op, lhs, rhs} = getASTNode()

// 可以用于形参处
function g({name: x}) {
    console.log(x)
}
g({name: 5})

// 错误容忍
var [a] = []
a === undefined

// 错误容忍+默认值
var [a = 1] = []
a === 1

// 错误容忍+默认参数
function r({x, y, w = 10, h = 10}) {
    return x + y + w + h;
}
r({x:1, y:2}) === 23
```

解构赋值是ES6语法的新特性，使用解构赋值可以让代码的可读性更强，而且看起来更舒服，但是解构赋值还是要在实际项目中边写边感受。

## switch

switch 结构如下：

```javascript
switch(value) {
case 1:
    doSome()
    break
case 2:
    doSome()
    break
default
    doSome()
}

```

case 比较使用 `===` 运算符，即严格比较。

JavaScript 的 switch 匹配一个 case 后，会继续检测后续的 case，除非显式使用 `break`。

一个 switch 是一个语句块，case 不形成新的语句块或作用域，因此不要 case 中使用 `let` 定义变量。例如:

```javascript
switch(value) {
case 1:
    let a = 1
    doSome()
    break
case 2:
    let a = 1 // 报错！a 已声明！
    doSome()
    break
default
    doSome()
}

```

## 循环

基本 for 循环：

```javascript
for(initializtion; condition; increment)
```

for-in 枚举对象的所有属性名（或键名）。用于数组，枚举所有的 index。

通常需要检查 `object.hasOwnProperty(varibale)` 来确定这个属性名就是该对象的成员，还是从其原型链中得到的。

```javascript
for (myvar in obj) {
    if (obj.hasownProperty(myvar)) {
        ...
    }
}
```

for-of 可以遍历可迭代的对象，最常见的例子是数组。

```javascript
let a = [1, 2, 3]
for(let item of a) console.log(item) // 1 2 3
```

## 三元运算符与布尔段路

JavaScript 常利用布尔短路和动态类型的特性，编写快捷代码，如：

```javascript
window.onload = window.onload || funciton() {...}

var a = obj && obj.field || ""
```

如果 `obj` 存在则尝试访问它的一个字段。否则，或者字段本身为空，返回空字符串。

三元运算符即 `?:`，与 JavaScript 代码中常见的快捷写法 `a && a.b || c` 并不一定等同。比如：

```javascript
var a = obj && obj.field || -1
```

如果 `obj` 存在，但 `obj.field` 为 0，结果将是 `-1`。因为 0 也是假值。但用三元运算符就没有这个问题。

```javascript
var a = obj ? obj.field : -1
```