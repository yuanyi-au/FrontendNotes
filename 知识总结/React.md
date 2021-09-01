# 目录

# 正文

## 开始

`npx create-react-app xxx`

不要挂梯子，容易出错

`npm start`

vscode 里 rfc + 回车 快速生成无状态组件代码块， rcc + 回车 快速生成有状态组件代码块

### 无状态组件 与 有状态组件 的区别

- 有状态组件可以使用 state，无状态组件不能，都有 props
- 只有有状态组件才拥有生命周期，可以通过 this 接受状态和属性

如果组件自己没有私有数据，不需要使用生命周期函数，使用无状态组件性能更高。

#### props 与 state 的区别

- props 是外部传入的数据参数，不可更改
- state 是组件自己管理的数据，可以更改

### npx 与 npm 的区别

#### npm (Node Package Manager)

#### npx (Node Package eXecute)

npx 的原理是运行时到 node_modules/.bin 路径和环境变量$PATH里检查命令是否存在

- 代码运行时会将需要使用的模块下载到一个临时目录，使用后会删除，如果 npx 后面的模块无法在本地发现，就会自动下载该模块然后运行
- 由于 npx 会检查环境变量$PATH，所以系统命令也可以调用
- 
注意：Bash 内置的命令不在 $PATH 里面，所以不能用。例如：`npx cd`是错误的

详细可参见:

