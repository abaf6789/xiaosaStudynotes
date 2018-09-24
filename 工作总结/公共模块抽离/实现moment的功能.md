# 实现moment的功能

返回年月日时分秒

```javascript
const pad0 = n => {
  const str = n.toString();
  return str[1] ? str : `0${str}`;
};

export const formatTime = date => {
  const year = date.getFullYear();
  const month = date.getMonth() + 1;
  const day = date.getDate();

  const hour = date.getHours();
  const minute = date.getMinutes();
  const second = date.getSeconds();

  const t1 = [year, month, day].map(pad0).join('/');
  const t2 = [hour, minute, second].map(pad0).join(':');

  return `${t1} ${t2}`;
};

export default {
  pad0,
  formatTime,
};
```

