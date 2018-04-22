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