阮一峰老师的 [npx使用教程](http://www.ruanyifeng.com/blog/2019/02/npx.html)

[npm官方文档](https://www.npmjs.com/)

## React 的特点

1. 声明式编码
2. 组件化编码
3. 支持客户端、服务端渲染
4. 高效的 DOM Diff 算法，最小化页面重绘
5. 单向数据流

## JSX

JavaScript XML 类似于 XML 的 JS 拓展语法

本质：以 React.createElement 的形式实现，并没有直接把 HTML 代码渲染到页面上

引擎在编译 JSX 代码时，如果遇到 < 会当作 HTML 代码编译，如果遇到 {} 会当作 JS 代码编译

## 虚拟 DOM

### 虚拟 DOM 原理

React 会在内存中维护一个虚拟 DOM 树（实质上是一个JS对象），当数据变化时虚拟 DOM 树就会自动更新，然后和之前对比找出变更的部分得出一个 diff，将每次的 diff 都放到一个队列里，最后一起更新到真正的 DOM 中

### 虚拟 DOM 优缺点

- 优点：可以避免不必要的节点访问，只对真正变化的部分进行 DOM 操作，保证高效渲染
- 缺点：首次渲染时需要创建虚拟 DOM，会比 innerHTML 慢

### Diffing 算法

- tree diff：两棵树之间逐层对比。跨层级的改变会导致整个子树被删除然后重新创建，因此官方建议不要跨层级操作
- component diff：在进行组件之间的对比时，如果组件的类型不同，则立即将旧组件删除，然后创建新的组件
- element diff：在同一层中进行元素之间的对比时，通过唯一的 key 对比避免删除又重建的操作，直接移动

#### Key

diff 算法通过 key 来识别哪些元素发生了变化

如果用 index 作 key，当数据顺序发生变化时会产生问题

## 生命周期

#### 组件生命周期的执行顺序

1. Mounting 挂载

constructor()：React 数据的初始化

componentWillMount()

render()

componentDidMount()

2. Updating 更新

componentWillReceiveProps()

shouldComponentUpdate()：可以用于性能优化，返回 false 则不会重新渲染

componentWillUpdate()

render()

3. Unmounting 卸载

componentWillUnmount()

## 组件

### 函数组件与类组件

#### 区别

- class 将所有数据和逻辑都封装在同一个组件里
- function 需要为每一个操作写一个函数，数据状态应该和操作方法分离0
函数组件只做一件事：返回组件的 HTML 代码，因此 React 引入了 Hook 的概念，Hook 的作用就是为函数组件引入其他操作

## Hook

### setState()

在原生事件中和 setTimeout 中都是同步的

在合成事件与钩子函数中是异步的（实际上只是因为合成事件和钩子函数的调用顺序在更新之前，导致这两种情况下不能立即拿到更新后的值，看起来像是“异步”)

[你真的理解setState吗](https://juejin.cn/post/6844903636749778958)

### useState()

用于保存状态

### useEffect()

useEffect() 是通用的副效应钩子，作用是指定一个函数（要实现的副效应），组件每次渲染后该函数都自动执行一次

#### 参数

useEffect() 的第一个参数是函数本身，第二个参数是函数的依赖项

为了让函数不要在每次渲染后都执行，可以传入第二个参数指定函数的依赖项，这样只有依赖项发生变化时函数才会执行

#### 常见用途：

- 获取数据
- 事件监听或订阅
- 改变 DOM
- 输出日志

#### 返回值

useEffect() 允许返回一个函数，用于在组件卸载时清除副效应

实际操作中，这个函数在每次渲染后也都会执行，用于清除上一次渲染的副效应

[轻松学会 React 钩子：以 useEffect() 为例](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)

[React官方文档](https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect)

### useContext() 

保存上下文

### useRef() 

保存引用

## 受控组件 与 非受控组件

受控组件是 React 中处理输入表单的一种技术，维护表单的状态，大多数情况下建议使用受控组件。非受控组件通过使用 Ref 来处理表单数据，直接从 DOM 访问表单值。

## Fragments `<> </>`

在 React 里，return 后面必须要有一个根标签，一般我们是用 `<div> </div>`

当 <div> 没有属性的时候，我们就可以用 Fragments 来代替它

作用：允许将子列表分组，无需向 DOM 添加额外节点

目前 Fragments 唯一允许的属性是 key

## 错误边界 Error Boundaries

为了防止部分 UI 的 JS 错误导致整个应用崩溃， React 16 引入了错误边界的概念。

错误边界是一种 React 组件，可以捕获并打印发生在其子组件树任何位置的 JS 错误，然后渲染备用 UI 而不是出错崩溃了的子组件树。

错误边界无法捕获以下场景中出现的错误：
- 事件处理
- 异步代码
- 服务端渲染
- 自身抛出的错误（不是子组件）

任何未被错误边界捕获的错误将会导致整个组件树被卸载

## react-router-dom

react-router-dom 基于 react-router，加入了一些在浏览器运行环境下的功能组件

react-router 利用 history API 做到改变 URL 而不刷新页面，注意 pushState() 不能跨域会抛出错误

### 原理

#### 路由匹配原理

路由有三个属性来决定是否匹配一个 URL：

- 嵌套关系：当一个给定的 URL 被调用时，整个集合中都会被渲染，嵌套路由是一种树形结构，React Router 会深度优先遍历整个路由配置来寻找与给定 URL 匹配的路由

- 路径语法：路由路径是匹配一个（或一部分） URL 的一个字符串模式，大部分路由路径都可以直接按照字面量理解，除了以下几个特殊符号：
   - `:paramName` 匹配一段位于 `/` `?` `#` 之后的 URL，命中部分将被作为一个参数
   - `()` 其内部的内容是可选的
   - `*` 匹配任意字符直到命中下一个字符或者整个 URL 的末尾，并创建一个 splat 参数
   使用绝对路径可以使路由匹配行为忽略嵌套关系
 
- 优先级：路由算法会根据定义的顺序自顶向下匹配路由，请确认前一个路由不会匹配后一个路由中的路径

#### 动态路由

React Router 里的路径匹配以及组件加载都是异步的，可以延迟加载路由配置，首次加载包中只需要有一个路径定义，剩下的会被自动解析。

### 组件

#### 路由组件

- BrowserRouter 使用 History API 中的 pushState() 创建一个像 example.com/reference/about 这样真实的 URL，推荐使用

- HashRouter 使用 URL 中的 hash(#)部分去创建一个像 example.com/#/reference/about 的路由，不推荐在实际线上环境中使用，但是它不需要任何配置就可以运行，简单容易上手

#### 路由匹配组件

`<Route>`：`<Route path="/" component={Home} />`
   
`<Switch>`：把多个路由组合在一起
   ```
   <Switch>
     <Route />
     <Route />
     <Route />
   </Switch>
   ```
#### 导航组件

`<Link>`：取代 `<a>`，只更新变化的部分避免不必要的重渲染
   
`<NavLink>`：特殊的 <Link>，匹配路径时渲染的 <a> 标签带有 active

`<Redirect>`：路由重定向

#### 其他组件

`<ScrollToTop>`：导航到新页面时滚动条在顶部

### 相关 hook

- useParams：读取路由参数
- useRouteMatch：匹配最接近 router 树的路由
- useHistory：返回上一个网页
- useLocation：可以查看当前路由

[React Router 中文文档](http://react-guide.github.io/react-router-cn/) 仿佛机翻...

[React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html)

[【react-router】从Link组件和a标签的区别说起，react-router如何实现导航并优化DOM性能？](https://blog.csdn.net/sinat_17775997/article/details/66967854)


## Redux

Redux 是一种架构模式，将 Flux 与函数式编程结合在一起

Redux 的适用场景：
- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据


视图与状态一一对应,所有的状态，保存在一个对象里面，更改状态的惟一方法是触发一个动作

![Redux 工作流程](https://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)

### Store

Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store

- store.getState()
- store.dispatch()
- store.subscribe()

### State

Store对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State

一个 State 对应一个 View，只要 State 相同，View 就相同

### Action

State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。

View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。

store.dispatch()是 View 发出 Action 的唯一方法

### Reducer

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer

Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出

Reducer 可以拆分成三个属性：chatLog属性、statusMessage属性、userName属性

### middleware

Action 发出以后，Reducer 立即算出 State，这叫做同步；Action 发出以后，过一段时间再执行 Reducer，这就是异步。要让 Reducer 在异步操作结束后自动执行就要用到中间件（middleware）

### React-Redux

Redux 的作者封装了一个 React 专用的库 React-Redux

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）

- UI 组件有以下几个特征：

只负责 UI 的呈现，不带有任何业务逻辑
没有状态（即不使用this.state这个变量）
所有数据都由参数（this.props）提供
不使用任何 Redux 的 API

- 容器组件的特征：

负责管理数据和业务逻辑，不负责 UI 的呈现
带有内部状态
使用 Redux 的 API

#### connect()

React-Redux 提供connect方法，用于从 UI 组件生成容器组件

#### <Provider> 组件

Provider组件，可以让容器组件拿到state

使用React-Router的项目，与其他项目没有不同之处，也是使用Provider在Router外面包一层，毕竟Provider的唯一功能就是传入store对象。

```
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
```