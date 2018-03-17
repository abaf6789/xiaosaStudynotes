# 对象

## 对象和属性概述

普通的 JavaScript 对象是“键值对”的无序集合。

属性包括一个名和一个值。属性名是字符串。除了名和值，每个属性还有一些关联的值，我们称之为属性的特性（attributes）：

- `writable` 特性规定属性值是否可以被设置
- `enumerable` 特性规定 for/in 循环是否包含该属性
- `configurable` 特性规定属性是否可以被删除，以及它的特性是否可以被修改。

ES5 之前，你的代码创建的属性都是可写、可枚举，可配置的。ES5 允许你配置属性的特性。

每个对象都有三个关联的对象特性（attribute）：

- 对象的 *prototype*，到另一个对象的引用，从该对象继承属性。
- 对象的 *class* 是一个字符串，分类对象的类型。
- 对象的 *extensible* 标志控制新属性是否可以加到对象。

## 原型

每个对象都有一个原型对象，并且它可以从中继承属性。

通过对象字面量创建的所有对象的原型对象都是 `Object.prototype`。通过 new 创建的对象的原型是构造器的 `prototype` 属性。如 `new Array()` 的原型是 `Array.prototype`。

`Object.prototype` 是少有的没有原型的对象。所有内建的构造器的原型继承自 `Object.prototype`。如 `Date.prototype` 继承自 `Object.prototype`。

## 原型继承

假设要查询对象 `o` 的 `x` 属性。若对象 `o` 自己没有该属性。则从对象的原型中找。若原型也没有，则从原型的原型中找。直到找到 `x`，或所有原型都被检查。

若给对象 `o` 的属性 `x` 赋值，若对象 `o` 有属性 `x`（不是继承的），则直接设置该属性。否则，赋值在对象 `o` 上创建一个新属性 `x`。如果对象 `o` 继承了过 `x` 属性，继承的属性被新创建的自有属性隐藏。

属性赋值检查原型链来决定赋值是否被允许。如果 `o` 继承了只读属性 `x`，则赋值不被允许。若允许，赋值总是在对象 `o` 自身创建或修改属性，不会碰原型链。

一个例外情况。若 `o` 继承的属性 `x` 是一个访问器属性，定义了 setter 方法。则调用 setter 方法，而不是在 `o` 上创建新属性 `x`。但 setter 方法是在对象 `o` 上调用的，不是在定义它的原型上调用的。因此如果 setter 方法定义了属性，它们定义在对象 `o` 上，原型链不会被修改。

原型关系是一种动态的关系。如果我们添加一个新的属性到原型中。该属性会立即对所有基于该原型创建的对象可见。

## Object.create()

静态方法 `Object.create()` 用于创建一个新对象，使用第一个参数作为对象原型。第二个参数可选，描述新对象的属性。

```javascript
var o1 = Object.create({x:1, y:2}) // o1 inherits properties x and y.
```

传入 `null` 创建的对象没有原型，此时对象什么也不继承，包括 `toString()` 等方法（因此不能使用 `+` 运算符）：

```javascript
var o2 = Object.create(null) // o2 inherits no props or methods.
```

创建空对象，即等价于 `{}` 的写法是：

```javascript
var o3 = Object.create(Object.prototype)
```

## 属性访问错误

查询不存在的属性不会报错，返回 `undefined`。

`null` 和 `undefined` 没有属性。因此对其查询属性会报错 `TypeError`。尝试给 `null` 或 `undefined` 设置属性抛`TypeError`。

尝试设置只读属性，或者向不允许添加新属性的对象添加属性，会失败。但这些失败尝试不会抛任何错误。在 ES5 严格模式下，会抛 `TypeError` 异常。

属性是否可以赋值规则比较复杂，以下情况会失败：

- `p` 是 `o` 本身的属性，但是只读的，即不能设置只读属性。
- `p` 是 `o` 继承来的属性，但是只读的：不能用自有属性隐藏一个继承来的只读属性。
- `p` 不是 `o` 的属性，`p` 也不是继承来的带有 setter 的属性，但 `o` 的 `extensible` 特性是 false。

## delete

`delete` 运算符从对象中删除属性。它的运算数是一个属性访问表达式。

```javascript
delete book.author
delete book["main title"]
```

`delete` 只删除自己的属性，不删除继承的。

删除成功，删除操作返回 true。返回 false 表示删除无效，例如不存在该属性。当表达式不是一个属性访问表达式时，`delete` 也返回 true。

`delete` 不会删除不可配置的属性（`configurable` 为false）。但它会删除一个不可扩展的对象的可配置的属性。内建对象的部分属性是不可配置的。通过变量声明和函数声明创建的全局对象的属性也是不可配置的。严格模式下，尝试删除一个不可配置的属性会导致`TypeError`。在非严格模式下（ES3），`delete` 操作只是返回 false。

```javascript
delete Object.prototype // Can't delete property is non-configurable
var x = 1 // Declare a global variable
delete this.x // Can't delete this property
function f() {} // Declare a global function
delete this.f // Can't delete this property either
```

