# 函数

函数是对象，函数可以存放在变量中，作为参数传递。函数可以返回函数。因为函数也是对象，因此函数可以拥有方法。

**ES6函数新增特性**

- **参数默认值**

  在ES5，参数是没有默认值的；想让参数达到默认值的效果，通过||运算符实现这个效果

- **rest参数**

  ...变量

- **扩展运算符**
  rest参数的逆运算或逆运用

- **箭头函数**

  ()=>箭头函数

  **this绑定**
  -- 重点补充箭头函数在用的时候this的指向
  ES5:this指向是函数被调用时的对象所在
  ES6箭头函数：在定义时候对象所在

- **尾调用**

## 定义

ES6 之前的函数定义可以通过函数表达式或函数语句，分别是：

```javascript
var test = function() {}

function test2() {}
```

函数语句在解析时会被提升：不管函数语句放置在哪里，它会被移动到所在作用域的顶部。可使得函数不必先声明后使用。

ES6 新增了箭头函数：

```javascript
let odds = evens.map(v => v + 1)
```

`=>` 之前是函数参数列表。如果只有一个参数可以省略括号。如果没有参数，必须加空的括号。多个参数也必须使用括号。

```javascript
let test1 = v => v + 1
let test1 = () => console.log("hello")
let test1 = (a, b) => a + b
```

箭头后如果只有一个语句可以省略大括号，且如果是语句则语句的值作为函数返回值，即可以省略 `return`。如果有多个语句，必须使用大括号，且返回值一定要显式通过 `return` 返回。

```javascript
let test1 = v => v + 1 // 函数返回 v + 1，不需要 return
let test2 = v => {
    let a = v + 1
    return a
}
```

## 调用

除了声明时定义的形式参数，每个函数还接收两个附加参数：`this` 和 `arguments`。

当实参与形参个数不匹配时不会导致运行时错误。如果实参过多，多出的参数被忽略。如果参数值过少，缺失的值将被替换为 `undefined`。对参数值不会进行类型检查。

## 函数内的 this

函数内可以访问一个隐式的变量 `this`，它的值取决于函数定义和调用的方式。

如果函数不是通过箭头定义的，`this` 的值完全取决于调用方式。有四种：函数式、方法式、构造式、apply。

函数跟方法的区别即直接调用还是通过一个对象调用。当函数被存储为对象的一个属性时，称其为**方法**。当方法被调用时，`this` 绑定到那个对象。

函数调用时，`this` 被绑定到全局对象。

```javascript
let obj = {
    a: 1,
    good: function() { console.log(this && this.a) }
}
obj.good() // 方法，打印 1

let g = obj.good
g() // 函数，打印 undefined
```

构造式即通过 `new` 调用，此时函数是构造器，`this` 指要创建的对象。

```javascript
function Apple {
    this.red = true
}
let apple = new Apple()
```

`apply` 和 `call` 函数的两个特殊方法，可以显式指定 `this` 的值。二者的区别是，`apply` 需要实参放在一个数组中传递，而 `call`的实参直接列出，见例子：

```javascript
let obj = {
    add: function(c, d) { this.a + this.b + this.c + this.d }
}

let add = obj.add
add.apply({a: 1, b: 2}, [3, 4])
add.call({a: 1, b: 2}, 3, 4)
```

ES5 有一个 `bind` 方法可以强制绑定函数的 `this` 到某个对象。此时，函数的调用不在受调用方式的影响。

```javascript
let obj = {
    a: 1,
    b: 2,
    add: function() { this.a + this.b }
}

let add = obj.add
add() // 报错！

add = add.bind(obj)
add() // 正常
```

如果函数通过箭头定义，则 `this` 绑定到函数定义时的上下文。

```javascript
var bob = {
    _name: "Bob",
    _friends: [],
    printFriends() {
        this._friends.forEach(f =>
            console.log(this._name + " knows " + f))
    }
}
```

某些情况下，如果必须访问一个因为某种原因访问不到的 `this`，一般先将这个 `this` 赋给另外一个变量（一般命名为 `that`）。然后传递或通过闭包访问这个变量。

