# 1.基础



## 1.1 JavaScript是什么？

JavaScript就是JavaScript。不要强制使用任何规范去写JavaScript，特别是OOP语法。

Javascript中最强大的就是**对象**和**函数**。JavaScript的对象不需要任何类就可以存在。在ES6之前，原形继承可以满足大多数的需求。ES6之后，应学会使用`class`语法来创建类或者继承。

JavaScript中函数是一等公民。

## 1.2 变量定义

这里直接说ES6

### **常量**

`const abc = "123"`

### **变量**

ES6中变量的定义使用`let`，也可以用`var`

二者的区别在于let定义的变量具有块级作用域。块级作用域由块级语句（`if`/`for`/`while`/`try`）或者`{}`组成。

如果代码环境是在ES6中，那么就不要使用`var`，用`let`和`const`。

小技巧：实际写代码的过程中一般先用`const`声明，发现报错后把对应的`const`改成`let`。

**下面的一些例子说明了`var`和`let`的区别**

```javascript
let x = 10
{
    let x = 20
    console.log(x) //20
}
console.log(x) //10
```

这个例子比较傻瓜，就是简单的介绍了`let`的块级作用域

```javascript
function fn() {
    var a = 1 
    var a = 2 //ok
    let b = 1
    let b = 2 //报错
}
```

这个例子说明了`var`允许在一个作用域内被重复定义，但是`let`不允许

```javascript
function fn2() {
    // 在这个函数最开头其实会把下面var b 提升到这里
    //var b
    console.log(a) //undefined
    b = 20
    console.log(a) //20
    var b = 10
}
```

`var`与`let`还有一个区别,`var`允许定义在使用之后，因为`var`的变量在运行时有一个变量提升的效果，上面的代码其实和下面这段代码效果一样

```javascript
function fn3() {
    var b
    console.log(a) //undefined
    b = 20
    console.log(a) //20
    var b = 10
}
```

所以，如果一个js文件中使用`var`定义的变量，比如说用var定义了十几二十个，就都放在函数的最开头，以免发生不必要的错误。`let`定义的变量必须先定义再使用。

## 1.3 JavaScript中的类型

JavaScript是**弱类型**、**动态类型**的，这是JavaScript的两大特点。弱类型的意思是定义变量时不用指定变量的类型。动态类型的意思是，一个变量可以持有不同类型的值。

动态类型的具体表现：

```javascript
let a = 1
a = "abc"
```

狭义讲，JavaScript类型包括：**布尔**、**数字**、**字符串**、`undefined`、**对象**、**函数**。

这里注意`null`、**数组**和**正则表达式**的类型都是`object`。

```javascript
typeof true  // "boolean"
typeof 1  // "number"
typeof "a"  // "string"

typeof (new Date())  // "object"
typeof null  // "object"

typeof undefined  // "undefined"

typeof [] //  "object"
typeof {} //  "object"
typeof /good/  // "object"

typeof function(){}  // "function"
```

布尔值分为`true`和`false`

对象通过引用传递，永远不会被拷贝。

### 字面量

字面量包括**数字字面量**、**字符串字面量**、**对象字面量**、**函数字面量**、**正则表达式字面量**。

## 1.4 数字

JavaScript中只有一种数字类型。内部表示为**64位的浮点数**。没有单独的整数，所以1和1.0是相等的。

`0.1+0.2 `不等于`0.3`，这是JavaScript中经常被报告的一个bug，原因是二进制浮点数不能正确处理十进制小数。**浮点运算中对于整数的运算是精确的**。

如果两个变量都有小数部分，判断他们是否相等最好使用插值法：

```javascript
if（ a-b > 1e-10 ）...
```

### `NaN`和`Infinity`

JavaScript在运算发生上溢、下溢或者除零操作时不会报错。

当运算符结果超过了 JavaScript 所能表示的数字上限（溢出）（`1.79769313486231570e+308`），结果为一个特殊的无穷大的值，即 `Infinity`。同样的，当负数超过能表示的范围，结果为负无穷大，用 `-Infinity` 表示。无穷大的值的加减乘除运算的结果仍为无穷大。

下溢即运算结果无限接近于零比 JavaScript 能表示的最小值还小的时候发生。此时 JavaScript 返回零。

被零整除并不报错，返回 `Infinity` 或 `-Infinity`。

零除以零是没有意义的，结果是`NaN`。无穷大除以无穷大、给任意负数开方运算或算数运算符与不是数字或无法转换为数字的操作数一起使用时都将返回 `NaN`。`typeof NaN === 'number'`。

只能使用`isNaN`函数判断一个值是否是数字。NaN不等于自己。

```javascript
NaN === NaN    // false
NaN !== NaN    // true

isNaN(NaN)       // true
isNaN(0)         // false
isNaN('oops')    // true
isNaN('0')       // false
```

判断一个值是否可用于数字运算的最佳方式是使用 `isFinite()` 函数，因为它会筛选掉 `NaN` 和 `Infinity`。不幸的是，如果操作数不是数字，`isFinite()` 会试图将操作数转换为数字。因此，最好定义函数：

```javascript
function isNumber(value) {
    return typeof value === 'number' && isFinite(value)
}
```

## 1.5 字符串

字符串字面量可以被包围在单引号或双引号中。

JavaScript中没有字符类型（只有字符串）。

字符串有`length`属性。

字符串是不可变的。

可以用`+`连接字符串，实际工作中常用于拼接`path`。

### 模版字符串（ES6）

