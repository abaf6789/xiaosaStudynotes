# 函数promise化

```javascript
const isObject = obj => Object.prototype.toString.call(obj) === '[object Object]';

const promisfy = func =>
  (...args) => new Promise((resolve, reject) => {
    const defaultOption = {
      success: resolve,
      fail: reject,
    };
    isObject(args[args.length - 1]) ?
      Object.assign(args[args.length - 1], defaultOption) :
      args.push(defaultOption);
    func(...args);
  });

export default promisfy;
```

