# CSS工作问题总结

## 1.修改input(checkbox与radio的默认样式)

### html

```html
<label className="subject-select-node" >
    <input type="radio" />
    <i className="iconfont icon-wancheng"></i>
    <span>内容</span>
</label>
```

核心思想就是将`input`隐藏，利用`i`标签以及伪类控制`radio`以及`checkbox`被选中的状态

### SCSS

```css
.subject-select-node{
        display: block;
        background: #393848;
        width: 713px;
        height: 47px;
        border: 1px solid #54535a;
        border-radius: 4px;
        cursor: pointer; 
        padding-left: 10px;
        padding-top: 10px;
        margin-top: 8px;
        input{
            position: absolute;
            left: -9999px;
        }
        input:checked+i:before {
            color: #EE5050;
            background: #FFFFFF;
            border-radius: 50%;
        }
        input:checked+i {
            border:0px;
        }
        span{
            display: inline-block;
            padding-left: 30px;
            font-size: 12px;
            color: #BBBBBB;
        }  
        i{
            position: absolute;
            margin-top: 2px;
            display: block;
            width: 18px;
            height: 18px;
            outline: 0;
            border: 1px solid #514f60;
            border-radius: 50%;
            cursor: pointer;
        }
    }
```

上面`scss`代码的核心分析，首先是隐藏原本的`input`，这里利用了绝对定位

```css
input{
    position: absolute;
    left: -9999px;
}
```

这样本来的input就不会在页面上显示了，接下来需要控制的就是`i`标签的样式。

```Scss
 i{
     position: absolute;
     margin-top: 2px;
     display: block;
     width: 18px;
     height: 18px;
     outline: 0;
     border: 1px solid #514f60;
     border-radius: 50%;
     cursor: pointer;
}
```

这里做的样式是一个圆，采用的也是绝对定位，`border`的颜色控制的和背景色不同。接下来看`input`在`checked`状态下的伪类如何设置。

```scss
input:checked+i:before {
    color: #EE5050;
    background: #FFFFFF;
    border-radius: 50%;
}
input:checked+i {
    border:0px;
}
```

这里用到了`css`的相邻选择器，这样当`input`被`checked`时就可以控制`i`标签以及伪类的样式了。

**这样就完成了input默认样式的修改**。

## 2.移动端背景图片如何处理？

之前的一个项目中遇到了一个背景图片的问题，这个问题的两大特点：

1. 移动端
2. 背景图片中还有其他背景图片的嵌套

​	首先移动端所以需要考虑到各种不同屏幕的自适应问题，然后背景图中还有其他背景图片嵌套，这里只用rem显然是不行的，需要使用百分比。

```html
<div class="outer">
    <div class="inner-wrap">
        <div class="inner-content">
           
        </div>	
    </div>
</div>
```

​	上面的结构就是在项目中所使用的，首先最外层的`outer`是外层的背景图片所在的`div`，中间用了`inner-wrap`这个类去做一层包裹，这层的包裹作用在于让内部的`inner-content`的内容能固定在这个容器中，这样能让内容随着百分比自适应。

```css
.outer {
    width:100%;
    height:0;
    padding-top:58%;
    background:url("url") no-repeat;
    background-size:contain;
    position:relative;
}
.inner-wrap {
    width:100%;
    height:100%;
    position:absolute;
    top:0;
    left:0;
    box-sizing:border-box;
}
.inner-content {
    background:url("url") no-repeat;
    background-size:contain;
    padding-top:30%
}
```

​	外层采用相对定位，内层用一层`wrapper`绝对定位来保持自适应。

​	背景图片的大小控制使用了宽度100%，不设高度，利用`padding-top`或者`padding-bottom`来控制大小。

## 3.背景图片在IphoneX上的适配问题

​	一般UI给的视觉图是以iphone6这个尺寸来给的，所以有些背景图片的大小会不够，在iPhoneX上无法撑满，导致屏幕会有留白问题，这里就要使用`background`的渐变来控制

```css
background:#2848d1 url(url) no-repeat
```

当背景图片的大小不够时，会使用给定的颜色来填充剩下的部分。