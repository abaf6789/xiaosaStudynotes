# web直传

http://oss-demo.aliyuncs.com/oss-h5-upload-js-php/index.html?spm=a2c4g.11186623.2.4.eHPZYS

上面是一个阿里云web直传的例子

## 流程

这个流程我使用的是`axios`

### 获取信息

```javascript
export const getInform = () => {
    return AxiosRequest.get(`oss/policy`);
}
```

### append信息

```javascript
export const directUpload = (attachment, onUploadProgress) => {
    let data = new FormData()
    const file = attachment.file
    data.append('name', file.name)
    data.append('policy', file.policy)
    data.append('signature', file.signature)
    data.append('success_action_status', '201')
    data.append('OSSAccessKeyId', file.OSSAccessKeyId)
    data.append('key', file.key)
    data.append('file', file)
    if (!onUploadProgress) onUploadProgress = () => { }
    return AxiosAliRequest.post(``, data, {
        headers: { 'content-type': 'multipart/form-data; boundary={boundary}' },
        onUploadProgress
    })
}
```

这里是把第一步获取的信息都`append`到`data`中。

**这里需要注意的是`file`必须在最后一个`append`，**`success_action_status`状态码为`201`时，`Promsie`会返回一个`xml`，设置为`200`时返回的`data`为空。

如果想获取进度信息，可以用`axios`中封装的`onUploadProgress`方法。