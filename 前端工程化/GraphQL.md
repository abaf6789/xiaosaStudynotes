# GraphQL

## 背景

在平时做需求的时候，不知道大家是否会有这样的疑问：

- 比如做了一个列表页，前端只需要每个列表项的`name`、`id`、`age`，但是当我们去调接口时却返回了一大堆无关信息，这其实是由于后端的接口可能需要同时兼顾不同的平台，又或者是需求一直在变动，也不敢去轻易删除一些字段。
- 一个页面，有商品信息和商品详情两个接口，数据可以放在一起，但是因为被分成两个接口，所以需要请求两次。

## 我们想要的效果

- 当我们拿数据时可以直接拿到我们想要的数据，没有冗余字段
- 可合并的接口：可以一次请求完成并获取想要的数据

##### **所以我们可以学习一下GraphQL**

## 什么是GraphQL

首先说一下我对GraphQL的理解：

> **把后端请求的接口信息转化成一个数据库，当我们想要什么信息时，直接在数据库里`SELECT`对应的字段。**

所以就像数据库一样，在`GraphQL`中，预先定义了一张`Schema`和一些`Type`来达到我们想要的效果。

- 对于数据模型的抽象时通过Type来描述的
- 对于接口获取数据的逻辑是通过Schema来描述的

### Type

**对于数据模型的抽象是通过Type来描述的，每一个Type有若干Field组成，每个Field又分别指向某个Type。**

在`GraphQL`中，`type`分为**`Scalar Type`（标量类型）和`Object Type`（对象类型）**，这里直接引入一个直观的例子来感受一下。

```javascript
type Article {
  id: ID
  text: String
  isPublished: Boolean
}
```

这里的代码声明了一个`Article`类型，它有3个`Field`，分别是`ID`类型的id，`String`类型的text和`Boolean`类型的isPublished。`ID`、`String`、`Boolean`就是**标量类型**。

```javascript
type User {
  id: ID
  name: String
}
```

上面又声明了另一个`User`类型，然后我们更改一下刚才的`Article`类型。

```javascript
type Article {
  id: ID
  text: String
  isPublished: Boolean
  author: User
}
```

这里注意：`Article`新增的`author`的Field是我们上面声明的`User`类型。

**通过这个简单的例子来说明各个数据模型之间如何进行内在关联。**

顺便提一下，`GraphQL`支持以下标量类型：

- Int: 带符号的32位整数
- Float: 带符号的双精度浮点数
- String: UTF-8 字符串
- Boolean: true or false
- ID: 唯一标识符

### Type Modifier

类型里还有一个重要的概念，类型修饰符，当前的类型修饰符有两种，分别是`List`和`Required `，它们的语法分别为`[Type]`和`Type!`。

- 列表本身为必填项，但其内部元素可以为空           `[Type]!`
- 列表本身可以为空，但是其内部元素为必填           `[Type!]`
- 列表本身和内部元素均为必填                                  `[Type!]!`

### Schema

Schemas 描述了数据的组织形态以及服务器上的那些数据能够被查询，Schemas提供了你数据中可用的数据的对象类型，GraphQL中的对象是强类型的，因此schema中定义的所有的对象必须具备类型。

GraphQL中使用`Query`来抽象数据的查询逻辑，当前标准下，有三种查询类型，分别是*query（查询）*、*mutation（更改）*和*subscription（订阅）*。

- query（查询）：当获取数据时，应当选取Query类型
- mutation（更改）：当尝试修改数据时，应当使用mutation类型
- subscription（订阅）：当希望数据更改时，可以进行消息推送，使用subscription类型

### Query

一个最简单的query：

```javascript
query AuthorName {
  author(id: 5) {
    name
  }
}
```

这里的`name` 是`author`的一个 `Field`，也就是我们想获取的具体数据。

然后我们就可以拿到它的返回：

```javascript
{
    "author": {
        "name": "Lukas"
    }
}
```

#### 变量

`query`支持传递变量来使参数动态变化，变量以`$`开头书写

```javascript
query GetAuthor($authorID: Int! = 5) {
      author(id: $authorID) {
        name
      }
    }
```

这里`$authorID`就是一个变量，而且是一个**必填项**，且默认值为5。

#### Allases

比如说我想要`id`为10，`id`为20的数据，我们会这样写：

```javascript
query GetAuthor {
    author(id:10) {
        name
    }
    author(id:20) {
        name
    }
}
```

因为存在相同的`name`，所以会报错，所以这里就要用到`Allases`了。

```javascript
query GetAuthor {
    luka: author(id:10) {
        name
    }
    tom: author(id:20) {
        name
    }
}
```



#### Fragments

`Fragments`是一套在`queries`中可复用的`fields`。比如说我们想获取`twitterHandle`field，我们可以按下面这样做：

```javascript
    {
      chimezie: author(id: 5) {
        name
        twitterHandle
      }
      neo: author(id: 7) {
        name
        twitterHandle
      }
    }
```

但是如果`fields`过多，就会显得重复和冗余。`Fragments`在此时就可以起作用了。以下是使用`Fragments`的语法：

```javascript
    {
      chimezie: author(id: 5) {
        ...authorDetails
      }
      neo: author(id: 7) {
        ...authorDetails
      }
    }

    fragment authorDetails on Author {
      name
      twitterHandle
    }
```

####对比GraphQL与传统API请求

```javascript
// 传统API请求
GET /shops
GET /shop/:id
POST /shop/:id
DELETE /shop/:id

// GraphQL
query {
    shops(): [shop!]!
    shop($Id:Int!): shop!
}
              
mutation {
	createShop(): shop!
    updateShop($Id:Int!): shop!
	deleteShop($Id:Int!): shop!
}
```

