# 常见CSS知识点汇总

## 1.CSS盒子模型

标准盒子模型：宽度=内容的宽度（content) +padding + border + margin

IE盒子模型：宽度 = 内容宽度（content+padding+border）+margin

浏览器默认为标准盒子模型

box-sizing属性可以控制盒子模型的解析模式

```css
box-sizing: border-box //IE模型
box-sizing: content-box //标准模型
```

## 2.CSS选择器有哪些？哪些属性可以继承？

CSS选择符：id选择器（#id）、类选择器（.class）、标签选择器（h1、p、div）、相邻选择器（h1+p）、子选择器（ul>li）、后代选择器（li a）、通配符选择器（*）、属性选择器（a[rel="ext"]）、伪类选择器（a:hover、a:after、a:before、li:nth-child）

可继承属性：font-size、font-family、color

不可继承的样式：border、padding、margin、width、height

优先级（就近原则）: !important > [ id > class > tag ] 

!important比内联优先级高

## 3.CSS3的新增伪类有哪些

p:first-of-type 选择属于其父元素的首个元素
p:last-of-type 选择属于其父元素的最后元素
p:only-of-type 选择属于其父元素唯一的元素
p:only-child 选择属于其父元素的唯一子元素
p:nth-child(2) 选择属于其父元素的第二个子元素
:enabled :disabled 表单控件的禁用状态。
:checked 单选框或复选框被选中。

## 4.居中的各种实现（div、浮动元素、绝对定位元素）

div：

> border: 1px solid red;
>
> margin: 0 auto;
>
> height: 50px;
>
> width: 80px;

浮动元素的上下左右居中：

> border: 1px solid red;
>
> float: left;
>
> position: absolute;
>
> width: 200px;
>
> height: 100px;
>
> left: 50%;
>
> top: 50%;
>
> margin: -50px 0 0 -100px;

绝对定位的左右居中：

> border: 1px solid black;
>
> position: absolute;
>
> width: 200px;
>
> height: 100px;
>
> margin: 0 auto;
>
> left: 0;
>
> right: 0;

