# jdjr项目开发app部分

## CSS部分

在app目录下新建css文件夹，在里面新建`reset.scss`、`layout.scss`、`element.scss`。

`reset.css`

这个文件是用来清除标签元素的默认样式的，**在路由里引进**。

`layout.scss`

这个文件中声明了`element`中需要的一些依赖，如下

```scss
@charset "UTF-8";

@mixin flex($direction:column,$inline:block) {
    display: if($inline==block,flex,inline-flex);
    flex-direction: $direction;
    flex-wrap: wrap;
}

```

- 声明了编码形式


- 把`element`中需要的样式抽离成更小的形式（和组件化类似）

`element.scss`

这里面就是所有抽离出来可以公用的css样式，因为这个项目大部分的组件都是小标题加内容的形式，所以我们在js目录中定义了一个智能组件`panel`，css里就对这个`panel`的样式进行设计。另外还有btn和flex布局等公共样式的抽离。

## JavaScript部分

### 1.路由

vue的项目，js目录下就是主要的项目构成。首先在js目录下分别新建`App.vue`和`main.js`。然后新建文件夹`router`。

`App.vue`

```vue
<template>
    <div id="app">
        <router-view/>
    </div>
</template>

<script>
export default {
    name: "App",
}
</script>

```

`main.js`

```javascript
import Vue from "vue"
import App from "./App.vue"
import router from "./router"

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
    el: "#app",
    router,
    components: { App },
    template: "<App/>",
})
/* eslint-enable no-new */

```

在新建的`router` 文件夹中新建`index.js`。

在终端使用`npm`/`cnpm`安装`vue-router`。

新建`home`、`financial`、`ious`、`personal`四个文件夹，里面各建`index.vue`。这四个文件就对应着京东金融这个项目的`首页`、`金融`、`白条`、`我的`。注意:`import`的类首字母必须大写。之后记得将`reset.scss`引入路由，在路由引入之后接下来所有写的vue文件都不用再引入了。

要使用路由要声明一句`Vue.use(Router)`。

```javascript
import Vue from "vue"
import Router from "vue-router"
import Home from "../home/index.vue"
import Financial from "../financial/index.vue"
import Ious from "../ious/index.vue"
import Personal from "../personal/index.vue"
import "../../css/reset.scss"

Vue.use(Router)

export default new Router({
    routes: [
        {
            path: "/",
            name: "Home",
            component: Home,
        }, {
            path: "/finance",
            name: "Financial",
            component: Financial,
        }, {
            path: "/ious",
            name: "IOUS",
            component: Ious,
        }, {
            path: "/personal",
            name: "Personal",
            component: Personal,
        },
    ],
})

```

### 2.core && public

在js目录下新建`core`和`public`文件夹。

#### core

`core`文件夹下放的是一些智能组件，比如我们刚才上面提到的`panel`。在`core`文件夹下新建`panel.vue`、`slider.vue`、`btn.vue`。

- `btn.vue`中存放的组件是主页上的app下载的按钮。
- `panel.vue`中存放的组件就是前面提到过的小标题+内容的组件，由于整个项目都沿用了这么一套，所以抽离出来。
- `slider.vue`中存放的组件是轮播图的组件，这一部分需要安装轮播图的相关插件。

#### public

`public`文件夹中放的是一些公用组件，比如上导航，下导航，和一些公共信息。在`public`文件夹下新建`header.vue`，`navbar.vue`，`footer.vue`。

- `header.vue`中放的是上导航部分。
- `footer.vue`中放的是下导航部分。
- `navbar.vue`中放的是公共信息。

