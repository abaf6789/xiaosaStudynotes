## MVVM框架（双向绑定）

## MVVM框架有哪些

- Vue
- React
- Angular

## 对MVVM框架的认识（与MVC框架对比）

- MVC：Model、View、Control
- MVVM：Model、View、ViewModel

## 双向绑定的原理

正向：通过data的改变驱动view层的改变（**通过ES5中定义的`Object.defineProperty`**）

反向：通过view的改变驱动data的改变（input事件）

### Object.defineProperty()

`Object.defineProperty() `方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

```javascript
Object.defineProperty(obj, prop, descriptor)
```

#### 参数

`obj`

要在其上定义属性的对象。

`prop`

要定义或修改的属性的名称。

`descriptor`

将被定义或修改的属性描述符。

#### 返回值

被传递给函数的对象

#### 描述

该方法允许精确添加或修改对象的属性。默认情况下，使用`Object.defineProperty()`添加的属性值是不可变的。

#### 属性描述符

对象里目前存在的属性描述符有两种主要形式：**数据描述符**和**存取描述符**。**数据描述符**是一个具有值的属性，该值可能是可写的，也可能不是可写的。**存取描述符**是由getter-setter函数对描述的属性。描述符必须是这两种形式之一；不能同时是两者。

**数据描述符和存取描述符均具有**以下可选键值：

- `writable` 特性规定属性值是否可以被设置
- `enumerable` 特性规定 for/in 循环是否包含该属性
- `configurable` 特性规定属性是否可以被删除，以及它的特性是否可以被修改。
- `value`该属性对应的值可以是任何有效的 JavaScript 值（数值，对象，函数等）。**默认为 undefined**。

**存取描述符同时具有以下可选键值**：

- `get`

  一个给属性提供 getter 的方法，如果没有 getter 则为 `undefined`。该方法返回值被用作属性值。**默认为 undefined**。

- `set`

  一个给属性提供 setter 的方法，如果没有 setter 则为 `undefined`。该方法将接受唯一参数，并将该参数的新值分配给该属性。**默认为 undefined**。

#### `object.defineProperty`与`reflect.defineProperty`区别

静态方法 `reflect.defineProperty()` 基本等同于`object.defineProperty`方法，唯一不同是返回` boolean`值。

#### 检查属性是否被成功定义

`Object.defineProperty `方法，如果成功则返回一个对象，否则抛出一个 TypeError 。另外，当定义一个属性时，你也可以使用`try...catch` 块去捕获其中任何的错误。而因为 `Reflect.defineProperty` 返回 `Boolean` 值作为成功的状态，所以你也可以只使用` if…else` 块：

```javascript
if (Reflect.defineProperty(target, property, attributes)) {
  // 成功
} else {
  // 失败
}
```

#### `object.defineProperty`的实际使用

```javascript
var obj1 = {}
var descriptor = Object.create(null) // 没有继承的属性
// 默认没有 enumerable，没有 configurable，没有 writable
descriptor.value = 'static'
Object.defineProperty(obj1, 'key', descriptor)
console.log(obj1)

//显式
var obj2 = {}
Object.defineProperty(obj2, 'key', {
    enumerable: true,
    configurable: false,
    writable: false,
    value: 'static',
})
console.log(obj2)
```

`obj2`当设置`enumerable`整个属性为true时，它的key属性就可以遍历了。