# JavaScript常用方法集合

## 获取URL中的参数

```javascript
function getUrlParam(param, url) {
   if (!url) url = window.location.href;
   let regex = new RegExp(regexStr(param)),
       results = regex.exec(url);
   if (!results) return null;
   if (!results[2]) return '';
   return decodeURIComponent(results[2].replace(/\+/g, " "));
}
function regexStr(param) {
   param = param.replace(/[\[\]]/g, "\\$&");
   return "[?&#]" + param + "(=([^&#]*)|&|#|$)";
}
```

