# Promise（ES6）

promise是一种处理异步的思路。`Promise`也是一个类。当我们说一个promise的时候，一般指一个`Promise`对象。

首先我们来搞清什么是异步

- 什么是异步

  比如：函数a，执行一个步骤，a执行完要执行b，这是一个顺序的问题；

  在程序上设置a执行完执行b，一种通过回调方式，一种通过事件触发方式。

## 传统回调 VS Promise

由于我入行前端比较晚，所以没有使用过传统回调，但是只有与传统回调相对比，才能体现出`Promise`的强大（同事告诉我的TAT）。

- 传统回调

```javascript
fs.readFile('config.json', function (error, text) {
    if (error) {
        console.error('Error while reading config file')
    } else {
        try {
            const obj = JSON.parse(text)
            console.log(JSON.stringify(obj, null, 4))
        } catch (e) {
            console.error('Invalid JSON in file')
        }
    }
})
```

- Promise

```javascript
function readFilePromisified(filename) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filename, { encoding: 'utf8' }, (error, data) => {
            if (error) {
                reject(error)
            } else {
                resolve(data)
            }
        })
    })
}

readFilePromisified('config.json').then(function (text) { // (A)
    const obj = JSON.parse(text)
    console.log(JSON.stringify(obj, null, 4))
}).catch(function (error) { // (B)
    // File read error or JSON SyntaxError
    console.error('An error occurred', error)
})
```

其中 `readFilePromisified` 方法可以通过库快速改写，如通过 bluebird 库：

```javascript
const readFilePromisified = bluebird.promisify(fs. fs.readFile)
```

### Promise的优点

使用`promise`代替传统回调等机制，主要有两方面优势：一是Promise写法更简练方便、清晰、表达力更强。二是传统回调需要处理很多问题，比如重复回调，错误处理，回调注册时机，组合多个异步操作等，非常容易出问题，而使用Promise可以规避这类问题（没有经历过传统回调，听同事说传统回调需要捕获很多异常，不断try…catch,try…catch，经常容易出错）

### Promise的状态

一个Promise会处于下面三种状态中的一种：

- 异步结果就续前，Promise处于`pending`状态
- 获得结果后，Promise处于`fulfilled`状态
- 如果发生了错误，Promise处于`rejeced`状态

Promise落定（settled），指它已获得了结果或发生了错误（fulfilled或rejected）。Promise一旦落定，状态不会再改变。

Promise提供两种接口使Promise从进行的状态（pending）达到落定状态（settled）：

- 一类是拒绝（`reject`）方法，让Promise进入`rejected`状态
- 一类是解决（`resolve`）方法，如果解决的值不是一个Promise，则进入`fulfilled`状态；否则如果解决的值本身又是一个Promise，则要等这个Promise达到落定状态。

## 创建Promise

### 通过构造器

```javascript
const p = new Promise(function (resolve, reject) { // (A)
    ···
    if (···) {
        resolve(value) // success
    } else {
        reject(reason) // failure
    }
})
```

并且如果在函数中抛出了异常，`p` 自动被该异常拒绝。

```javascript
const p = new Promise(function (resolve, reject) {
    throw new Error("Bad")
})
p.catch(function(e) { // 将执行这里
    console.error(e)
})
```

### thenable

thenable 是一个对象，具有类似与 Promise 的 `then()` 方法。thenable 不是一个 Promise。它一般是 Promise 标准化之前出现的采用 Promise 思想的自定义的对象。通过 `Promise.resolve(x)` 可以将一个非标准化的 thenable 转换为一个 Promise，见下一节。

#### Promise.resolve(x)

对于`x`不是一个Promise，`Promise.resolve(x)`返回一个Promise，该Promise立即以`x`的值解决。

```javascript
Promise.resolve('abc')
    .then(x => console.log(x)) // abc
```

如果 `x` 是一个 Promise，则 `Promise.resolve(x)` 原样返回 `x`。

```javascript
const p = new Promise(() => null)
console.log(Promise.resolve(p) === p) // true
```

如果 `x` 是一个 thenable，则把它转换为一个 Promise。

```javascript
const fulfilledThenable = {
    then(reaction) {
        reaction('hello')
    }
}
const promise = Promise.resolve(fulfilledThenable)
console.log(promise instanceof Promise) // true
promise.then(x => console.log(x)) // hello
```

