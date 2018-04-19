# 知识点总结

## CSS相关知识

#### 定位问题

> 设置绝对定位（position：absolute）的参照物是设置了position属性（position：static无效）的父（祖先）元素。你的代码中不符合这样的规则。一般给父级元素设置position：relative。
>
> 1. 当没有可以作为参照物的父元素时，元素会做如下处理：（可以确定当绝对定位元素的所有祖先元素都没有有效的position定位时，不是以body和html为参照物的。）
>    a. web页面最外层的约束为body，就是浏览器面板能看到的部分。
>    b. body即可理解为文档流，从上到下，从左向右。
>    c. 文档流超过了浏览器面板（可视区域），就会出现滚动条。
>    d. absolute的定制，就会将元素脱离文档流，不占据文档流，元素将不出现在文档流中，当浏览器渲染时，不会出现滚动条。
>    e. 在文档流“流动”时，浏览器屏幕（范围），会在文档流中截出一块区域。可以简记为“第一屏”。
>    f. 当没有定制relative时，absolute（元素无论大小）即以此定位。

#### Flex布局

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html（阮一峰老师写的，很详细）

#### React虚拟dom与jquery的区别

> 虚拟的DOM的核心思想是：对复杂的文档DOM结构，提供一种方便的工具，进行最小化地DOM操作。简单来说就是DOM由JS托管，例如在修改页面元素以后，把渲染的工作交给js而不是浏览器，让js去计算哪些布局需要更改，再将它转换为真实DOM渲染出来，从而减小浏览器计算的复杂度。

## JS数组有哪些方法，分别是干什么的

没有回答完全，有很多是自己知道的，但是因为这不是一个业务上的需求，所以没有一下子想起来

> var list = new Array() 
>
> list[0] = 0; 
>
> list[1] = 1; 
>
> list[2] = 2; 
>
> 或者这样声明：var list = [0,1,2] 
>
> shift():删除数组的第一个元素,返回删除的值。这里是0 
>
> unshift(3,4):把参数加载数组的前面，返回数组的长度。现在list:中是3,4,0,1,2 
>
> pop():删除数组的最后一个元素，返回删除的值。这里是2
>
> push(3):将参数加载到数组的最后，返回数组的长度，现在List中时：0,1,2,3 
>
> concat(3,4):把两个数组拼接起来。 
>
> splice(start,deleteCount,val1,val2,...)：从start位置开始删除deleteCount项，并从该位置起插入val1,val2,... 
>
> reverse：将数组反序 
> var a = [1,2,3,4,5] 
> var b = a.reverse() //a：[5,4,3,2,1] b：[5,4,3,2,1] 
>
> sort(orderfunction)：按指定的参数对数组进行排序 
> var a = [1,2,3,4,5]; 
> var b = a.sort(); //a：[1,2,3,4,5] b：[1,2,3,4,5] 
>
> slice(start,end)：返回从原数组中指定开始下标到结束下标之间的项组成的新数组 
> var a = [1,2,3,4,5] 
> var b = a.slice(2,5)   //a：[1,2,3,4,5] b：[3,4,5] 
>
> join(separator)：将数组的元素组起一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符 
> var a = [1,2,3,4,5]
> var b = a.join("|"); //a：[1,2,3,4,5] b："1|2|3|4|5"

## JS数组去重的几种办法

1. 最简单的一种

```javascript
function uniq(array){
    var temp = []; //一个新的临时数组
    for(var i = 0; i < array.length; i++){
        if(temp.indexOf(array[i]) == -1){
            temp.push(array[i]);
        }
    }
    return temp;
}

var aa = [1,2,2,4,9,6,7,5,2,3,5,6,5];
console.log(uniq(aa));
```

2.获取没重复的最右边的值放入新数组

```javascript
function uniq(array){
    var temp = [];
    var index = [];
    var l = array.length;
    for(var i = 0; i < l; i++) {
        for(var j = i + 1; j < l; j++){
            if (array[i] === array[j]){
                i++;
                j = i;
            }
        }
        temp.push(array[i]);
        index.push(i);
    }
    console.log(index);
    return temp;
}

var aa = [1,2,2,3,5,3,6,5];
console.log(uniq(aa));
```

3.ES6数组去重方法（Set）

```javascript
var a = [1,2,2,4]
var newa = Array.from(new Set(a))
//或者
var newb = [...new Set(a)]
```



## 获取页面上所有标签的种类的个数

首先是类数组对象如何转换成数组

