[TOC]



# 48个常用JavaScript算法

## 排列组合

**Anagrams of string（带有重复项）**

```javascript
const anagrams = srt => {
    if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str]
    return str.split('').reduce((acc, letter, i) =>
        acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), 		[])
}
// anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']
```

使用递归。对于给定字符串中的每个字母，为字母创建字谜。使用`map()`将字母与每部分字谜组合，然后使用`reduce()`将所有字谜组合到一个数组中，最基本情况是字符串长度等于2或1。

## 数组平均数

```javascript
const average = arr => arr.reduce((acc, val) => acc + val, 0) / arr.length
// average([1,2,3]) -> 2
```

使用`reduce()`将每个值添加到累加器，初始值为0，总和除以数组的长度。

## 大写每个单词的首字母

```javascript
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase())
// capitalizeEveryWord('hello world!') -> 'Hello World!'
```

使用`replace()`匹配每个单词的第一个字符，并使用`toUpperCase()`来将其大写。

## 首字母大写

```javascript
const capitalize = (str, lowerRest = false) =>
str.slice(0, 1).toUpperCase() + (lowerRest ? str.slice(1).toLowerCase() : str.slice(1))
```

设置了一个`lowerRest`，`lowerRest`为`true`时首字母大写，其余部分有大写的地方变小写，为`false`时其余部分保持原样。

## 检查回文

```javascript
const palindrome = str => {
    const s = str.toLowerCase().replace(/[\W_]/g, '')
    return s === s.split('').reverse().join('')
}
    // palindrome('taco cat') -> true
```

将字符串转换为`toLowerCase()`，并使用`replace()`从中删除非字母的字符。然后，将其转换为`tolowerCase()`，将`()`拆分为单独字符，`reverse()`，`join('')`，与原始的非反转字符串进行比较，然后将其转换为`tolowerCase()`。

## 计数数组中值的出现次数

```javascript
const countOccurrences = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0)
// countOccurrences([1,1,2,1,2,3], 1) -> 3
```

返回一个数组中某个值出现的次数

## 当前URL

```javascript
const currentUrl = _ => window.location.href
// currentUrl() -> www.xxx.com
```

当前页面的url

## Curry

```javascript
const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length
    ? fn(...args)
    : curry.bind(null, fn, arity, ...args);
// curry(Math.pow)(2)(10) -> 1024
// curry(Math.min, 3)(10)(50)(2) -> 2
```

使用递归。如果提供的参数（args）数量足够，则调用传递函数f，否则返回一个curried函数f。

## Deep flatten array

```javascript
const deepFlatten = arr =>
    arr.reduce((a, v) => a.concat(Array.isArray(v) ? deepFlatten(v) : v), [])
// deepFlatten([1,[2],[[3],4],5]) -> [1,2,3,4,5]
```

使用递归，使用`reduce()`来获取所有不是数组的元素，flatten每个元素都是数组。

## 补集

```javascript
const difference = (a, b) => { const s = new Set(b); return a.filter(x => !s.has(x)) }
// difference([1,2,3], [1,2]) -> [3]
```

## 两点之间的距离

```javascript
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0)
// distance(1,1, 2,3) -> 2.23606797749979
```

使用Math.hypot()计算两点之间的距离。

## 是否可以整除

```javascript
const isDivisible = (dividend, divisor) => dividend % divisor === 0
// isDivisible(6,3) -> true
```

## 转义正则表达式

```javascript
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')
// escapeRegExp('(test)') -> \\(test\\)
```

用`replace()`来转义特殊字符

## 偶数或奇数

```javascript
const isEven = num => num % 2 === 0
// isEven(3) -> false
```

奇数返回`false`，偶数返回`true`。

## 阶乘

```javascript
const factorial = n => n <= 1 ? 1 : n * factorial(n - 1)
// factorial(6) -> 720
```

使用递归。如果n小于或等于1，则返回1。否则返回n和n - 1的阶乘的乘积。

## 斐波那契数组生成器

```javascript
const fibonacci = n =>
  Array(n).fill(0).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), [])
// fibonacci(5) -> [0,1,1,2,3]
```

