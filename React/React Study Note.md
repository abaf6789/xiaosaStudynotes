# React 学习记录

摘要：简单记录了react官网上的的基础知识和基本用法，下面一些代码用到了ES6语法

## React简介

React的组件相当于MVC里面的View。React将页面分成了各个独立的小块，每一个块就是一个组件，这些组件可以组合、嵌套，构成我们的页面

一个组件的显示形态和行为是由数据决定的，而数据是可能发生改变的，这时候组件的显示状态就会发生改变，React提供了一种非常高效率的方式让数据（data）和显示（view层）实现同步。



## hello world

```javascript
import React from 'react'
import { render } from 'react-dom'

// 定义一个Hello组件
class Hello extends React.Compoent {
    render() {
        return (
        	<p> hello world </p>
        )
    }
}

//渲染组件
render(
	<Hello />
    document.getElementById('root')
)
//定义一个组件的时候必须大写字母开头
```



## jsx语法

jsx是React里的一种特殊语法，与HTML有点点相似。



### 必须用一个父节点包裹所有子节点

jsx中不能返回多个零散的节点，比如如果有好几个a标签，必须包裹在一个div里才能返回。



### 注释

jsx中的注释用`{/*    */}`来表示



### 样式

`class`在jsx中用`className`来表示

内联样式：`<div style={{color: 'red',fontSize: '30px'}}>abc</div>`，注意`fontSize`的驼峰写法，还有样式用的双大括号，可以`var sty = {color: 'red',fontSize: '30px'}`，`style={sty}`。



### 事件

例如click事件，要在标签上绑定click

```javascript
class Hello extends Component {
    render() {
		return (
        	<div onClick={this.clickFn.bind(this)}>hello world</div>
        )
    }
    
    clickFn(e) {
        e.preventDefault()
        console.log(2)
    }
}
```

这里注意`bind`，这是ES5的语法，用来绑定this，实际项目中写函数后this指向可能会发生改变，所以要绑定`this`，或者采用ES6的箭头函数，或者`var that = this`，这样就可以防止this指向错误。

这里还要注意onClick的驼峰式写法。



### 循坏

在jsx中使用循环，会用到`Array.prototype.map`

```javascript
class Hello extends Component {
    render() {
		const arr = ['a','b','c']
        return (
            <div>
                {arr.map((item,index) => {
                    <div key={index}>{item}</div>
                })}
    		</div>
        )
    }
}
```

这里要注意在用map的时候给标签设置一个`key`值，不然浏览器会报一个警告，设置key值标示每一个节点都是独一无二的，这里key值设置为`index`，但是一般key值不建议这样设置，一般绑定的比如说`data.id`，类似这种。此外，`arr.map`必须包裹在{}



### 判断

jsx中的判断一般用三元表达式，表达式也是放在{}中的



## 组件化

一般用react写项目，要学会把项目拆成一个个很小的组件



## 数据传递

### props

**React中，`props`一般只作为父组件给子组件传递数据用，不要试图去修改自己的porps！**

### state

如果要改变组件内部自身的属性，用`state`。

**React会实时监听每个组件的props和state的值，一旦有变化，会立刻更新组件，页面就将重新渲染**

## 智能组件 & 木偶组件 

在用React做开发的时候，所有的单位都称为组件，但是这些组件也是需要区分的，区分他们并且分别放在`containers`和`components`。

#### 智能组件    

智能组件在日常开发中一般是放在`components`中，在智能组件里会放一些数据以及对数据处理的函数，然后把数据传递给具体实现的组件即可。举个例子，比如说一个`input`，就可以是一个智能组件，可以在`input`里定义各种各样的函数来操纵它在不同界面的不同效果，这个例子说明了智能组件为何是"智能"的。

#### 木偶组件

木偶组件这里的"木偶"就是说它总是被线牵着，它无法改变，木偶组件可以说是智能组件的一种组合，但是数据，函数等已经在智能组件内确定了，它要做的就只是把智能组件的组合呈现出的效果展现给用户。

## 生命周期

#### 常用的几个生命周期函数(hook)

1. `getInitialState`

初始化组件state数据，在es6的语法中，可以用下面的书写方式代替

```jsx
class Hello extends React.Component {
    constructor(props, context) {
        super(props, context);
        // 初始化组件 state 数据
        this.state = {
            now: Date.now()
        }
    }
}
```

2. `render`

最常用的，返回组件要渲染的模版，类似于`vue`中的`template`。

3. `comopentDidMount`

组件第一次加载时渲染完成的事件，一般在此获取网络数据。

4. `shouldComponentUpdate`

主要用于性能优化。

5. `componentDidUpdate`

组件更新后触发的事件，一般用于清空并更新数据。

6. `componentWillUnmount`

组件在销毁之前触发的事件，一般用户存储一些特殊信息，以及清理`setTimeout`事件等。