[TOC]

# React工作问题收集总结



## 1.如何修改state中一个对象的某个属性

假设目前我们有这样一个需求：要修改`introduction`的`field`

```react
this.state = {
    introduction: {
        class: '',
        field: '',
        teacher: ''
    }
}
```

这里要使用`Object.assign()`，要注意**不要返回原对象，应该返回一个新对象**：

```react
this.setState(Object.assign({},this.state.introduction,{field:"English"}))
```

## 2.this.setState是异步的，如何在调用后获取最新数据？

- 回调函数

```react
this.setState({
    introduce: '111'
},() => {
    console.log(this.state.introduce)
})
```

- componentDidUpdate

```React
componentDidUpdate(){
    console.log(this.state.introduce)
}
```

在`componentDidUpdate`中调用的`state`是最新状态。

## 3.深拷贝在工作中的使用

第一种情况：**修改state中一个对象的某个属性**，这里用到深拷贝。

第二种情况：当我们想将`this.state.object`当成一个参数传递。

```react
function handleIntroduction(introduction){
    //假如这里的操作会对introduction产生改变
}
const introduction = this.state.introduction
handleIntroduction(introduction)
```

上面的例子当我们把`introduction`传入，在`introduction`改变的同时，`state`中的`introduction`也发生了改变，从而会影响页面的渲染，所以这里要使用深拷贝

```react
function handleIntroduction(introduction){
    //假如这里的操作会对introduction产生改变
}
const introduction = Object.assign({}, this.state.introduction)
handleIntroduction(introduction)
```

