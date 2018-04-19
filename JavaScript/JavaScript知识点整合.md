# JavaScript知识点整理

## 1.js的基本类型有哪些？引用类型有哪些？null和undefined的区别？

js的基本类型有：null、undefined、boolean、Number、String

js的引用类型有：Object、Array、Function

**undefined**：（1）变量被声明了，但没有赋值时，就等于undefined。

​		      （2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。

​                      （3）对象没有赋值的属性，该属性的值为undefined。

​                      （4）函数没有返回值时，默认返回undefined。

**null**：（1） 作为函数的参数，表示该函数的参数不是对象。

​           （2） 作为对象原型链的终点。

```javascript
typeof null  // "object"

typeof undefined  // "undefined"
```

判断一个值是否是 `null`，直接使用 `===`

```javascript
my_value === null
```

`undefined` 这个值尽量不要自己使用。但仍有时候值可能是 `undefined`。比如访问一个不存在的属性，返回 `undefined`。没有返回值的方法，赋值给一个变量，得到 `undefined`。

要判断一个属性是否存在，值为 `null` 也算存在，可以用 `in` 运算符：

```javascript
var a = { b: 1, d: null }
"b" in a // true
"c" in a // false
"d" in a // true
```

如果一定要判断值是否为 `undefined`，可以检测 `typeof v === "undefined"`。

## 2.如何判断一个变量是Array类型？如何判断一个变量是Number类型？

### 判断一个变量是Array类型

```javascript
var arr = [1,2,3]
console.log(arr instanceof Array)  //true
console.log(arr.constructor === Array)  //true
console.log(Array.isArray(arr))  //true
```

### 判断一个变量是Number类型

```javascript
var a = 1
console.log(isNaN(a))  //false
```

## 3.Object是引用类型嘛？引用类型和基本类型有什么区别？哪个是存在堆哪一个是存在栈上面的？

Object是引用类型

1. **基本类型**：Undefined、Null、Boolean、Number 和 String，这5种基本数据类型可以**直接访问**，他们是按照值进行分配的，存放在**栈(stack)内存**中的简单数据段，数据大小确定，内存空间大小可以分配。 
2. **引用类型**：即存放在**堆(heap)内存**中的对象，变量实际保存的是一个指针，这个指针指向另一个位置。 

```javascript
var obj1 = {name:'bangbang',age:18}
var b = obj1
var c = obj1.age

console.log(b.name) //bangbang
console.log(c)      //18
//改变b和c的值
b.name = 'yanniu'
c = 22
console.log(obj1.name)     //yanniu
console.log(obj1.age)       //181234567891011
```

  以上看出：当我们改变b的数据的时候，我们看到了obj1.name的数据也在改变,但是我们改变c的数据的时候发现，obj1.age的值没有变化，这说明了：b和obj1变量操作的是同一个对象，c和obj1完全独立的。

## 4.JS常见的dom操作api

常用的原生JS

## 节点查找API（最常见）

- `document.getElementById` ：根据ID查找元素，大小写敏感，如果有多个结果，只返回第一个
- `document.getElementsByClassName` ：根据类名查找元素，多个类名用空格分隔，返回一个 HTMLCollection 。注意兼容性为IE9+（含）。另外，不仅仅是document，其它元素也支持 getElementsByClassName 方法
- `document.getElementsByTagName` ：根据标签查找元素， * 表示查询所有标签，返回一个 HTMLCollection 
- `document.getElementsByName` ：根据元素的name属性查找，返回一个 NodeList 
- `document.querySelector` ：返回单个Node，IE8+(含），如果匹配到多个结果，只返回第一个
- `document.querySelectorAll` ：返回一个 NodeList ，IE8+(含）
- `document.forms` ：获取当前页面所有form，返回一个 HTMLCollection 

## HTMLCollection

HTMLCollection是一个特殊的NodeList，表示包含了若干元素（元素顺序为文档流中的顺序）的通用集合，它是实时更新的，当其所包含的元素发生改变时，它会自动更新。另外，它是一个伪数组，如果想像数组一样操作它们需要像 Array.prototype.slice.call(nodeList, 2) 这样调用。



## 判断一个变量是数组类型的方式

```javascript
var arr = [1, 2, 3]

console.log(arr instanceof Array)  //true

console.log(typeof arr)  //object typeof不能判断数组

Array.isArray(arr)  //true
```

## 原型链继承

```javascript
function Element(id) {

   this.elem = document.getElementById(id)

}

Element.prototype.html = function (val) {

    var elem = this.elem

    if (val) {

        elem.innerHTML = val

        return this //return this可以链式操作

   } else {

        return elem.innerHTML

    }

}

Element.prototype.on = function (type, fn) {

    var elem = this.elem

    elem.addEventListener(type, fn)

   console.log(this, 666)

    return this //return this可以链式操作

}

var div1 = new Element('main-outer')

div1.html('<p>test</p>').on('click', function () {

    alert('click')

}).html('<p>javascript</p>')
```

## 构造函数

大写字母开头的函数就是构造函数，构造函数类似于模版可以创建多个对象

```javascript
function Foo(name, age) {

    this.name = name

    this.age = age

    this.class = '1-1'

    //return this 构造函数默认有return this

}

//new就是构造函数形成一个实例的过程

var f1 = new Foo('xs', 20)

//new一个构造函数返回一个对象的过程

// 1.先new的时候把参数传进去

// 2.new这个函数执行的时候，函数里边的this会先变成一个空对象，再this.name这些属性赋值

// 赋值完把this return回来，赋值给f，f就具备了f.name = 'zhangsan'
```