```javascript
myObject.double = function () {
    var that = this
    var helper = function () {
        that.value = add(that.value, that.value)
    }
    helper()
}
```

## 默认参数（ES6）

```javascript
function myfunction(value, option = 'default') {
}

myfunction(1)
myfunction(1, "my")
```

## 剩余参数和展开参数（ES6）

利用 `...`，调用函数时，将一个数组展开为多个参数。反过来，收集剩余参数到一个数组。此特性用于替代 `arguments`。

```javascript
function f(x, ...y) {
    // y is an Array
    return x * y.length
}

f(3, "hello", true) === 6

function f(x, y, z) {
    return x + y + z
}
f(...[1,2,3]) === 6
```

## `arguments`

通过 `arguments` 数组可以访问所有实参，包括那些多余参数。这使得编写一个无需指定参数个数的函数成为可能：

```javascript
var sum = function () {
    var i, sum = 0
    for (i = 0; i < arguments.length; i += 1) {
        sum += arguments[i]
    }
    return sum
};

document.writeln(sum(4, 8, 15, 16, 23, 42)) // 108
```

因为语言的设计错误，`arguments` 并不是一个真正的数组。它只是一个“类似数组”的对象。`arguments` 有 `length` 属性，但缺少所有的数组方法。

## 返回

函数总是会返回一个值。如果没有指定返回值，返回 `undefined`。

## 闭包

函数定义可以嵌套。内部函数可以访问到外围函数的**参数和变量**（`this` 和 `arguments` 除外）。通过函数字面量创建的函数对象包含到外部上下文的连接，这被称为**闭包**。

通过闭包可以实现私有字段、方法和公有字段、方法的分隔。例如下面代码，参数 `status` 是私有的，只能通过 `getStatus` 方法获取：

```javascript
var quo = function (status) {
    return {
        getStatus: function () {
            return status
        }
    }
}
// Make an instance of quo.
var myQuo = quo("amazed")
document.writeln(myQuo.getStatus())
```

这个 `que` 函数被设计成无须在前面加上 `new` 来使用，所以名字也没有首字母大写。当调用 `que` 时，返回一个新对象。该对象包含一个 `getStatus` 方法。即使 `que` 已经返回了，但 `getStatus` 方法仍能访问 `que` 对象的 `status` 参数。`getStatus` 方法并不访问该参数的一个拷贝，它访问的是该参数本身。函数可以访问被创建时所处的上下文环境，这被称为**闭包**。

理解内部函数访问外部函数的变量的实际值而非副本是很重要的，见下面这个错误：

```javascript
var add_the_handlers = function (nodes) {
    var i
    for (i = 0; i < nodes.length; i += 1) {
        nodes[i].onclick = function (e) {
            alert(i)
        }
    }
}
```

原本想每次弹出时显示节点序号，但最后每个节点显示的都是数字都等于节点数组。原因是事件处理函数绑定的是变量 `i`，而非 `i` 当时的值。

正确的修改，利用传递参数绑定当时的值：

```javascript
var add_the_handlers = function (nodes) {
    var i
    for (i = 0; i < nodes.length; i += 1) {
        nodes[i].onclick = function (i) {
            return function (e) {
                alert(i)
            }
        }(i)
    }
}
```

ES6 的 `let` 也可以解决该问题。每个 `i` 定义在自己的作用域中，是独立的。

```javascript
var add_the_handlers = function (nodes) {
    for (let i = 0; i < nodes.length; i += 1) {
        nodes[i].onclick = function (e) {
            alert(i)
        }
    }
}
```

## 尾调用

**作用：**(提升性能)之前做递归，涉及一个函数地址嵌套，相当耗费资源的，有了尾调用在尾调用优化的过程中，提升性能。
在性能优化过程中，觉得某个过程是不断嵌套别的函数或者是你这个函数依赖另一个函数的一个操作，建议可以写成尾调用的形式。

```javascript
{
    function tail(x){
       console.log('tail',x)
   }

   function fx(x){
      //尾调用就是这种情况：
      //return语句就是fx这个函数的最后一步；最后一步就是一个函数tail(x)
       return tail(x)
   }
   fx(123)  //tail 123
}
```