创建一个特定长度的空数组，初始化前两个值（0和1）。使用Array.reduce（）向数组中添加值，后面的一个数等于前面两个数相加之和（前两个除外）。

## 过滤数组里所有的非唯一值

```javascript
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i))
// filterNonUnique([1,2,2,3,4,4,5]) -> [1,3,5]
```

将`Array.filter()`用于仅包含唯一值的数组。

## Flatten数组

```javascript
const flatten = arr => arr.reduce((a, v) => a.concat(v), [])
// flatten([1,[2],3,4]) -> [1,2,3,4]
```

使用`reduce()`来获取数组中的所有元素，并使用`concat()`来使它们`flatten`。

## 从数组中获取最大值

```javascript
const arrayMax = arr => Math.max(...arr)
// arrayMax([10, 1, 5]) -> 10
```

使用`Math.max()`与`spread`运算符结合得到数组中的最大值。

## 从数组中获得最小值

```javascript
const arrayMin = arr => Math.min(...arr)
// arrayMin([10, 1, 5]) -> 1
```

使用`Math.max()`与`spread`运算符结合得到数组中的最小值。

## 获取滚动位置

```javascript
const getScrollPos = (el = window) =>
  ({x: (el.pageXOffset !== undefined) ? el.pageXOffset : el.scrollLeft,
    y: (el.pageYOffset !== undefined) ? el.pageYOffset : el.scrollTop})
 // getScrollPos() -> {x: 0, y: 200}
```

如果已定义，请使用`pageXOffset`和`pageYOffset`，否则使用`scrollLeft`和`scrollTop`，可以省略el来使用window的默认值。

## 最大公约数（GCD）

```javascript
const gcd = (x, y) => !y ? x : gcd(y, x % y)
// gcd (8, 36) -> 4
```

使用递归。基本情况是当y等于0时。在这种情况下，返回x。否则，返回y的GCD和x / y的其余部分。

## Head of list

```javascript
const head = arr => arr[0]
// head([1,2,3]) -> 1
```

返回数组第一个元素。

## list初始化

```javascript
const initial = arr => arr.slice(0, -1)
// initial([1,2,3]) -> [1,2]
```

去掉数组最后一个元素并返回数组

## 用range初始化数组

```javascript
const initializeArrayRange = (end, start = 0) =>
  Array.apply(null, Array(end - start)).map((v, i) => i + start)
// initializeArrayRange(5) -> [0,1,2,3,4]
```

使用Array（end-start）创建所需长度的数组，使用map（）来填充范围中的所需值，可以省略start使用默认值0。

## 用值初始化数组

```javascript
const initializeArray = (n, value = 0) => Array(n).fill(value)
// initializeArray(5, 2) -> [2,2,2,2,2]
```

## 返回数组的最后一个元素

```javascript
const last = arr => arr.slice(-1)[0]
// last([1,2,3]) -> 3
```

## 测试功能所花费的时间

```javascript
const timeTaken = callback => {
  console.time('timeTaken')
  const r = callback()
  console.timeEnd('timeTaken')
  return r
}
// timeTaken(() => Math.pow(2, 10)) -> 1024
// (logged): timeTaken: 0.02099609375ms
```

使用`performance.now()`获取函数的开始和结束时间，`console.log()`所花费的时间。第一个参数是函数名，随后的参数传递给函数。

## 键值对的对象

```javascript
const objectFromPairs = arr => arr.reduce((a, v) => (a[v[0]] = v[1], a), {})
// objectFromPairs([['a',1],['b',2]]) -> {a: 1, b: 2}
```

使用Array.reduce（）来创建和组合键值对。

## 管道

```javascript
const pipe = (...funcs) => arg => funcs.reduce((acc, func) => func(acc), arg)
// pipe(btoa, x => x.toUpperCase())("Test") -> "VGVZDA=="
```

## Powerset

```javascript
const powerset = arr =>
  arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [[]])
// powerset([1,2]) -> [[], [1], [2], [2,1]]
```

找到选定数组的子集，除了自己

## 范围内的随机整数

```javascript
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
// randomIntegerInRange(0, 5) -> 2
```

使用Math.random（）生成一个随机数并将其映射到所需的范围，使用Math.floor（）使其成为一个整数。