```javascript
`This is a pretty little template string.`

// 支持多行
`In ES5 this is
 not legal.`

// 支持插值
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// 综合例子：
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

有了模版字符串之后，工作的代码会更加简洁。

## 1.6 对象

对象字面量即通过`{}`定义的对象。

对象字面量中，若属性名是一个合法的JavaScript标识符且不是保留字，可以不使用引号括住属性名。例如：`"first-name"`的引号是必须的，而`firstname`可以不用引号。

增强的对象字面量（ES6）

1. 简写`foo:foo`形式的赋值：

```javascript
var a = 1
var obj = { a: a }
// 等价于
var obj = { a }
```

2. 定义方法：

```javascript
var obj = {
    work: function() {}
}
// 等价于
var obj = {
    work() {}
}
```

3. 调用父类方法（**super**）：

```javascript
var obj = {
    toString() {
        return "d " + super.toString()
    }
}
```

4. 动态的属性名：

```javascript
var fieldName = "aaa"
var obj = {[fieldName]: 1}
// 等价于
var obj = {}
obj[fieldName] = 1
```

###  访问字段

如果字符串是一个合法的JavaScript标识符且非保留字，可以使用`.`运算符访问，否则应该使用中括号。（**调用接口请求数据时经常用到**）

```javascript
tooge["first-name"]
flight.departure.IATA
```

如果检索的属性不存在，返回`undefined`。

尝试从不存在的对象（`undefined`）读取字段会抛出`TypeError`。所以要用`&&`运算符做兼容判断。（**实际开发时如果访问接口数据报错，可能就是因为没有做兼容导致**）

```javascript
// 假如想拿到flight.equipment.model的数据，直接访问可能会报错
flight.equipment.model // throw "TypeError"
// 所以要做如下兼容处理
flight.equipment && flight.equipment.model // undefined
```

## 1.7 `null`和`undefined`

判断一个值是否是`null`，直接使用`===`

```javascript
a == null 
// 上面这句话等于下面这句话
a === null || a === undefined
//所以判断一个值是否是null用下面这句话
if(a === null){
    //...
}
```

`undefined`这个值尽量不要自己使用，因为有时一个值可能是`undefined`。实际开发过程中，请求一个接口的数据，假如返回了一个对象，我们去访问这个对象的一个不存在的属性时，就会返回`undefined`。

要判断余个属性是否存在，值为`null`也算存在，所以用`in`运算符去判断。

```javascript
var a ={ b: 1, c: null }
"b" in a // true
"c" in a // true
"d" in a // false 
```

如果一定要判断某个值是否是`undefined`，用`typeof a ==="undefined"`。

## 1.8 数组

JavaScript中没有真正的数组，而是提供一些类数组的对象。它把数组下标转换成字符串，用其作为属性。它比真正的数组慢。

JavaScript中，数组也是对象的一种形式。用`typeof`运算符报告数组的类型会返回`object`。

### 数组字面量

```javascript
var a = []
var b = ['zero', 1, 2 ]

a[1]  // undefined
b[1]  // 1
b[0]  // "zero"

a.length  // 0
b.length  // 3
```

数组下标以0开始。数组字面量的原型是`Array.prototype`。

就像上面声明的b这个数组一样，数组元素可以是混合类型（因为JavaScript是弱类型语言）。

JavaScript中没有多维数组，但是数组里可以嵌套数组。

```javascript
var c = [[1,2],[3,4]]
c[1][1]  //4
```

### 读写数组元素

下标运算符`[]`将其中的表达式转换为一个字符串，转换使用的是表达式的`toString`方法。产生的字符串将作为属性名。

```javascript
var arr = [ 1, 2 ]
arr[10] // 直接访问arr[10]的结果为undefined
arr[10] = 3
```

上面定义了一个数组`arr`，当我们定义`arr[10] = 3`时，相当于给`arr`这个数组的第11个元素赋值，同时 ，也将数组长度扩大到11。此时`arr[2]`到`arr[9]`都是`undefined`。`arr.length`为11。

直接给数组的`length`赋值相当于将数组设定成指定大小，长度可以比原数组更长或更短。

如果要给数组的最后面追加元素的方法是push()

```javascript
var arr = [1,2]
arr.push('three')
arr // [1,2,'three']
```

### 删除

使用`delete`删除数组元素，但是这个删除会留下”空洞“。`delete`不是从数组中移除一个元素，而是将指定位置上的值置为`undefined`。

```javascript
var a = [1,2,3]
delete a[1] 
a   //[1, undefined, 3]
```

实际开发中，用的比较多的是`splice(start, deleteCount)`。需要传递两个参数，第一个参数指定开始删除的位置，第二个参数指定删除的个数。

```javascript
var a = [1,2,3]
a.splice(0,2)
a  // [3]
```

这里补充一个在日常工作中经常和`splice`共同使用的方法：`indexOf()`

`indexOf()`方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

```javascript
var a = [2,3,5,7,9]
a.indexOf(2) //0
a.indexOf(5) //2
```

`indexOf`可以返回指定元素的**第一个索引**，这里注意，是第一个索引，所以接下来要讲的这个方法，如果数组中有重复元素，就尽量不要使用，或者根据需求使用。

```javascript
var a = [2,3,5,7,9]
//现在要删除7这个元素
a.splice(a.indexOf(7), 1)
console.log(a)  // [2,3,5,9]
```

这个方法可以用来删除数组中的指定的某个值的元素，就比如我取到了一连串不同的`id`，要删除其中指定的某个，就可以用上述方法。