总结：通过 `Promise.resolve()` 将任何值转换为一个 Promise。

### Promise.reject(err)

`Promise.reject(err)` 返回一个 Promise，以 `err` 拒绝：

```javascript
const myError = new Error('Problem!')
Promise.reject(myError)
    .catch(err => console.log(err === myError)) // true
```

## 消费一个Promise

通过`then`和`catch`注册处理方法，再Promise解决或拒绝时调用。

```javascript
promise
    .then(value => { /* fulfillment */ })
    .catch(error => { /* rejection */ })
```

使用回调的一个常见问题时，还来不及注册回调函数，异步执行就结束了。但 Promise 没有这个问题。如果一个 Promise 对象落定了，会保持住状态以及解决的值或拒绝的异常。此时再使用 `then` 或 `catch` 注册处理方法仍可以得到结果。

`then` 还有一个两参数的写法：

```javascript
promise.then(
    null,
    error => { /* rejection */ })

promise.catch(
    error => { /* rejection */ })
```

#### 具体实例

```javascript
const q1 = pPostServer("c/sign-in", {username, password})
        q1.then(function() {
            setTimeout(page("/"), 100)
        }).catch(err => {
            if (err.message === "无此用户") {
               alert("用户名不存在")
            } else {
                alert("密码错误")
            }
        })
```

上面这个例子是我在工作中遇到的，首先请求的这个接口是用来检测用户名是否存在，密码是否错误的。

当用户名密码都正确并匹配时执行`then`后的内容，当用户名密码错误时，执行`catch`后的内容。

### Promise的传递

`then()` 方法会返回一个新的 Promise，于是你可以链接调用：

```javascript
const q = Promise.resolve(true).then(function() {
    return new Promise(function(resolve) {
        setTimeout(function() {
            resolve("Good")
        }, 1000)
    })
})
q.then(function(result) {
    console.log(result) // Good
})
```

上述代码其实先后会产生 4 个 Promise：

```javascript
const q1 = Promise.resolve(true)
const q2 = q1.then(function() {
    const q3 = new Promise(function(resolve) {
        setTimeout(function() {
            resolve("Good")
        }, 1000)
    })
    return q3
})
const q4 = q2.then(function(result) {
    console.log(result)
})
```

具体做公司业务时，`.then(onFulfilled, onRejected)`返回的Promise值取决于它的回调函数的执行。

### 链式调用的优点

```javascript
asyncFunc1()
    .then(asyncFunc2)
    .then(asyncFunc3)
    .catch(function (reason) {
        // Something went wrong above
    })
```

链式调用可以在最后统一处理错误。中间环节的任何错误可以被最终统一处理。

### 串行与并行

通过then链式调用异步函数，这些函数的执行是串行的：

```javascript
asyncFunc1()
    .then(() => asyncFunc2())
```

如果不使用 `then` 连接，它们是并行执行的，但是你拿不到结果，也不知道什么时候他们全部完成。

```javascript
asyncFunc1()
asyncFunc2()
```

### Promise.all()

解决方法是使用 `Promise.all()`。它的参数是一个数组，数组元素是 Promise。它返回一个 Promise，解析的结果是一个数组。

```javascript
Promise.all([ asyncFunc1(), asyncFunc2() ])
    .then(([result1, result2]) => {
        ···
    })
    .catch(err => {
        // Receives first rejection among the Promises
        ···
    })
```

如果 `map` 的映射函数返回一个 Promise，`map` 产生的数组由 `Promise.all()` 处理：

```javascript
const fileUrls = [
    'http://example.com/file1.txt',
    'http://example.com/file2.txt',
]
const promisedTexts = fileUrls.map(httpGet)

Promise.all(promisedTexts)
    .then(texts => {
        for (const text of texts) {
            console.log(text)
        }
    })
    .catch(reason => {
        // Receives first rejection among the Promises
    })
```

#### Promise.all()工作实例

`Promise.all()`在工作中非常实用，经常与`map`一起使用，下面来看一个实例

