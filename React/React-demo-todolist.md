# React-demo-todolist

react的基础demo todolist

## 1.构建项目

使用`create-react-app`创建项目，首先全局安装

```
npm install -g create-react-app
```

创建项目及运行

```
create-react-app todolist
cd todolist
npm start
```

运行完访问localhost：3000在浏览器查看

## 2.项目搭建

首先是根目录下有`App.js`，删除里面的所有内容，添加如下代码

```javascript
import React, { Component } from 'react';
import ToDoBox from './containers/Todo'
import './App.css';

class App extends Component {
  render() {
    return (
        <ToDoBox />
    );
  }
}

export default App;
```

这里的`ToDoBox`就是主文件

下面看一下文件路径结构

```javascript
src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── component
    ├── containers
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── registerServiceWorker.js
```

`src`文件下`component`文件夹存放组件，`containers`存放的就是`ToDoBox`这个`todolist`的主干，接下来看一下`src`下的`index.js`

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

`App`本身也是一个组件，最后是通过`index.js`渲染到网页上，所有的样式可以写到`index.css`中。最后一行的`registerServiceWorker()`就是为`react`项目注册了一个`service worker`，用来做资源的缓存，这样你下次访问时，就可以更快的获取资源。而且因为资源被缓存，所以即使在离线的情况下也可以访问应用（此时使用的资源是之前缓存的资源）。注意，`registerServiceWorker`注册的`service worker `只在生产环境中生效`（process.env.NODE_ENV === 'production'）`

## 3.编写代码

### containers

先看`./containers/Todo`这里的`index.jsx`

```javascript
import React,{Component} from 'react';
import Input from "../../component/Input"
import List  from "../../component/List"
```

开头，先引用两个组件，这样就可以通过`<Input />`和`<List />`引用

```javascript
export default class ToDoBox extends Component {
    constructor(){
        //...
    }
    render(){
        return(
        	<div></div>
        )
    }
    function(){
        //...
    }
    fn2() {
        //....
    }
}
```

接下来搭一个react的模版框架，先来看constructor部分

```javascript
constructor(props,context){
    super(props,context)
    this.state = {
      data:[]
    }
  }
```

基本算是一种固定用法，data里存放数据，然后是render部分

```javascript
render() {
    return (
      <div className="todo-list">
        <header className="todo-head">todoList</header>
        <Input submitFn={this.submitFn.bind(this)}/>
        <List data={this.state.data} deleteFn={this.deleteFn.bind(this)}/>
      </div>
    )
  }
```

`render`部分和`html`很像，要注意的地方是必须要有`return`，相当于`return`出模版，另外`return`出来的部分不能是零散的元素，最后必须包裹在一个大的父节点中（这里是`todo-list`这个`div`）。**注意**：这里两个函数都用了ES5的`bind`，因为在下面的函数定义中`this`的指向可能会发生变化，所以这边绑定`this`。

接下来看下`render`中定义的两个函数，首先是`submitFn`

```javascript
submitFn(value) {
      const id = this.state.data.length
      this.setState({
          data: this.state.data.concat({
              id: id,
              text: value
          })
      })
  }
```

这里采用了Es6的语法，不用`function`，就这样定义函数。`this.setState()`中用了`concat`

来合并这些数据到`data`中成为一个数组。接下来看`deleteFn`

```javascript
deleteFn(id) {
      let data = this.state.data
      this.setState({
          data: data.filter(item => {
              if(item.id !== id) {
                return item
              }
          })
      })
  }
```

这里通过`filter`这个方法巧妙得删除了数组`data`里的元素,`filter`会形成一个新数组，新数组中的元素必须通过在`filter`中定义的函数的条件，这里的条件是`item.id !== id`，这是不可能的，所以实现了动态移除`data`数组中的数据的操作

### component

先是`Input`组件

```javascript
constructor(props,context) {
        super(props,context)
        this.state = {
            value: ''
        }
    }
```

`constructor`中`this.state`先定义`value: ''`表示开始时，input输入框的内容为空。

```javascript
render() {
        const s = {width: '100%',height:'40px',fontSize:'30px'}
        return (
            <div>
                <input
                    style={s}
                    value={this.state.value}
                    onChange={this.changeHandler.bind(this)}
                    onKeyUp={this.keyUpHandler.bind(this)}
                 />
            </div>
        )
    }
```

这里给`input`的`value`赋值`this.state.value`，就是上面定义过的空。同时定义一个类似监听内容变化的函数`changeHandler`和监听按键`enter`的函数`keyUpHandler`，同时记得绑定`this`

```javascript
changeHandler(e) {
      //实时同步数据
        this.setState({value: e.target.value})
    }
```

当`input`的内容发生变化，触发`onChange`函数，然后触发`changeHandler`，然后触发`setState`就可以保持实时的`value`监听。

```javascript
keyUpHandler(e) {
        const value = this.state.value
        if(e.keyCode === 13 && value.trim()) {
            //这里是this.props.submitFn表示从todo里取到到submit函数
            this.props.submitFn(value)
            this.setState({value: ''})
        }
    }
```

这里和上面一样，通过触发`keyUp`事件触发`this.props.submitFn`，这里的`this.props.submitFn`表示从`todo`里取到到`submit`函数。每次`enter`最后记得要把`input`这个组件的`value`清零。`value.trim()`判断内容是否为空。这里还要注意调用`submitFn`函数时要把`value`传入，因为`submitFn`中定义的`text`中的`value`就是通过这里传递的

总结一下，子组件想取到父组件或者父级的方法时，通过`this.props.fn`来实现。



再看`List`

没有需要存储并且与父组件相关联的内容，所以不需要写`constructor`。

```javascript
render() {
    const data = this.props.data
    const sty2 = {marginTop: '10px',fontSize: '20px', 		 lineHeight:'30px',listStyle:'none'}
    return(
        <ul style={sty2}>
            {data.map((item, index) => {
                return <li key={index} onClick={this.clickFn.bind(this,item.id)}>{item.text}</li>
            })}
        </ul>
    )
  }
```

这里和上面基本一样，唯一的区别就是用了`map`，在**React**中经常会用到`map`，作用是遍历数组，传递值，渲染界面，因为**React**是数据驱动的，当用map遍历数组时，记得每个标签要绑定一个`key`，这里用的是`key={index}`。

`Keys`可以在`DOM`中的某些元素被增加或删除的时候帮助`React`识别哪些元素发生了变化。一个元素的`key`最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据的`id`作为元素的`key`。

一般不建议用`index`作为`key`值，因为这样会导致渲染变得很慢。

## 总结

这个`todolist`开始是没有数据的，通过子组件`input`的`onChange`事件监听`value`的改变，`onKeyUp`事件触发的`submitFn`将数据渲染进`data`中，`data`中数据的改变，触发了List组件，导致了页面的重新渲染，`list`的点击事件也同时可以触发了