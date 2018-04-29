# JavaScript设计模式

《javascript设计模式》Pro+JavaScript+Design++Patterns

### Part 1  面向对象的JavaScript

# 第一章 富有表现力的JavaScript

## JavaScript的灵活性

​	JavaScript这门语言是十分灵活的，在日常写代码的时候，我们可以使用函数式编程的方式去写，也可以通过面向对象的方式去写。

​	JavaScript允许我们去效仿一些其他语言的设计模式和代码习惯，它也提供了服务端所有的面向对象语言的特点。

#### Examples

```javascript
function startAnimation(){
    ...
}
function stopAnimation(){
	...
}
```

**Eg1.这是编写函数最简单的方式，但是这种方式不允许我们去创建一个可以保存状态或者可拓展方法的对象。**

```javascript
var Anim = function() {
    ...
}
Anim.prototype.start = function() {
        
}
Anim.prototype.stop = function() {
        
}

/* Usage */
var myAnim = new Anim()
myAnim.start()
myAnim.stop()
```

**Eg2.这种方式创建了一个`Anim`的类，然后在它的原型链上添加了两个方法（第三章有详细讲解）**

```javascript
var Anim = function() {
    ...
}
Anim.prototype = {
    start: function() {
        ...
    },
    stop: function() {
         ...
    }
}
```

**Eg3.这是第二种方式的改写,这种写法更加优雅，更符合传统的面向对象编程，和第二种的调用方式是相同的，如果喜欢在一个声明里写完所有方法，推荐使用这种。**

------

​       接下来我们试着增加函数的扩展性，上面的`start`和`stop`方法，都是我们去声明的，接下来我们试着构建一个函数，通过构建的这个函数来创造我们之前写的`start`和`stop`方法。

```javascript
Function.prototype.method = function(name,fn) {
    this.prototype[name] = fn
}

var Anim = function() {
    ...
}
Anim.method('start', function() {
	...
})
Anim.method('stop', function() {
	...
})
```

​	我们通过在`Function`的原型链上增加一个`method`方法，这个方法中传递两个参数，第一个是函数名，第二个是函数，来进行函数的扩展。这里的`this`指向调用构造函数的对象。

```javascript
Function.prototype.method = function(name,fn) {
    this.prototype[name] = fn
    return this
}

var Anim = function() {
    ...
}
Anim.method('start', function() {
	...
}).method('stop', function() {
	...
})
```

这是上一种方法的改进版，在原型链的`method`的最后`return this`，让函数可以链式调用。

上面介绍了五种方法，每一种方法都有不同的效率和表现。在日常编程中，我们可以根据项目来找到最适合的方法。

## 弱类型的JavaScript

​	JavaScript语言最大的两个特点弱类型、动态类型。JavaScript中的变量是不需要声明类型就可以存在的，但这不代表它就没有类型，它的类型取决于它所包含的内容，一个变量可以根据它所被赋予的值改变类型。

​	JavaScript中`toString`方法可以将一个数字或者布尔值转化成字符串。`parseFloat`和`parseInt`方法可以将字符串转换成数字，如果想判断一个值的结果是`true`还是`false`，可以打印`!!num`，常配合if使用。

## 函数作为一等公民的JavaScript

在JavaScript中，函数是作为一等公民存在的。

函数可以作为变量存放，也可以被当作参数传递到另一个函数中，还可以被当作一个返回值`return`，JavaScript中的函数在运行时才会被构建。

### 匿名函数

自执行函数

```javascript
(function() {
    var foo = 10
    var bar = 2
    alert(foo*bar)
})()
```

上面这种函数不需要声明就能立即执行。

```javascript
(function(foo,bar) {
    alert(foo*bar)
})(10,2)
```

带参数的自执行函数，和第一种结果一样。

```javascript
(function(foo,bar) {
    return foo * bar
})(10,2)
```

有返回值的自执行函数。

### 闭包

​	JavaScript语言最大的特点之一就是闭包了，闭包是一个受保护的变量命名空间，创建一个闭包的方式是使用内嵌函数。在ES6之前，JavaScript是只有函数作用域，没有块级作用域的。**JavaScript中函数是运行在它们被定义的作用域内，而不是他们被执行时所在的作用域。**

```javascript
var baz
(function(){
    var foo = 10
    var bar = 2
    baz = function() {
        return foo * bar
    }
})()
baz()
```

​	上面的`baz()`执行后可以接收到`foo`和`bar`。因为`baz`这个函数时被定义在闭包内，即使闭包已经完成执行了，它仍然能够取到这两个变量的值。关于闭包，会详细的在第三章的封装中解释。

## 动态类型的JavaScript（易变的对象）

​	在JavaScript中，`everything is an object`，**一切都是对象**。并且，所有对象都是易变的，这两点意味着一些其他语言中不能使用的技术可以在JavaScript中实现，比如说**给函数添加属性**。

```javascript
function displayError(message) {
    displayError.numTimesExecuted++
    alert(message)
}
displayError.numTimesExecuted = 0
```

这也意味着你可以在类被定义之后修改它

```javascript
function Person(name,age){
    this.name = name
    this.age = age
}
Person.prototype = {
    getName: function(){
        return this.name
    },
    getAge: function() {
        return this.age
    }
}
```

实例化类

```javascript
var alice = new Person('Alice', 93)
var bill = new Person('Bill', 30)
```

修改类

```javascript
Person.prototype.getGreeting = function() {
    return 'Hi' + this.getName() + '!'
}
```

实例调用修改后的类新增的方法

```javascript
alice.displayGreeting = function() {
    alert(this.getGreeting())
}
```

​	JavaScript原型链使得JavaScript这门语言非常灵活，你可以实例化一个类，然后动态的去执行方法，添加方法。

​	如果之前你学的是C++和Java，你会觉得JavaScript是一门很奇怪的语言，因为在C++和Java中，对象一旦实例化，是不能扩展方法的，类被声明之后也是不能修改的。

​	在JavaScript中，任何东西都可以在运行时被修改，这是JavaScript语言强大的地方，当然它也有副作用，易变性导致了在类中声明的方法很难保持原封不动

## JavaScript中的继承

​	JavaScript中的继承并不像其他面向对象语言那样直接，在ES6之前，JavaScript的继承使用了原型链，这是一种效仿类继承的继承，第四章会详细介绍。

## JavaScript设计模式

为什么要在JavaScript中使用设计模式，主要有以下三点：

#### 1.可维护性

​	设计模式可以使你写的模块更加宽松与耦合，它可以让你更加方便的重构你的代码，同时能让你在大型团队中和其他程序员更加好的合作。

#### 2.更好的交流性

​	设计模式中有很多词语对应很多种模式，例如工厂模式、观察者模式。这样在一个团队内交流时我们就可以不用对一段代码有过长的解释，直接说这段代码使用了什么模式。

#### 3.高性能

​	使用设计模式可以彻底的提升代码的运行速度以及优化上传到客户端的代码数量，第13章享元模式与14章代理模式是高性能的最好体现。

为什么不在JavaScript中使用设计模式

#### 1.复杂性

​	可维护性往往是十分昂贵的，你的代码会更加复杂，对于新手程序员来说不是很友好。

#### 2.性能

​	虽然一些设计模式增加了性能，但是他们并没有提升太大的性能。根据业务需求，这种在性能上的间接的花费会从不明显的变到完全无法接受。在日常业务中由于时间的关系往往顾不上性能。

### 总结

​	在日常开发中针对不同需求选择不同的设计模式，并且要保持一个高性能。

