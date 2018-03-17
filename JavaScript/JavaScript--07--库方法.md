# 库方法

## 数字与数学

```javascript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2
```

## 字符串

```javascript
"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"
```

## 对象

```javascript
Object.assign(Point, { origin: new Point(0,0) })
```

