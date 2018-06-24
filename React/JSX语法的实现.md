# JSX语法的实现

JSX语法的实现离不开`Babel`，它可以将你编写的`ES6/ES7`语法转换成浏览器可识别的JavaScript。

> 先来看一个简单的例子

你编写的代码：

```javascript
let foo = <div id="foo">Hello!</div>;
```

实际运行的代码：

```javascript
var foo = h('div', {id:"foo"}, 'Hello!'); 
```

**JSX实际上就是一种非常得体的语法糖**

## 开始编写一个JSX

上面的例子中用到了`h`这个标识符来作为函数名，来看一下函数的实现

```javascript
function h(nodeName, attributes, ...args) {
    let children = args.length ? [].concat(...args) : null
    return { nodeName, attributes, children }
}
```

上面这个函数假如有返回值，应该会返回下面这种形式：

```javascript
{
  nodeName: "div",
  attributes: {
    "id": "foo"
  },
  children: ["Hello!"]
}
```

这里返回的只是`JSON`对象，我们需要将它转换成真实的`DOM`节点

```javascript
function render(vnode) {
    if(vnode.split) return document.createTextNode(vnode)
    let n = document.createElement(vnode.nodeName)
    let a = vnode.attributes || {}
    Object.keys(a).forEach( k=>n.setAttribute(k,a[k]) )
    (vnode.children || []).forEach(c=>n.appendChild(render(c)))
    return n
}
```

## 使用JSX语法

```javascript
//JSX转换成虚拟DOM
let vdom = <div id="foo">Hello!</div>
//VDOM转换成DOM
let dom = render(vdom)
//将dom加入body
document.body.appendChild(dom)
```

