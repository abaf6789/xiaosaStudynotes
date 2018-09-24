# 微信小程序API封装

## 传入URL与参数，转换成完整URL

```javascript
export const generatorUrl = (url = '', params = {}) => {
  const queryString = Object.keys(params).map(key => `${key}=${params[key]}`).join('&');
  return `${url}?${queryString}`;
};
```

## 微信跳转

```javascript
import promisify from 'promisify'
import { generatorUrl } from './network'

export default {
  navigateTo: (url, params) => promisify(wx.navigateTo)({ url: generatorUrl(url, params) }),
  redirectTo: (url, params) => promisify(wx.redirectTo)({ url: generatorUrl(url, params) }),
  reLaunch: (url, params) => promisify(wx.reLaunch)({ url: generatorUrl(url, params) }),
  switchTab: (url, params) => promisify(wx.switchTab)({ url: generatorUrl(url, params) }),
}
```

## 微信Toast

```javascript
const isError = obj => Object.prototype.toString.call(obj) === '[object Error]'

import promisify from 'promisify';

const DEFAULT_MESSAGE = '未知错误，请重试';

export const Toast = (message, icon = 'none') =>
  promisify(wx.showToast)({ title: message, icon });

export const ToastError = (error = DEFAULT_MESSAGE) => {
  console.error(error);
  return Toast(isError(error) ? error.message : error);
}
```