```javascript
const query = {_pageNo: pageNo, _pageSize: pageSize}
 return api.default.get("/pgc/pingshen/quns", query).then(r => {
            const q = Promise.all(r.page.map(r2 => {
                const teacherIds = r2.setting.teachers || []
                r2.teacherName = []
                const teachStr = teacherIds.join("&ids=")
                const q2 = api.main.get("users/teachers?ids=" + teachStr)
                return q2.then(r3 => {
                    r3.forEach(info => {
                        r2.teacherName.push(info.truename || info.nickname || info.username)
                    })
                    r2.tName = r2.teacherName.join()
                    return r2
                })
            })).then(qs => {
                console.log(qs)
                r.page = qs
                return r
            })
            return q
        })
```

首先我解释一下上面这段代码，这是一个基于学校的项目，这里`qun`指学校里的班级（学校里肯定有很多班级，所以班级是以数组形式存放的），`teachers`指班级里的老师（一个班级肯定有好几个老师，所以也是以数组形式存放的），但是这里`teachers`这个字段中返回的不是教师的真实姓名，而是与姓名一一对应的`id`，**所以我们要做的就是把这些`id`通过接口转换成真实姓名**。

首先`api.default.get("/pgc/pingshen/quns", query)`是一个`Promise`，`.then（）`后的`r.page`就是班级的数组，接下来用`map`遍历数组，这里`r2`指的就是一个班级。接下来利用接口将班级里的老师由`id`转换成真实姓名。**注意：这里就要用到`Promise.all()`了，`map`遍历数组后会返回执行了回调函数的数组，`Promise.all()`将多个`map`遍历后的多个` Promise`实例当做一个`Promise`实例进行处理**。返回处理的结果（也是一个Promise），并通过链式调用，将返回的这个Promise重新赋予班级的数组，最后返回我们要的结果`q`。

通过上述这个例子说明了Promise在实际工作中的使用，**这里说一下我一开始是怎么处理这个转换的：我一开始没有使用`Promise.all`，也没有用`map`，用的是`forEach`，同样是遍历数组，`forEach`没有返回值，就导致了它不能产生Promise，这样页面的数据是加载不出来的，因为会先进行`forEach`的遍历循环再去处理Promise，因为Promise是处理异步加载的。**

### Promise.race()

`Promise.race()` 与 `Promise.all()` 类似，但只要数组中一个 Promise 落定（不管解决还是拒绝），该 Promise 的结果作为 `Promise.race()` 的结果。

注意如果数组为空，`Promise.race()` 永远不会落定（settled）。

下面两个例子是真实业务中如何去使用`Promise.all()`和`Promise.race()`，以及它们的对比和说明，可以让我们更加理解Promise。

**1.所有图片加载完在添加到页面**

```javascript
{
    function loadImg(src){
    
        return new Promise((resolve,reject)=>{
        
            let img = document.createElement('img');
            img.src = src;
            img.onload=function(){
                resolve(img);
            };
            
            img.onerror=function(err){
                reject(err);
            };
        });
    }
    
    function showImgs(imgs){
        imgs.forEach(function(img){
            document.body.appendChild(img);
        });
    }
    
    //Promise.all：把多个Proimse实例当做一个Promise实例；
    //all下面一个数组，数组传递进来多个Promise实例；当所有promise实例状态发生改变的时候；
    新的Promise实例才会跟着发生变化；
    （下面这三张图片的状态都完成之后，才会触发Promise.all这个新的Promise对象，所以这个
    Promise对象才会有then方法；
    Promise.all方法返回的是一个Promise实例，所以才可以调用then方法；
    --loadImg本身是一个Promise实例，这个promise实例是做一个图片加载的动作，把三个图片加载
      的动作放在Primose.all这个函数里面，生成一个新的promise实例，当三张图片都加载完之后，
      才会触发显示图片的逻辑；
    Promise.all([
        loadImg('http://i4.buimg.com/567571/dflef0720bea6832.png'),
        loadImg('http://i4.buimg.com/567571/2b07ee25b08930ba.png'),
        loadImg('http://i2.muimg.com/567751/5eb8190d6b2a1c9c.png')
    
    ]).then(showImgs);
}
```

**三张图片位于三个不同的位置，页面需要加载一张图片，三张图片不知道哪张返回的比较快；**

**2.三张图片有三个来源，加载出来一个就可以；(先到先得)**

