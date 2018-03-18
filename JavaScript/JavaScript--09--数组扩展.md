# 数组扩展（ES6）

### 数组新增特性（ES6）

- **Array.of**
- **Array.from**
- **copyWithin**
- **find\findIndex**
- **fill**
- **entries\keys\values**
- **includes**

### 数组特性（ES5）

- **Array.isArray()** 
- **Array.prototype.concat()**
- **Array.prototype.every()** 
- **Array.prototype.filter()** 
- **Array.prototype.forEach()**

## ES6新特性

### Array.of() 

`Array.of()` 收集所有参数，放入一个数组。

```javascript
Array.of(7)       // [7] 
Array.of(1, 2, 3) // [1, 2, 3]

Array(7)          // [ , , , , , , ]
Array(1, 2, 3)    // [1, 2, 3]
```

### Array.from() 

`Array.from()` 将类似数组的对象或 iterable 对象转换为一个真实的数组。类似数组的对象有 `length` 属性并且可以被下标索引。

在转换数组的同时还进行了遍历，把整个数组map跑了一遍，把里面每个元素根据条件转换。

语法：

```javascript
Array.from(arrayLike[, mapFn[, thisArg]])
```

- `arrayLike`：类似数组的对象或 iterable 对象
- `mapFn`：可选，映射函数，对每个元素调用。函数有两个参数，第一个是元素值，第二个是索引。
- `thisArg`：可选，用于执行 `mapFn`。

例子：

```javascript
<p>ES6</p>  
<p>ES5</p> 
<p>html</p> 
<p>css</p> 
<p>javascript</p> 

{
  //1
    let p = document.querySelectorAll('p') //得到的p的集合
    let pArr = Array.from(p)
    
    pArr.forEach(function(item){
    
        //原生js：获取DOM节点文本内容的属性；
        console.log(item.textContent) //ES6  ES5  html  css  javascript
    })
    
    
    //2.在转换数组的同时还进行了遍历（map），把整个数组map跑了一遍，把里面每个元素乘以2；
    let arr2 = Array.from([1,3,5],function(item){return item*2})
    console.log(arr2) //[2,6,10]
}
```

### Array.prototype.copyWithin() 

`copyWithin()` 将同一个数组的一块区域粘贴（覆盖）到另一块区域，不改变数组大小。

```javascript
['alpha', 'bravo', 'charlie', 'delta'].copyWithin(2, 0)
// results in ["alpha", "bravo", "alpha", "bravo"]
```

语法：

```javascript
arr.copyWithin(target)
arr.copyWithin(target, start)
arr.copyWithin(target, start, end)
```

参数：

- `target`：拷贝到的位置。负数从后面计算。如果 `target` 大于等于 `arr.length`，什么也不发生。If target is positioned after start, the copied sequence will be trimmed to fit `arr.length`.
- `start`：可选。拷贝开始的位置。负数从后面计算。若省略，默认值 0。
- `end`：可选。拷贝结束的位置（不含）。负数从后面计算。若省略，默认值 `arr.length`。

修改数组本身，并返回该数组。

`copyWithin` 类似于 `memmove`，是一种高性能的移动数组数据的方法。主要用于 TypedArray。拷贝和粘贴的区域重叠也不要紧。

例子：

```javascript
[1, 2, 3, 4, 5].copyWithin(-2)
// [1, 2, 3, 1, 2]

[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(-2, -3, -1)
// [1, 2, 3, 3, 4]

[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

var i32a = new Int32Array([1, 2, 3, 4, 5])

i32a.copyWithin(0, 2)
// Int32Array [3, 4, 5, 4, 5]

[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4)
// Int32Array [4, 2, 3, 4, 5]
```

### Array.prototype.find() 

`find()` 返回满足条件的第一个元素。

`findIndex()` 返回的是索引而不是元素。

语法：

```javascript
arr.find(callback[, thisArg])
```

回调函数有三个参数，分布是当前元素，当前索引和数组本身。

如果找不到，返回 `undefined`。

回调对数组的所有索引调用，即从 0 到 length - 1。因此对于稀疏数组效率比较低。

处理元素的范围在第一次执行回调时设置。后续添加的元素不会被回调处理。如果数组中值有被修改，以访问时的值为准。被删除的值不会被执行。

例子：

```javascript
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
]

function findCherries(fruit) { 
    return fruit.name === 'cherries'
}

console.log(inventory.find(findCherries)) 
// { name: 'cherries', quantity: 5 }
```

### Array.prototype.findIndex() 

`findIndex()` 返回满足条件的第一个元素的索引。找不到返回 -1。

语法：

```javascript
arr.findIndex(callback[, thisArg])
```

回调函数有三个参数，分布是当前元素，当前索引和数组本身。

回调对数组的所有索引调用，即从 0 到 length - 1。因此对于稀疏数组效率比较低。

处理元素的范围在第一次执行回调时设置。后续添加的元素不会被回调处理。如果数组中值有被修改，以访问时的值为准。被删除的值不会被执行。