在非严格模式下删除可配置属性，你可以省略全局对象引用。`delete` 后面直接使用属性名：

```javascript
this.x = 1 // Create a configurable global property (no var)
delete x // And delete it
```

但在严格模式下，如果属性前不加限定，会抛出 `SyntaxError`。

删除属性可能让原型链中的属性浮现出来。

## 测试属性

用 `in` 运算符测试对象是否有某个属性（自有或继承都可以）。若有返回 true：

```javascript
var o = { x: 1 }
"x" in o // true
"y" in o // false
"toString" in o // true 继承的
```

用 `hasOwnProperty()` 方法测试对象是否有自有属性。

```javascript
var o = { x: 1 }
o.hasOwnProperty("x") // true: o has an own property x
o.hasOwnProperty("y") // false: o doesn't have a property y
o.hasOwnProperty("toString") // false: toString is an inherited property
```

`propertyIsEnumerable()` 仅当属性是对象的自有属性且属性的 `enumerable` 特性为 true 时才返回 true。部分内建的属性是不可枚举的。

```javascript
var o = inherit({ y: 2 })
o.x = 1
o.propertyIsEnumerable("x") // true: o has an own enumerable property x
o.propertyIsEnumerable("y") // false: y is inherited, not own
Object.prototype.propertyIsEnumerable("toString") // false: not enumerable
```

## 枚举/遍历属性

遍历属性，一般通过 **for/in** 循环，此外，ES5 提供两种额外的方式。

**for/in** 循环遍历自有和继承的属性。循环变量是属性名。对象继承的内建方法是不可枚举的。

```javascript
var o = {x:1, y:2, z:3}
o.propertyIsEnumerable("toString") // => false: not enumerable
for(p in o)
	console.log(p)
```

一些工具库向 `Object.prototype` 添加了新方法或属性。在 ES5 前，没有办法令这些属性不可枚举，因此它们会出现在 **for/in** 中。一些常见的过滤：

```javascript
for(p in o) {
	if (!o.hasOwnProperty(p)) continue // Skip inherited properties
}
for(p in o) {
	if (typeof o[p] === "function") continue // Skip methods
}
```

ES5 定义了两个函数，用于枚举属性名。

- `Object.keys()`，枚举对象的可枚举的自有属性，返回值是一个数组。
- `Object.getOwnPropertyNames()` 返回自有属性，包括不可枚举的属性。

## 属性 Getters 和 Setters

ES5 开始，除了普通的数据属性，还可以通过 getters 和 setters 定义的属性有时被称作访问器属性。

访问器属性不像数据属性有 writable 特性。若属性有 getter 和 setter 两个方法，则它是一个读写属性。如果属性只有 getter 方法，它是一个只读属性。若只有 setter 方法，它是一个只可写的属性；此时读取它直接返回 `undefined`。

访问器属性的函数名就是属性名。

例子：

```javascript
var p = {
    x: 1.0,
    y: 1.0,
    get r() { return Math.sqrt(this.x*this.x + this.y*this.y) },
    set r(newvalue) {
        var oldvalue = Math.sqrt(this.x*this.x + this.y*this.y)
        var ratio = newvalue/oldvalue
        this.x *= ratio
        this.y *= ratio
    },
	// theta 是只读的
	get theta() { return Math.atan2(this.y, this.x) }
}

p.x = 10
p.y = 10
console.log(p.r) // 14.14...
```

访问器属性与数据属性一样是可以被继承的。

本节只介绍了通过字面量如何创建访问器属性，下一节介绍如何向已存在的对象添加访问器属性。

## 属性特性

特性决定属性是否可写、可枚举、可配置。ES3 无法设置这些特性：所有属性都是可写、可枚举、可配置的。ES5 提供 API 查询和设置属性的特性。

本节我们将会把 getter 和 setter 方法看做属性的特性。并且数据属性的值也可以看做它的特性。于是，我们可以说一个属性包含一个属性名和四个特性。对于数据属性，四个特性是 `value`、`writable`、`enumerable` 和 `configurable`。对于访问器属性，四个特性`get`、`set`、`enumerable` 和 `configurable`。

ES5 中，属性特性通过属性描述符对象查询或设置。属性描述符对象包含几个属性，与特性名相同：对于数据属性，描述符对象的属性有`value`、`writable`、`enumerable` 和 `configurable`。访问器属性的描述符包含 `get`、`set`、`enumerable` 和 `configurable`。`writable`、`enumerable` 和 `configurable` 属性都是布尔值，`get` 和 `set` 属性是函数。

通过 `Object.getOwnPropertyDescriptor()` 获取属性描述符：

```javascript
Object.getOwnPropertyDescriptor({x:1}, "x") // 返回 {value: 1, writable:true, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor({}, "x") // undefined, no such prop
Object.getOwnPropertyDescriptor({}, "toString") // undefined, inherited
```