## 范围内的随机数

```javascript
const randomInRange = (min, max) => Math.random() * (max - min) + min
// randomInRange(2,10) -> 6.0211363285087005
```

使用Math.random（）生成一个随机值，使用乘法将其映射到所需的范围。

## 随机化数组的顺序

```javascript
const shuffle = arr => arr.sort(() => Math.random() - 0.5)
// shuffle([1,2,3]) -> [2,3,1]
```

使用sort（）重新排序元素，利用Math.random（）来随机排序。

## 重定向URL

```javascript
const redirect = (url, asLink = true) =>
  asLink ? window.location.href = url : window.location.replace(url)
// redirect('https://google.com')
```

使用window.location.href或window.location.replace（）重定向到url。 传递第二个参数来模拟链接点击（true - default）或HTTP重定向（false）。

## 反转一个字符串

```javascript
const reverseString = str => [...str].reverse().join('')
// reverseString('foobar') -> 'raboof'
```

## RGB到十六进制

```javascript
const rgbToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0')
// rgbToHex(255, 165, 1) -> 'ffa501'
```

使用按位左移运算符（<<）和toString（16），然后padStart（6，“0”）将给定的RGB参数转换为十六进制字符串以获得6位十六进制值。

## 滚动到顶部

```javascript
const scrollToTop = _ => {
  const c = document.documentElement.scrollTop || document.body.scrollTop
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop)
    window.scrollTo(0, c - c / 8)
  }
}
// scrollToTop()
```

使用document.documentElement.scrollTop或document.body.scrollTop获取到顶部的距离。

从顶部滚动一小部分距离。

使用window.requestAnimationFrame（）来滚动。

## 随机数组值

```javascript
const shuffle = arr => {
    let r = arr.map(Math.random)
    return arr.sort((a,b) => r[a]-r[b])
}
//shuffle([1,2,3]) -> [2, 1, 3]
```

使用Array.map（）和Math.random（）创建一个随机值的数组。使用Array.sort（）根据随机值对原始数组的元素进行排序。

## 数组之间的相似性

```javascript
const similarity = (arr, values) => arr.filter(v => values.includes(v))
// similarity([1,2,3], [1,2,4]) -> [1,2]
```

使用filter（）移除不是values的一部分值，使用includes（）确定。

## 按字符串排序（按字母顺序排列）

```javascript
const sortCharactersInString = str =>
  str.split('').sort((a, b) => a.localeCompare(b)).join('')
// sortCharactersInString('cabbage') -> 'aabbceg'
```

使用split（''）分割字符串，sort（）使用localeCompare（），使用join（''）重新组合。

## 数组总和

```javascript
const sum = arr => arr.reduce((acc, val) => acc + val, 0)
// sum([1,2,3,4]) -> 10
```

使用reduce（）将每个值添加到累加器，初始化值为0。

## 交换数组中两个变量的值

```javascript
[varA, varB] = [varB, varA]
// [x, y] = [y, x]
```

ES6新特性解构赋值

## 列表的tail

```javascript
const tail = arr => arr.length > 1 ? arr.slice(1) : arr
// tail([1,2,3]) -> [2,3]
// tail([1]) -> [1]
```

返回arr.slice（1）

## 数组唯一值

```javascript
const unique = arr => [...new Set(arr)]
// unique([1,2,2,3,4,4,5]) -> [1,2,3,4,5]
```

使用ES6 Set和…rest操作符实现数组去重

## URL参数

```javascript
const getUrlParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  )
// getUrlParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}
```

使用match() 与适当的正则表达式来获得所有键值对，适当的map() 。使用Object.assign（）和spread运算符（...）将所有键值对组合到一个对象中，将location.search作为参数传递给当前url。

## UUID生成器

```javascript
const uuid = _ =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
  )
// uuid() -> '7982fcfe-5721-4632-bede-6000885be57d'
```

使用crypto API生成符合RFC4122版本4的UUID。

## 验证是否是number类型

```javascript
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n
// validateNumber('10') -> true
```

使用`!isNaN`和`parseFloat()`来检查参数是否是一个数字，使用`isFinite()`来检查数字是否是有限的。