例子：

```javascript
function isPrime(element, index, array) {
  var start = 2
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false
    }
  }
  return element > 1
}

console.log([4, 6, 8, 12].findIndex(isPrime)) // -1, not found
console.log([4, 6, 7, 12].findIndex(isPrime)) // 2
```

### Array.prototype.fill() 

`fill()` 以固定值填充数组指定区域。

语法：

```javascript
arr.fill(value)
arr.fill(value, start)
arr.fill(value, start, end)
```

`start` 和 `end` 均可以省略，默认值分别是 0 和 `this.length`。区间是 `[start, end)`。返回修改后的数组。

如果 `start` 为负，当做 `length+start`，如果 `end` 为负，当做 `length+end`。

```javascript
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}
```

### Array.prototype.entries() （ES6）

`entries()` 返回一个数组 Iterator 对象，每次返回一个“索引/值”对。

```javascript
var a = ['a', 'b', 'c']
var iterator = a.entries()

console.log(iterator.next().value) // [0, 'a']
console.log(iterator.next().value) // [1, 'b']
console.log(iterator.next().value) // [2, 'c']
```

例子：遍历keys、values、entries

```javascript
{
    for(let index of ['1','c','ks'].keys()){
        console.log('keys',index); // keys 0 1 2
    }
    
    //values()在chrome不做兼容处理是没法用的，会报错的；
     for(let value of ['1','c','ks'].values()){
        console.log('values',value); // values 1 c ks
    }

    //既想取到索引又想取到值；entries没有兼容性问题；
     for(let [index,value] of ['1','c','ks'].entries()){
        console.log('values',index,value); 
        // values 0  1 
        // values 1  c
        // values 2  ks
    }
}
```

### Array.prototype.includes()

检测数组中是否包含这个值

```javascript
{
    console.log('Number',[1,2,NaN].includes(1)); //Number true
    console.log('NaN',[1,2,NaN].includes(NaN)); //NaN true
}
```

##  ES5特性

### Array.isArray() 

`Array.isArray()` 判断传入值是否是一个 `Array`。

```javascript
Array.isArray([1, 2, 3])  // true
Array.isArray({foo: 123}) // false
Array.isArray('foobar')  // false
Array.isArray(undefined)  // false
```

### Array.prototype.concat()

`concat()` 合并两个或多个数组。不改变这些数组，返回一个新数组。

语法：

```javascript
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

参数可以是数组或单个值。不会递归展开嵌套的数组。

例子：

```javascript
var alpha = ['a', 'b', 'c']
var numeric = [1, 2, 3]
alpha.concat(numeric)
//  ['a', 'b', 'c', 1, 2, 3]
var num1 = [1, 2, 3],
    num2 = [4, 5, 6],
    num3 = [7, 8, 9]
var nums = num1.concat(num2, num3)
console.log(nums) 
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
var alpha = ['a', 'b', 'c']
var alphaNumeric = alpha.concat(1, [2, 3])
console.log(alphaNumeric)
// ['a', 'b', 'c', 1, 2, 3]
```

### Array.prototype.every() 

`every()` 测试是否所有元素满足条件。

语法：

```
arr.every(callback[, thisArg])
```

回调函数有三个参数，分别是当前元素、索引和数组本身。如果有一个回调函数返回假值，遍历结束。

只对有值的索引执行回调函数，如果数组的一个索引被删除或从未被赋值，不会在此索引处执行回调。

处理元素的范围在第一次执行回调时设置。后续添加的元素不会被回调处理。如果数组中值有被修改，以访问时的值为准。被删除的值不会被执行。

空数组总是返回 true。

例子：

```javascript
function isBigEnough(element, index, array) {
    return element >= 10
}
[12, 5, 8, 130, 44].every(isBigEnough)   // false
[12, 54, 18, 130, 44].every(isBigEnough) // true
```

### Array.prototype.filter()

`filter()` 创建一个新数组，只包含通过测试的元素。

语法：

```javascript
var newArray = arr.filter(callback[, thisArg])
```

回调函数返回 true 保留元素。回调函数有三个参数，分布是当前元素，当前索引和数组本身。

只对有值的索引执行回调函数，如果数组的一个索引被删除或从未被赋值，不会在此索引处执行回调。

`filter()` 不修改原数组。

处理元素的范围在第一次执行回调时设置。后续添加的元素不会被回调处理。如果数组中值有被修改，以访问时的值为准。被删除的值不会被执行。

例子：

```javascript
function isBigEnough(value) {
    return value >= 10
}

var filtered = [12, 5, 8, 130, 44].filter(isBigEnough)
// filtered is [12, 130, 44]
```

### Array.prototype.forEach()

`forEach()` 方法对数组的每个元素执行一次提供的函数。

**注意：没有返回一个新数组，没有返回值。**

```javascript
const arr = ['a', 'b', 'c']

arr.forEach(function(element) {
    console.log(element)
})

arr.forEach( element => console.log(element))
// a
// b
// c
```

