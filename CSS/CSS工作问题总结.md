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