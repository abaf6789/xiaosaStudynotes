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

