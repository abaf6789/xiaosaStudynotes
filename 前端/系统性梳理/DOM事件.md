# DOM事件

## DOM事件的级别

DOM0：`element.onclick=function(){...}`

DOM2：`element.addEventlistener('click', function(){}, false)`

DOM3：`element.addEventlistener('click', function(){}, false)`

DOM2与DOM3中的true，false指定冒泡还是捕获

DOM2与DOM3的区别：DOM3事件类型增加了很多。

## DOM事件模型（冒泡捕获）

捕获（从上往下）

冒泡（从目标元素往上）

## DOM事件流（页面中接受事件的顺序）

浏览器在为当前页面与用户做交互的过程中，比如点击了鼠标左键，这个左键是怎么传到你的页面上的，又怎么相应的。

1. 捕获阶段
2. 目标阶段    事件通过捕获到达目标元素
3. 冒泡阶段    从目标元素上传到window对象

## 描述DOM事件捕获的具体流程

- 第一个接受事件的对象是window
- 如何取得body标签：document.body
- 表示html节点：document.documentElement

捕获流程：**window**->**document**->**html标签**->**body**->**父级元素**->**子级元素**->**目标元素**

冒泡流程：从目标元素一层一层最后到window完成一次冒泡的流程

## Event对象的常见应用

- `Event.preventDefault() `   **阻止默认行为**

  a标签绑定了click事件，在响应函数中设置了preventDefault，就阻止了链接默认跳转行为。

- `Event.stopPropagation() `     **阻止冒泡**

  父级元素、子级元素都绑定有事件，单击子元素做一件事，单击父元素再做一件事，如果不阻止冒泡，单击子元素的范围的时候，根据冒泡的原理，父级元素也会被响应。

- `Event.stopImmediatePropagation()`    **事件响应优先级**

  一个按钮绑定了两个click事件1和2，想通过优先级的方式，第一个响应函数是a，第二个响应函数是b，依次注册了a，b两个事件，想让执行a完之后不在执行b了；a响应函数中加上这个代码，就会成功阻止b执行。

- `Event.currentTarget`(currentTarget当前绑定事件的对象；指定的是父级元素)

  把子元素的事件代理都转移到父元素上，只绑定一次事件就可以了。做响应的时候就要区分是哪个元素被触发

- `Event.target`(target是当前被点击的元素)

- **自定义事件**

```javascript
eg:有一个按钮，增加一个事件，在别的地方触发这个事件，而不是用回调的方式处理；
  
  var eve = new Event('custome')
  //通过new Event声明一个自定义事件，把eve这个事件当作普通事件对象，只不过这个事件是 
你定义的
 
  ev.addEventListener('custome',function(){
      console.log('custome')
  }) //ev是一个dom节点，通过dom2这种事件注册的方式绑定这个事件名称；
  
  //最后触发就是用这个dom节点，dispatchEvent这个api触发eve这个对象。
  ev.dispatchEvent(eve)
```

## 总结

- `Event`的不足是智能指定事件名，如果想给这个事件加些数据，要用`CustomEvent`
- `CustomEvent`自定义事件的一个方法，除了可以指定事件名，后边还可以跟一个object来做指定参数。这个参数是自定义的，所以用法是一样的。