什么是类数组对象：类数组对象（有length属性的对象）和可遍历对象（ES6新增的Set与Map数据结构） 转化为真正的数组，

使用`Array.from()`方法

```javascript
var a = document.getElementsByTagName('*')
// a是一个类数组对象
var b = Array.from(a)
// b是由a转换而来的数组
var c = [...new Set(b)]
// c就是去重后的结果
```

写成一句话

```javascript
var d = [...new Set(Array.from(document.getElementsByTagName('*')))]
```

## 前端缓存

### 缓存的分类：

> **缓存：**对应文件在浏览器中存在的备份（副本），把请求的东西缓存到本地了（是放在电脑磁盘中的），浏览器下次请求相当于是从这个磁盘直接读了，而不会再去请求这个文件的地址。

#### 强缓存

```json
http协议头：
Expires       Expires:Thu,21 Jan 2017 23:39:02 GMT
Cache-Control      Cahe-Control : Max-age=3600 
```

> **强缓存：**问都不问，不直接请求，直接拿过来就用了。 不管是绝对时间还是相对时间，这个时间过期之前不会和服务器通信了，直接从浏览器副本拿出来给页面用

##### HTTP协议头：

 在请求一个文件的时候，http头上（响应头上）会带两个东西（有可能是2个，也有可能是1个，根据服务器配置）

 响应头中会有key，value 
 Expires  过期时间； value值表示的是绝对时间（服务器的绝对时间，这个时间是服务器下发的）
 判断客户端当前的时候是不是这个时间，比较的时候是按浏览器本地的时间做比较，下发的是服务器端的时间，会有偏差
 缺点：有可能客户端和服务器的时间不一致。

Cache-Control   value值   Max-age=3600（相对时间）
         不管客户端和服务器时间是否一致，它最后是以客户端相对时间为止，时间单位是秒（拿到资源之后再3600s之内不会再去请求服务器，在这个时间之内直接去浏览器拿缓存）

如果服务器两个时间都下发了，依哪个时间为准呢？
以后者相对时间为准（这个规定）

##### 协商缓存：

Last-Modified If-Modified-Since   Last-Modified:Wed,26 Jan 2017 00:35:11 GMT
Etag   If-None-Match

> **协商缓存：**浏览器发现本地有这个副本，但是又不确定用不用它，去服务器问一个这个文件要不要用（和服务器协商这个文件能不能用，过期没有）     

**Last-Modified(服务器下发的上次修改的时间)**
在拿到这个文件的时候，浏览器会给这个资源文件的http响应头中加一个Last-Modified，value值就是时间
**If-Modified-Since:(请求中给服务器带的，服务器要对比，所以一来一回，两个东西)**
当强缓存失效（过期了），浏览器在这个时间之外又开始请求了，不确定这个东西有没有变化，要携带上次给的时间
是哪一个，请求的时候会以携带这个字段的时间。（拿到新资源文件，会通过Last-Modified下发一个时间，当下次请求问服务器这个资源有没有发生变化，是用http请求
        头中加If-Modified-Since（key值），他们两个值是一个）
*缺点：虽然hash值变了，内容并没有变化，完全可以从副本拿，Etag就是解决这个问题的。
Etag（哈希值）：
    服务器给你这个资源的时候会给一个Etag值，当过了强缓存的时间，浏览器在问服务器请求问它这个资源可不可以在用的时候，
    会通过这个http中加一个key值（If-None-Math）一个value，那么value就是发的那个Etag值。
If-None-Match

## **和缓存相关的http头有哪些？能写出来几个？**

> Expires
> Cache-Control
> Last-Modified
> Etag
> If-None-Match
> 缓存可以提升性能

**使用CDN（网络优化，CDN加载资源非常快）；**
CDN可以让客户以最快的时间把资源请求过来；
（让网络快速到达服务器端把文件下载下来；）
当页面第一次打开的时候，浏览器缓存是起不了作用的，使用CDN效果是非常明显的；
—**预解析DNS**
<meta http-equiv=“x-dns-prefetch-control” content=“on”>
-页面中所有的a标签，在一些高级浏览器中默认打开了DNS预解析的；(不加这句话，a标签也是可以做到DNS预解析的，这是浏览器的一个功能；)但是如果你的页面是https协议头的，很多浏览器是默认关闭DNS预解析的。这句话是强制打开a标签的DNS预解析。

- **怎么使用预解析DNS呢？就是使用下面的语句：**
  <link rel=“dns-prefetch” href=“//host_name_to_prefetch.com”>