`Object.getOwnPropertyDescriptor()` 只能用于自有属性。要查询继承来的属性，必须显式的走继承链。

要设置属性的特性，会创建新属性，配以指定的特性，可以调用 `Object.defineProperty()`：

```javascript
var o = {} // 空对象
// 添加属性 x
Object.defineProperty(o, "x", { value : 1, writable: true, enumerable: false, configurable: true})
// 检查属性存在，但是不可枚举
o.x // => 1
Object.keys(o) // => []
// 把属性改成不能修改的
Object.defineProperty(o, "x", { writable: false })
o.x = 2 // 失败或报错（严格模式）
o.x // => 1
// 属性仍是可配置的，因此可以通过以下方式修改属性值
Object.defineProperty(o, "x", { value: 2 })
o.x // => 2
// 将 x 从数据属性改成访问器属性
Object.defineProperty(o, "x", { get: function() { return 0 } })
o.x // => 0
```

若传给 `Object.defineProperty()` 的属性描述符没有带全四个特性，则省略的特性取值 `false` 或 `undefined`。若是修改存在的属性，则省略的特性将保留原值。该方法可以创建新属性，或修改已有的属性，但不会修改继承来的属性。

若想一次修改或创建多个属性，利用 `Object.defineProperties()` 方法。例如：

```javascript
var p = Object.defineProperties({}, {
    x: { value: 1, writable: true, enumerable:true, configurable:true },
    y: { value: 1, writable: true, enumerable:true, configurable:true },
    r: {
        get: function() { return Math.sqrt(this.x*this.x + this.y*this.y) },
        enumerable: true,
        configurable: true
    }
})
```

`Object.defineProperties()` 和 `Object.defineProperty()` 都返回修改后的对象。

`Object.defineProperty()` 和 `Object.defineProperties()` 修改不可修改的属性会抛异常，如尝试向不可扩展的对象添加新属性。属性自己的特性也可能导致修改抛出异常。`writable` 管理是否能改变值。`configurable` 管理是否能许该其他特性（以及属性是否能被删除）。规则有点复杂，比如可以修改一个不可写但可配置的属性的值。如果属性不可配置仍能将属性从可写变成不可写。完整规则如下：

- 若对象是不可扩展的，则你可以编辑已存在的属性，但不能添加新属性。
- 若属性不可配置，则不能修改 `configurable` 和 `enumerable` 特性。
- 若一个访问器属性是不可配置的，则不能改变它的 getter 或 setter 方法，也不能将其改为一个数据属性。
- 如果一个数据属性是不可配置的，则不能将其改为访问器属性。
- 如果一个数据属性是不可配置的，则不能将 `writable` 特性从 false 改为 true，但反过来可以。
- 如果一个数据属性是不可配置且不可写的，不能改变它的值。但可以改变一个可配置但不可写的属性的值（因为你可以将其配置成可写，修改值，然后再改为不可写）。

## 对象的三个特性

每个对象有三个特性：prototype、class、extensible。

### prototype

对象的 prototype 特性指出对象从哪个对象继承。

对象的 prototype 特性在对象创建时设置。

ES5 支持通过静态方法 `Object.getPrototypeOf()` 查询对象的特性。通过表达式 `o.constructor.prototype` 也能确定对象 `o`的 prototype。通过 `new` 创建的对象一般继承一个 `constructor` 属性，指向创建它的构造器函数。构造器函数有一个 `prototype` 属性，执行它创建的对象的 prototype。通过对象字面量创建的对象及通过 `Object.create()` 创建的对象的 `constructor` 指向 `Object` 构造器。Thus, `constructor.prototype` refers to the correct prototype for object literals, but does not usually do so for objects created with `Object.create()`.

要判断一个对象是否是另一个对象的原型（或原型链上的），使用 `isPrototypeOf()` 方法。

```javascript
var p = {x:1}
var o = Object.create(p)
p.isPrototypeOf(o) // => true: o inherits from p
Object.prototype.isPrototypeOf(o) // => true: p inherits from Object.prototype
```

### extensible

`extensible` 特性控制对象是否允许可以添加新属性。

ES5 提供查询和设置可扩展性的函数。`Object.isExtensible()` 用于查询。`Object.preventExtensions()` 使对象不可扩展。对象一旦不可扩展后，无法再允许扩展。扩展性只影响对象自身。原型仍可以自由添加属性。

`Object.seal()` 除了令对象不可扩展，还让对象自己的属性不可配置。于是已存在的属性也不能被删除。`Object.isSealed()` 可以判断对象是否 sealed。

`Object.freeze()` 类似，在对象不可扩展和属性都不可配置的基础上，还令所有数据属性只读。（setter 属性不受影响。）`Object.isFrozen()` 判断对象是否被 frozen。

## 对象作为命名空间

利用对象创建一个上下文（或称为命名空间）。

```javascript
var MYAPP = {}

MYAPP.stooge = {
    "first-name": "Joe",
    "last-name": "Howard"
}

MYAPP.print = function() {
    //...
}
```

