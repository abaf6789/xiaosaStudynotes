# 类

## 类定义（ES6）

ES6 的类只是基于原型的语法糖；更易用。类支持基于原型的继承、调用父类、实例和静态方法，构造器。

```javascript
class SkinnedMesh extends THREE.Mesh {
    constructor(geometry, materials) {
        super(geometry, materials)
        this.idMatrix = SkinnedMesh.defaultMatrix()
        this.bones = []
        this.boneMatrices = []
        //...
    }
    update(camera) {
        //...
        super.update()
    }
    static defaultMatrix() {
        return new THREE.Matrix4()
    }
}
```

### 类的基本定义和生成实例

```javascript
{
    //定义一个类的基本方法：class 名称{ }
    class Parent{
        constructor(name='abc'){
            this.name = name
        }
	}   
    /生成实例；v就是构造函数里面的参数；
    let v_parent = new Parent('v')
    console.log('构造函数和实例',v_parent) //构造函数和实例 Parent{name:"v"}
}
```

### 静态方法

```javascript
{
     class Parent{
        constructor(name='abc'){
            this.name = name
        }        
        //静态方法的定义：static 方法名称(){ 函数要执行的具体内容 }
        //加了static这个方法就变成静态方法,static只是用来定义静态方法；
        //静态方法：通过类去调用，而不是通过类的实例调用；
        static tell(){
            console.log('tell')
        }
    }   
    //静态方法的使用；
    Parent.tell()  // tell
}
```

### 静态属性

```javascript
{
    class Parent{
        constructor(name='abc'){
            this.name = name
        }        
        static tell(){
            console.log('tell')
        }    
	}
    
    //静态属性没有关键词；
    //类定义完之后，直接在类上定义静态属性；
    //这个Parent注意一定是类，不是实例；
    Parent.type = 'test'
    
    console.log('静态属性的读取',Parent.type) //静态属性的读取 test  
}
```

### 类中的getter和setter

```javascript
{
    class Parent{
        constructor(name='abc'){
            this.name = name
        }
    
        //getter的写法：get 名字(){}这个是属性，不是方法；
        //读取longName属性；
        get longName(){
            return 'mk'+this.name
        }
    
        //setter
        set longName(value){
            this.name = value
        }
    }
    
    let v = new Parent()
    console.log('getter',v.longName) //getter mkabc
    
    v.longName ='hello'
    console.log('setter',v.longName) //setter mkhello
}
```



## ES6 之前

### 构造器函数

ES6 之前，JavaScript 没有类的概念，但有构造器的概念。构造器也是普通的函数，只是首字母一般大写，调用时加 `new` 关键字。

```javascript
function Tester() { this.a = 1 }
var a = new Tester()
a instanceof Tester // true
console.log(a.a) // 1
```

构造产生的对象，可以显式返回或隐式返回。显式返回：

```javascript
function Tester() {
    var a = 1
    return {good: a}
}
var a = new Tester()
a.good // 1
```

利用这种特性实现私有封装：

```javascript
function Tester() {
    var age = 80
    return {
        getAge: function() { return age },
        setAge: function(v) { age = v }
    }
}
var a = new Tester()
a.getAge() // 80
a.age // undefined
```

隐式返回对象，即构造函数内不使用 `return` 语句，通过隐式变量 `this` 添加属性和方法。

```javascript
function Tester() {
    this.good = 1
    console.log("done")
}
var a = new Tester()
a.good // 1
```

使用 `new` 构造的对象，构造函数即类：

```javascript
function Tester() {
    this.good = 1
}
var a = new Tester()
a instanceof Tester // true
```

### `prototype` 与方法

通过构造函数的 `prototype` 定义实例方法：

```javascript
function Range(from, to) {
    this.from = from
    this.to = to
}

Range.prototype.includes = function(x) {
    return this.from <= x && x <= this.to
}

Range.prototype.foreach = function(f) {
    for(var x = Math.ceil(this.from) x <= this.to x++) f(x)
}

Range.prototype.toString =  function() {
    return "(" + this.from + "..." + this.to + ")" }
}

var r = new Range(1,3)
r.includes(2)
r.foreach(console.log)
console.log(r)
```

`prototype` 还有一个不可枚举的属性 `constructor`：

```javascript
var F = function() {}
var p = F.prototype
var c = p.constructor
c === F // => true: F.prototype.constructor==F for any function
```

类的实例也有 `constructor` 属性，也等于其构造函数。

```javascript
var o = new F(); // Create an object o of class F
o.constructor === F // => true: the constructor property specifies the class
```

### instanceof

两个对象继承自同一个原型对象时，它们是同一个类的实例。构造函数只是“类”的“外在表现”。例如下面的代码，`instanceof` 实际不会检查 `r` 是否是 `Range` 构造的。只是检查 `r` 是否继承自 `Range.prototype`。

```javascript
r instanceof Range // returns true if r inherits from Range.prototype
```