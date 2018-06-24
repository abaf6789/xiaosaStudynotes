# 原型与原型链

## 构造函数创建对象

什么是构造函数？

```javascript
function Person() {
    
}
var person = new Person()
person.name = 'Kevin'
console.log(person.name)  //Kevin
```

上述例子中`Person`就是一个构造函数，我们使用`new`创建一个实例对象`person`。

## prototype

每个函数都有一个`prototype`属性

```javascript
function Person() {
    
}
//prototype是函数才会有的属性
Person.prototype.name = 'Kevin'
var person1 = new Person()
var person2 = new Person()
console.log(person1.name) //Kevin
console.log(person2.name) //Kevin
```

函数的`prototype`属性指向了一个对象，这个对象正是调用该构造函数而创建的实例的原型，也就是这个例子中的`person1`和`person2`的原型。

### 什么是原型？

每一个`JavaScript`对象（`null`除外），在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型**继承**属性。

## __proto__

每一个`JavaScript`对象（`null`除外）都有一个属性，叫`__proto__`，这个属性会指向该对象的原型，接下来整理一下实例和实例原型之间的关系。

`Person`：构造函数

`Person.prototype`：实例原型

`person`：实例

### 实例与实例原型间的关系

```javascript
person.__proto__ === Person.prototype   //true
```

## constructor

实例和构造函数都可以指向原型，原型没有属性可以指向实例，因为一个构造函数可以生成多个实例。

**但是原型指向构造函数是有的，这里就要引入`constructor`这个属性，每个原型都有一个`constructor`属性指向关联的构造函数**。

```javascript
function Person() {
    
}
console.log(Person === Person.prototype.constructor) //true
```

## 整理

```javascript
function Person() {
    
}
var person = new Person()
console.log(person.__proto__ === Person.prototype) //true
console.log(Person === Person.prototype.constructor) //true
//ES5中获得对象原型的方法
console.log(Object.getPrototypeOf(person) === Person.prototype) //true
```