```javascript
{
    //有一个图片加载完就添加到页面上；
    function loadImg(src){
    
        return new Promise((resolve,reject)=>{
        
            let img = document.createElement('img');
            img.src = src;
            img.onload=function(){
                resolve(img);
            };
            
            img.onerror=function(err){
                reject(err);
            };
        });
    }

    function showImgs(img){
        let p = document.createElement('p');
        p.appendChild(img);
        document.body.appendChild(p);
    }
    
    //多个实例中，有一个状态先改变，最后这个race实例也会跟着改变，其他的就不在响应了；
     Promise.race([
        loadImg('http://i4.buimg.com/567571/dflef0720bea6832.png'),
        loadImg('http://i4.buimg.com/567571/2b07ee25b08930ba.png'),
        loadImg('http://i2.muimg.com/567751/5eb8190d6b2a1c9c.png')
    
    ]).then(showImgs);
    //只显示了一个；
}
```

## Promise的常见错误

### 丢失 `then` 的结果

先看错误的代码：

```javascript
function foo() {
    const promise = asyncFunc()
    promise.then(result => {
        ···
    })

    return promise
}
```

再看正确的代码：

```javascript
function foo() {
    const promise = asyncFunc()
    return promise.then(result => {
        ···
    })
}
```

甚至再简化为：

```javascript
function foo() {
    return asyncFunc()
        .then(result => {
            ···
        })
}
```

不要忘了 `promise.then（）` 也会产生一个结果，甚至可能抛出异常，或返回一个异步结果（一个新的 Promise）。

### 捕获全部异常

前面提过可以通过链式调用，最后统一处理异常，这么做不但省事，而且可以避免遗漏错误：

```javascript
asyncFunc1()
    .then(
        value => { // (A)
            doSomething() // (B)
            return asyncFunc2() // (C)
        },
        error => { // (D)
            ···
        })
```

上面的代码，D 处的错误处理只能处理 asyncFunc1() 的错误，但无法处理 B 处抛出的移除和 C 处返回的拒绝的 Promise。正确的写法：

```javascript
asyncFunc1()
    .then(value => {
        doSomething()
        return asyncFunc2()
    })
    .catch(error => {
        ···
    })
```

**这个例子可以回过头看看上面所写的检测用户名密码的例子，看完之后能更深的理解**

### 忽视非异步代码的错误

有时我们编写的异步方法中 —— 这里的异步方法指返回 Promise 的方法 —— 在异步前存在部分非异步的代码。如果这些代码抛出异常，异常将直接从方法抛出，而不是进入返回 Promise 并拒绝，例如下面代码的 A 处：

```javascript
function asyncFunc() {
    doSomethingSync() // (A)
    return doSomethingAsync()
        .then(result => {
            ···
        })
}
```

解决方法一，捕获并显式拒绝：

```javascript
function asyncFunc() {
    try {
        doSomethingSync()
        return doSomethingAsync()
            .then(result => {
                ···
            })
    } catch (err) {
        return Promise.reject(err)
    }
}
```

解决方法二，通过 `Promise.resolve().then()` 开始一段 Promise 链，将同步代码包裹进 `then` 的方法中：

```javascript
function asyncFunc() {
    return Promise.resolve().then(() => {
        doSomethingSync()
        return doSomethingAsync()
    }).then(result => {
        ···
    })
}
```

方法三：

```javascript
function asyncFunc() {
    return new Promise((resolve, reject) => {
        doSomethingSync()
        resolve(doSomethingAsync())
    })
    .then(result => {
        ···
    })
}
```

## finally

有时不管成功还是失败都要执行一些方法，例如确认对话框，不管确定还是取消都要关闭对话框。Promise 原生 API 没有提供 finally 的功能，但我们可以模拟：

```javascript
Promise.prototype.finally = function (callback) {
    const P = this.constructor
    // We don’t invoke the callback in here,
    // because we want then() to handle its exceptions
    return this.then(
        // Callback fulfills => continue with receiver’s fulfillment or rejection
        // Callback rejects => pass on that rejection (then() has no 2nd parameter!)
        value  => P.resolve(callback()).then(() => value),
        reason => P.resolve(callback()).then(() => { throw reason })
    )
}
```

调用：

```javascript
createResource(···)
.then(function (value1) {
    // Use resource
})
.then(function (value2) {
    // Use resource
})
.finally(function () {
    // Clean up
})
```

## Promise 库

原生 Promise 库够用但不够强大，对于更复杂的功能，可以使用某个第三方 Promise 库。比如 [bluebird](http://bluebirdjs.com/docs/api-reference.html)。