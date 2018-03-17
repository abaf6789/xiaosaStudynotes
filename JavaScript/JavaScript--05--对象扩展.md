# 对象扩展（ES6）

对象不是类生成的对象，指的是Object这个对象。

## 新增特性

- 简洁表示法
- 属性表达式
- 扩展运算符
- Object新增方法

## 简洁表示法

```javascript
{
    //简洁表示法
    let o =1
    let k =2
    
    //ES5:声明一个简易的Object对象
    var es5 = {
        o:o,
        k:k
    }
    
    //ES6
    let es6 ={
        o,
        k
    }
    
    console.log(es5,es6)  //Object{k:2,o:1}  Object{k:2,o:1} 
   
    //ES5:对象中有方法
    let es5_method ={
        hello:function(){
            console.log('Hello')
        }
    }
    
    //ES6:对象中方法
    let es6_method = {
        hello(){
            console.log('Hello')
        }
    }
    console.log(es5_method.hello(),es6_method.hello()): //Hello  Hello
}
```

## 属性表达式

当一个对象中key值因为另一个变量的不同来做变化，就可以实现这种对变量。

```javascript
{
    //ES5：对象的写法
    //ES5中写的属性一定是一个固定的key值；
      ES6中key值是可以用表达式，也可以用变量来做这个key的
    let a ='b';
    let es5_obj ={
        a:'c',
        b:'c'
    }
    
    //ES6:key值[a]中括号包起来的就是表达式；可以直接是一个变量，
    let es6_obj ={
        //a是变量，变量的值是b
        [a]:'c'
    }  
    console.log(es5_obj,es6_obj);
    //Object{            Object{b:"c"}
        a:"c",
        b:"c"
    }
}
```

## Object对象中新增的常用的几个API

```javascript
{
    //判断两个值是不是相等
    //is的判断和===在功能上是一样的
    console.log('字符串',Object.is('abc','abc'),'abc'==='abc');//字符串 true true
    

 //1.数组也是引用类型，在值上都是空，两个数组引用的是两个不同的地址；严格意义上它两不是相等的
 //2.Object.is其实和===没有区别
    console.log('数组',Object.is([],[]),[]===[]); //数组 false false


    //assign--拷贝
    第一个参数要拷贝到的一个对象上；第二个参数把哪个值（哪个对象）拷贝到别的地方去；
    //1.拷贝的属性是有限制的
    （浅拷贝--引用类型值修改引用地址，而不是把所有的值拷过去）
    //2.这个方法拷贝的是只有自身对象的一个属性；如果这个对象有继承，它不会拷贝继承的属性；
        同时也不会拷贝这个对象中不可枚举的一个属性
    console.log('拷贝',Object.assign({a:'a'},{b:'b'}))
    //输出：拷贝 Object{a:"a",b:"b"} 两个对象合并成一个新的对象了

    //entries
    let test ={k:123,o:456}
    for(let [key,value] of Object.entries(test)){
        console.log([key,value])
    }
    //输出：["k",123]
            ["o",456]
}
```

## 扩展运算符（ES7）

```javascript
{
    //...变量；直接是编译不通过的，加入babel-polyfill扩展兼容的，还是会报错
    babel支持不太好，目前实际应用没法用
    let{a,b,...c} ={a:'test',b:'hill',c:'ddd',d:'ccc'}
    //a 解析出来是 a:'test'
      b 解析出来是 b:'hill'
      c ={
         c:'ddd',
         d:'ccc'
      }
}
```

