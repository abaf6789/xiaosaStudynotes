# Redux

Redux是一个数据状态管理插件，搭配React特别合适。

## 使用场景

无论是移动端还是pc端，当使用React或者vue开发组件化SPA程序时，组件之间共享信息是一个非常大的问题。例如，用户登录之后客户端会存储用户信息（如`userid`、头像等），而系统的很多个组件都会用到这些信息，例如收藏、点赞、评论等。这些组件在用到用户信息时，难道每次使用都重新获取一遍？———— 自然不是这样。因此每个系统都需要一个管理多组件使用的公共信息的功能，这就是 Redux 的作用。同理，vue 也有相应的工具，即 vuex ，可以自己去 github 上搜索相关资料。

## 安装

如果单纯使用 Redux 仅仅安装 Redux 即可，执行`npm install redux --save`，不过在 React 中使用 Redux 肯定会用到 `react-redux` 这一工具，因此这里一起安装完，执行`npm install react-redux --save`。