- **从浏览器输入一个url到页面真正渲染完中间发生了哪些环节？**
  -第一步就是DNS解析，当你的页面涉及多个域名的时候，DNS解析效果是非常明显的



## 性能优化

- **资源压缩合并，减少HTTP请求（资源传输的过程变小）**
- **非核心的代码异步加载—异步加载的方式—异步加载的区别**

异步加载的方式：

1.动态脚本加载

2.defer

3.async

- **异步加载的区别：**
  1.defer是在HTML解析完之后才会执行，如果是多个，按照加载顺序依次执行
  2.async是在加载完之后立即执行，如果是多个，执行顺序和加载顺序无关
- **异步加载原理：（任何一个时刻都只能做一件事）**
  1.**defer：**新加载的，在HTML解析完之前，是不会执行的
  defer能保证按你的加载顺序依次执行
  2.**async：**
  哪个先回来，哪个先执行，和加载顺序无关

## http有哪些请求头，请求体，请求方法

#### 请求方法 **URI协议/版本**

> 请求的第一行是“方法URI协议/版本”例如：GET/sample.jsp HTTP/1.1
>
> 以上代码中“GET”代表请求方法，“/sample.jsp”表示URI，“HTTP/1.1代表协议和协议的版本。



#### 请求头（Request Header）

> 请求头包含许多有关的客户端环境和请求正文的有用信息。例如，请求头可以声明浏览器所用的语言，请求正文的长度等。例如：

```
Accept:image/gif.image/jpeg.*/*

Accept-Language:zh-cn

Connection:Keep-Alive

Host:localhost

User-Agent:Mozila/4.0(compatible:MSIE5.01:Windows NT5.0)

Accept-Encoding:gzip,deflate.
```



#### 请求正文

> 请求头和请求正文之间是一个空行，这个行非常重要，它表示请求头已经结束，接下来的是请求正文。请求正文中可以包含客户提交的查询字符串信息：

username=jinqiao&password=1234

在以上的例子的HTTP请求中，请求的正文只有一行内容。当然，在实际应用中，HTTP请求正文可以包含更多的内容。

### 三面

## 对Promise的了解（一面也问了）

https://github.com/abaf6789/xiaosaStudynotes/blob/master/JavaScript/JavaScript--08--Promise.md

## let const的区别

https://github.com/abaf6789/xiaosaStudynotes/blob/master/JavaScript/JavaScript--01--%E5%9F%BA%E7%A1%80.md

## 二叉树遍历的实现

二叉树是由根节点，左子树，右子树组成，左子树和友子树分别是一个二叉树。

```javascript
//一个二叉树的例子
var tree = {
 value: 1,
 left: {
  value: 2,
  left: {
   value: 4
  }
 },
 right: {
  value: 3,
  left: {
   value: 5,
   left: {
    value: 7
   },
   right: {
    value: 8
   }
  },
  right: {
   	value: 6
  }
 }
}
```

**广度优先遍历**

广度优先遍历是从二叉树的第一层（根结点）开始，自上至下逐层遍历；在同一层中，按照从左到右的顺序对结点逐一访问。

```javascript
var levelOrderTraversal = function(node) { 
 if(!node) {  
  throw new Error('Empty Tree')
 } 
 var que = []
 que.push(node) 
 while(que.length !== 0) {
  node = que.shift()  
  console.log(node.value)  
  if(node.left) que.push(node.left)  
  if(node.right) que.push(node.right)
 }
}
```

**递归遍历**觉得用这几个字母表示递归遍历的三种方法不错：
D：访问根结点，L：遍历根结点的左子树，R：遍历根结点的右子树。
**先序遍历：DLR**

**中序遍历：LDR**

**后序遍历：LRD**

- 先序遍历：

```javascript
var preOrder = function (node) { 
 if (node) {  
  console.log(node.value);
  preOrder(node.left);
  preOrder(node.right);
 }
}
```

- 中序遍历：

```javascript
var inOrder = function (node) { 
 if (node) {
  inOrder(node.left);  
  console.log(node.value);
  inOrder(node.right);
 }
}
```

- 后序遍历：

```javascript
var postOrder = function (node) { 
 if (node) {
  postOrder(node.left);
  postOrder(node.right);  
  console.log(node.value);
 }
}
```



# 后记

- 前端vue、后端koa2，从今天开始学node吧
- 现阶段的不足在于计算机基础薄弱，计算机基础可能在刚开始工作的几年用到的不会很多，但是如果做了几年需要做构架的时候，这些基础知识是非常有用的
- 现阶段的提升在于：多读计算机专业的相关书籍