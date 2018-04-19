# 常用数组组合方法

工作中以及自己写的代码中经常会使用到的一些方法总结

## 1.给定一个值，删除数组中的这个值

这里使用到了两个数组的方法`indexOf()`和`splice()`

### 这里复习一下splice和slice的区别

### slice

```javascript
array.slice(start,end)
```

`start`参数指定从数组的何处开始选取，是必需值

`end`参数指定从何处结束选取

```javascript
var a = [1,2,3]
console.log(a.slice(1))  //(2) [2, 3]
console.log(a.slice(2)) // [3]
console.log(a)  //(3) [1, 2, 3]
console.log(a.slice(3)) // []
console.log(a.slice(-1)) // [3]
console.log(a.slice(0,1)) // [1]
```

通常，最后一种指定`start`和`end`的用法是最常使用的。

#### 注意：此方法不会修改原数组，而是返回一个子数组，所以上面的结果都是打印出来的

**注意：**如果没有`end`值，只有一个参数`n`的时候，当`n`为正数，是返回从数组第一个元素开始，删除`n`个元素后的数组，如果`n>=array.length`，返回空数组。

##### 若`n`为负数，与为正数不同，是取值（数组）的效果，所以可以通过参数为负来实现从数组尾部取值的效果。

##### 如果想实现删除原数组中某一个或者某一段元素的效果，就应该使用`splice`。

### splice

常见的用法有以下三种

```javascript
array.splice(start)
array.splice(start,deleteCount)
array.splice(start,deleteCount,item1,item2)
```

`start`参数指定了修改的开始位置（从0计数），为必需值

`deleteCount`参数指定了需要移除元素的个数

`item`参数指定了从`start`位置开始添加`item1`和`item2`，…元素

#### 返回值

`splice`的返回值为被删除的原数组元素组成的一个新的数组，如果没有删除元素则返回空数组。

#### 用splice来给数组添加元素

```javascript
var a = [1,2,3]
a.splice(0,0,0)  //将0添加到a数组的第一个位置
console.log(a) //[0,1,2,3]
a.splice(a.length,0,target) //将目标元素添加到数组的最后一个，这样写就和push一样了
```

### 回到开始，删除数组中的某个指定值的元素

```javascript
var a = [1,2,3]  //指定要删除元素2
a.splice(a.indexOf(2),1)
```

通过`indexOf`找到指定值的索引，完美利用了`splice`的`start`参数。

## 2.判断两个数组中的元素是否相同

**JavaScript是不能用==或===操作符直接比较两个数组是否相等的。**

因为JavaScript中Array是对象，==或者===操作符只能比较两个对象是否是同一实例，也就是是否是同一个对象引用。目前JavaScript没有内置的操作符判断对象的内容是否相同。

当两个数组都可以转换成字符串的情况下，可以使用如下方法

```javascript
var a = [1,2,'a']
var b = [2,'a',1]
console.log(a.sort().toString == b.sort().toString) //true
```

这样的话就算顺序不同也可以进行比较

## 根据数组中对象的某个属性进行排序

```javascript
var arr = [
    {name:"你",index:1},
    {name:"你",index:3},
    {name:"你",index:2}
]

function compare(property) {
    return function(a,b) {
        var value1 = a[property]
        var value2 = b[property]
        return value1 - value2
    }
}

arr.sort(compare('index'))
```

箭头函数

```javascript
var arr = [
    {name:"你",index:1},
    {name:"你",index:3},
    {name:"你",index:2}
]

arr.sort(()=>a.index-b.index)
```