#### Resolver

如果只有一个`query`，它并不能工作，这时就需要`resolver(解析函数)`了。

在介绍`Resolver`之前，是时候从整体上了解下`GraphQL`的内部工作机制了，假设现在我们要对使用我们已经声明的`articles`的`Query`，我们可能会写以下查询语句（同样暂时忽略语法）：

```javascript
Query {
  articles {
       id
       author {
           name
       }
       comments {
      id
      desc
      author
    }
  }
}
```

GraphQL在解析这段查询语句时会按如下步骤（简略版）：

- 首先进行第一层解析，当前`Query`的`Root Query`类型是`query`，同时需要它的名字是`articles`
- 之后会尝试使用`articles`的`Resolver`获取解析数据，第一层解析完毕
- 之后对第一层解析的返回值，进行第二层解析，当前`articles`还包含三个子`Query`，分别是`id`、`author`和`comments`
  - `id`在`Author`类型中为标量类型，解析结束
  - `author`在`Author`类型中为对象类型`User`，尝试使用`User`的`Resolver`获取数据，当前`field`解析完毕
  - 之后对第二层解析的返回值，进行第三层解析，当前`author`还包含一个`Query`, `name`，由于它是标量类型，解析结束
  - `comments`同上...

我们可以发现，`GraphQL`大体的解析流程就是遇到一个`Query`之后，尝试使用它的`Resolver`取值，之后再对返回值进行解析，这个过程是递归的，直到所解析Field的类型是`Scalar Type（标量类型）`为止。解析的整个过程我们可以把它想象成一个很长的`Resolver Chain（解析链）`。

### resolver的参数意义

`Resolver`本身的声明在各个语言中是不一样的，因为它代表数据获取的具体逻辑。它的函数签名(以`js`为例子)如下：

```
function(parent, args, ctx) {
    ...
}
```

其中的参数的意义如下：

- `parent`: 当前上一个`Resolver`的返回值
- `args`: 传入某个`Query`中的函数（比如上面例子中`article(id: Int)`中的`id`）
- `ctx`: 在`Resolver`解析链中不断传递的中间变量（类似中间件架构中的`context`）

### Mutation

在我们日常需求中，经常需要对从服务器拿到的数据进行处理。

通过`mutations`我们会给服务器发送请求来修改和更新数据，并且会接收到包含更新数据的反馈。

##### 输入类型

input类型对mutations来说非常重要，在 GraphQL schema 语言中，它看起来和常规的对象类型非常类似，但是我们使用关键字`input`而非`type`,`input`类型按如下定义：

```javascript
input CommentInput {
    body: String!
}
```

## 构建Schemas

### typeDefs

```javascript
const typeDefs = [`
  type Tag {
    id: Int
    label: String
    type: String
  }

  type TagsPage {
    tags: [Tag]
    hasMore: Boolean
  }

  type Query {
    tags(type: String!): [Tag]
    tagsPage(page: Int!, size: Int!): TagsPage
    randomTag: Tag
    lastTag: Tag
  }

  type Mutation {
    addTag(type: String!, label: String!): Tag
  }

  type Subscription {
    tagAdded(type: String!): Tag
  }

  schema {
    query: Query
    mutation: Mutation
    subscription: Subscription
  }
`];
```

我们在`schema`的`typeDefs`中定义了`tags`和`tagsPage`两个类型。

同时定义了一系列`Query`：

1. `tags`：返回所有`tag`
2. `tagsPage`：通过`pageSize`来分页返回`tag`
3. `randomTag`：返回随机的`tag`
4. `lastTag`：返回最后一个`tag`

也定义了一个`Mutation`和一个`Subscription`：

1. `addTag`：通过传入的`type`和`label`生成一个`tag`
2. `tagAdded`：通过传入的`type`把`tag`添加到对应的列表里

### reslover

```javascript
const resolvers = {
  Query: {
    tags(root, { type }, context) {
      return Tags.getTags(type);
    },
    tagsPage(root, { page, size }, context) {
      return Tags.getTagsPage(page, size);
    },
    randomTag(root, args, context) {
      return Tags.getRandomTag();
    },
    lastTag(root, args, context) {
      return Tags.getLastTag();
    },
  },
  Mutation: {
    addTag: async (root, { type, label }, context) => {
      console.log(`adding ${type} tag '${label}'`);
      const newTag = await Tags.addTag(type, label);
      pubsub.publish(TAGS_CHANGED_TOPIC, { tagAdded: newTag });
      return newTag;
    },
  },
  Subscription: {
    tagAdded: {
      subscribe: withFilter(
        () => pubsub.asyncIterator(TAGS_CHANGED_TOPIC),
        (payload, variables) => payload.tagAdded.type === variables.type,
      ),
    }
  },
};
```

`resolver`中定义了`Query`、`Mutation`、`Subscription`，里面的方法与`typeDefs`里的方法相对应。

## 总结

这个文档只是简单介绍了一下`GraphQL`的一些基础以及用法，从比较浅的层面来看，它的优势在于

1. 将多个网络请求合并成一个，减少前后端之间的网络请求次数，加快前端页面的渲染
2. 去除请求中的冗余字段

但是，它也有不足或者和js本身冲突的地方：

1. GraphQL本身是强类型的，而js是弱类型语言
2. 需要后端的配合
3. 它本身是一个中间层，所以相当于在业务中增加了一层中间层，并且需要学习成本