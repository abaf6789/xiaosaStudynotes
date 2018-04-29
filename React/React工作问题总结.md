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

## 4.父子组件的通信问题

#### 1.子组件内部如何接收到父组件`state`中的值，接收后如何修改父组件中的`state`？

##### 父组件

```javascript
this.state={
    intro: "aaa"
}

class Parent extends Component {
    revise(intro){
        this.setState({intro:intro})
    }
    render() {
        return (
            <Child intro={this.state.intro} />
        )
    }
}
```

##### 子组件

```javascript
class Child extends Component {
    receiveProps(){
        	let intro = this.props.intro
            intro = "bbb"
        	this.props.revise(intro)
        }
    render() {
        return (
            <Ceshi/>
        )
      }
    }
```

除了父组件中`state`的状态可以传递，父组件中的方法也是可以通过`props`来传递的，通过传入的方法，将修改后的参数传入函数，在父组件中更新`state`就可以完成**子组件修改父组件中的`state`了**。