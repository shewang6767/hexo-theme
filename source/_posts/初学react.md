---
title: 初学react
date: 2024-03-15 16:25:23
tags:
  - react
---
![image-20231112234525576](D:\webstorm\typora\images\image-20231112234525576.png)

# React介绍

由Meta公司（facebook）研发，是一个用于    构建Web（网页）    和   原生交互界面 （ios或者Android）    的库

### React优势（流行）

相对于传统基于DOM开发的优势：组件化的开发方式，不错的性能（传统的Dom性能差）

相较于其它前端框架的优势：丰富的生态是所有框架中最好的（Vue，Angular），跨平台（构建web，原生app），

# React开发环境搭建

### 使用create-react-app快速搭建开发环境

create-react-app是一个快速   创建React开发环境  的工具，底层由Webpack构建，封装了配置细节，开箱即用

```js
npx create-react-app react-basic
// npx   Node.js工具命令，查找并执行后续的包命令
// create-react-app     核心包(固定写法)，用于创建React项目
// 项目名（自定义）
npm start    // 启动项目
```

官网：https://zh-hans.react.dev/learn/start-a-new-react-project

<img src="D:\webstorm\typora\images\image-20231113001302857.png" alt="image-20231113001302857" style="zoom: 67%;" />

项目目录：package.json   项目配置文件：dependencies核心包，自定义命令，

​					src 源码目录   创建项目只保留两个文件： App.js   和  index.js（项目入口文件）

```js
// index.js 项目的入口文件    其他文件将它们组合在一起，并将最终成果注入 public 文件夹里面的 index.html 中
// 再清理index.js文件中不需要的部分   再清app.js

//（最核心的两个包：react，react-dom）
import React from 'react';
import ReactDOM from 'react-dom/client';
// 项目的根组件
import App from './App';

// 把  App根组件  渲染到  id为root  的dom节点上
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <App />
);

```

```js
// app.js项目的根组件
// app.js的引入到index.js,被一些核心代码渲染到public里面的index.html（root的div上）
function App() {
  return (
    <div className="App">
      this is react
    </div>
  );
}

export default App;
```

# JSX

概念：JSX是 JavaScript 和  XML  (HTML)  的缩写，表示在 JS代码中编写HTML模版结构 ,  它是React中   编写UI模版  的方式

优势：1，html的声明式模板语法    2，js的可编程能力

jsx的本质：jSX 不是标准的]S语法，是]5的语法扩展，浏览器本身不能识别，需要通过解析工具（babel）做解析之后才能在浏览器中运行

![image-20231113235316257](D:\webstorm\typora\images\image-20231113235316257.png)

### 识别js表达式

```js
// app.js
// 在 jSX 中可以通过 {} 大括号语法识别 JavaScript 中的表达式，比如常见的变量、函数调用、方法调用等等
// jsx 通过
// React 组件必须返回单个 JSX 元素  最外边只能有一层div
// 组件以大写字母开头
function App() {
  return (
    <div className="App">
      this is react

      // 1，使用引号传递字符串
      {‘this is react’}

      // 2，使用javascript变量
	  { sum }

      // 3，函数调用和方法调用
      { get() }
      { new Date().getDate() }

      // 4，使用对象
	  <div style={ { color='red'} }>this is react</div>
    </div>
  );
}
//  注意: if语句、switch语句、变量声明属于语句，不是表达式，不能出现在{}中
```

### 实现列表渲染（要有key属性）

原生js中map方法

### 条件渲染

```js
简单条件渲染
逻辑与 &&                 // 只控制一个元素的显示隐藏
 { isLogin &&  <span>   woshispan    </span> }
三元表达式              // 控制两个元素来回切换
isLogin ? <span> loading </span> : <span> woshisapna </span>

  复杂条件渲染
  // 根据不同的状态场景显示不同的模式
  方案：自定义函数 + if判断
```

### 响应事件

```js
// 在组件中声明  事件处理  函数来响应事件
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }
    // 获取事件形参e
  function handleClick(e) {
      log('点击了',e)
  }

// 语法： on事件名={ 事件处理函数名 }     驼峰命名
  return (
    <button onClick={handleClick}>    // 没有小括号
      Click me
    </button>


    // 如果想传递自定义参数， 要改造成箭头函数引用   就像 onClick={ () => {handleClick( 参数 )} }  在箭头中调这个函数
	<button onClick={ () => handleClick( 参数 ) }>     </button>

//同时传递事件对象e 和 形参
function handleClick(参数, e) {}
<button onClick={ (e) => handleClick( 参数, e ) } >
  );
}
```

### 组件

（首字母大写函数，UI界面的一部分，可以嵌套，复用，有自己的逻辑和外观 ）

常规函数 function  Button()  {}

箭头函数 const    Button  = （）=>    {}

### usestate

添加状态变量（数据驱动视图）

usestate 修改状态的规则：状态不可变

​		react中，状态被认为是只读的，如果想要修改，应该是替换掉，而不是直接修改，直接改的话不能引起试图更新

```js
// 首先，从 React 引入 useState是一个函数
import { useState } from 'react';
// 组件中声明一个 state 变量
function MyButton() {
  const [count, setCount] = useState(0);
  // ...当前值  count     用于更新它的函数（setCount）    useState(0)里面传的是初始值
    // 命名规则 [something, setSomething] 这样为它们命名


//   修改复杂状态（对象）的情况
setForm  { ...原本的对象，name：' john ' } ）   //   展开原来的对象，后边输入新的属性或方法，有的就替换，没有就是添加
```

```js
// 第一次显示按钮时，count 的值为 0，因为你把 0 传给了 useState()。
// 当你想改变 state 时，调用 setCount() 并将新的值传递给它。点击该按钮计数器将递增
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

```js
// 尝试多次渲染  同一个组件， 每个组件都会拥有自己的 state。 你可以尝试点击不同的按钮
// 注意，每个按钮会 “记住” 自己的 count，而不影响其他按钮
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}

```

### 组件的样式处理

```js
// 行内  <div style={ color="red" }>    不推荐
// 在 React 中，你可以使用 `className` 来指定一个 CSS 的 class。它与 HTML 的 class 属性的工作方式相同：
/* 在引入的 CSS文件中 */  前提是你引入了css文件 import ’./css‘
.avatar {
  border-radius: 50%;
}


classNames简单的js库，通过 条件 动态的控制class类的显示
例如：className={classNames('静态的类名')，{ active: 条件表达式 }  }
下载 npm install classNames
导入
```

### 表单绑定

  就是state状态 控制  表单状态属性value值    他俩做一个绑定

```js
// state状态值
const [value, setValue] = useState('')
// input 的value绑定 state的value
<input value={value} onChange={ (e) => setValue(e.target.value)}
```

react获取dom操作dom

```js
1// useRef 钩子函数   创建ref对象
 const inputRef = useRef(null)
 // 与jsx绑定
 <input type=’text‘  ref={inputRef} />
2// dom可用时  inputRef.current 获取即可
  用ref对象身上的属性
```

### 组件间共享数据（通信）

#### 父子之间：

<img src="D:\webstorm\typora\images\image-20231123151535264.png" alt="image-20231123151535264" style="zoom: 67%;" />

1. ​    父组件传，子组件绑定属性，子组件接收props，是一个对象，就可以使用
2. ​	props可以传任意数据（数组，对象，数字，字符串，函数，布尔，jsx，），子组件不能修改props，只能父修改
3. ​    children 特殊的props，的一个属性，识别    内容嵌套在子组件内时
4. ​    子传父：子组件调用父组件中的方法，先要父组件传处理函数过去，子组件调用然后可以传参数

#### 兄弟之间：

<img src="D:\webstorm\typora\images\image-20231123155833303.png" alt="image-20231123155833303" style="zoom:67%;" />

状态提升：通过共同的父组件，a组件  =>  父组件 => b组件

在前面的例子中，每个按钮都有自己的state，但是常常需要组件共享数据一起更新，所以需要 各个state ’向上‘移动到最接近所有按钮的组件

```js
//  state 上移到 MyApp 中
export default function MyApp() {
  const [count, setCount] = useState(0);
   //
  function handleClick() {
    setCount(count + 1);
  }

  return (
// 接着，将 MyApp 中的点击事件处理函数以及 state 一同向下传递到 每个 MyButton 中。你可以使用 JSX 的大括号向 MyButton 传递信息
    <div>
      <h1>Counters that update separately</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton() {
  // ... moving code from here ...
}

//最后，改变 MyButton 以 读取 从父组件传递来的 prop：
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

先是state上移到了包裹所有按钮的组件，再通过点击事件处理函数和state一同向下传递到每个button中

这种传递方式叫prop，要在子组件中接收

新的 `count` 值会被作为 prop 传递给每个按钮，因此它们每次展示的都是最新的值。这被称为“状态提升”。

#### 跨层级组件：

context

<img src="D:\webstorm\typora\images\image-20231123161804835.png" alt="image-20231123161804835" style="zoom:67%;" />

用createContext方法创建一个对象（最外层），在顶层组件中，用对象.Provider 组件（高阶组件）提供数据，在底层组件中用useContext钩子函数获取

```js
// 顶层组件
function App() {
    return (
    <div>
   		<对象.Provider value={ 数据 }> 内容 </对象.Provider>
    </div>
    )
}
// 底层组件
return (
	const msg = useContext(数据)   //接收
)
```

### useEffect

组件没有发生任何用户事件, 组件渲染完毕就要和服务端请求数据,      这个过程叫只由渲染引起的操作

比如:  发送请求,  更改dom

```js
// 语法   两个参数
// 第一个  是一个函数,也叫副作用函数,是我们要执行对的操作
useEffect( () => {   可执行操作   }, [] )
// 第二个  依赖项数组,依赖项不同会决定前面副作用函数的执行时机..

   1,空数组依赖时候, 组件只在初始渲染时执行一次
   2,当没有依赖项时, 组件初始渲染时执行一次 + 组件更新渲染时执行
   3,有特定依赖项时, 组件初始渲染时执行一次 + 特定依赖项变化执行  例:[state]

// 由渲染本身引起的，
// 清除副作用: 比如在useEffect的副作用函数中做了一些操作,有时需要及时清除掉,比如开启了定时器
// 清除最好的时机：组件被卸载的时候
...语法:
    useEffect( ()=> {
        // 在副作用函数中return
        return () => {
            // 清除副作用的逻辑
        }
    } ,[])
```

### Hook函数

以 `use` 开头的函数被称为 **Hook**。比如：useState、useEffect    是 React 提供的内置 Hook，实现逻辑封装和复用。你可以在 [React API 参考](https://react.docschina.org/reference/react) 中找到其他内置的 Hook。也可以根据现有的Hook编写自己的Hook。Hook 比普通函数更为严格

你只能在你的组件（或其他 Hook）的 **顶层** 调用 Hook。如果你想在一个条件或循环中使用 `useState`，请提取一个新的组件并在组件内部使用它。

```js
// 自定义hook(逻辑复用)    1.声明use开头的函数
// 比如布尔值频繁切换的场景
function useToggle() {
      // 2.写复用的代码逻辑
    const [ value, setValue] = useState(true)
    const toggle = () => { setValue(!value )}
    return {
        // 3.那些状态（数据）或者 回调函数 需要在其他地方使用就return出去
        value,
        toggle
    }
}
// 4.使用的时候直接解构出来 const { value, toggle } = useToggle()
```

#### ReactHooks使用规则

1. 只能在组件中  或者  其他自定义hook中 调用 （ 组件外不可以用 ）
2. 只能在组件的顶层调用，不能嵌套在if ，for， 或者其他的函数中

# 案例

为了“记住”一些东西，组件使用 *state*

要从多个子组件中收集数据，或让两个子组件相互通信，请在其父组件中声明共享state，父组件可以通过props将该state传回给子组件。这样子组件彼此同步并与其父组件保持同步。

prop中子无法修改父

在 React 中，通常使用 `onSomething` 命名代表事件的 props，使用 `handleSomething` 命名处理这些事件的函数。

### 不变性

在案例中，没有直接修改数组的原数据，而是.slice() 出一个新副本，修改这个副本。在一般情况中，要么修改原数据，要么修改副本，都可以满足我们的需求，但是用副本来说，有几个好处：可以实现复杂的功能，在案例中可以实现一个回溯的功能，此功能有广泛的应用，撤销和重做某些操作是应用的常见功能

### 时光回溯

每次落子都用slice创建了新的副本，这可以视为原数组squares不变， 把过去的squares存在一个新的数组中，这个数组视为一个新的state变量

### key

渲染列表时：要有key属性作为区分

# props   和   state

props 就像是你传递的参数至函数，它使父组件传递数据给子组件，

state  就像是组件的内存，对一些数据进行追踪，并根据交互来改变值，



# react对可见设计和应用构建的思考

构建用户界面，分解成组件，把组件链接在一起，使数据流经它们

#### 步骤一：将UI拆解为组件层级结构

<img src="D:\webstorm\typora\images\image-20231122151840463.png" alt="image-20231122151840463" style="zoom:50%;" />

现在你已经在原型中辨别了组件，并将它们转化为了层级结构。

在原型中，组件可以展示在其它组件之中，在层级结构中如同其孩子一般:

```js
|---FilterableProductTable
    |--SearchBar
    |--ProductTable
     	|--ProductCategoryRow
    	|--ProductRow
```

#### 步骤二：使用 React 构建一个静态版本

有了组件的层级结构后，最直接的办法就是构建一个不带交互的静态版本，再一一添加交互，

既可以从层级高的组件自上而下构建（小案例），也可以从底层级组件构建（大项目）

在我们构建好了组件之后，我们就有了待渲染的可复用组件库，这是静态的，最顶层的组件将接收数据作为其prop传递给子组件，称为单向数据流

#### 步骤三：找出 UI 精简且完整的 state 表示（找出state）

不是state的情况：

​			需要修改的数据

​			通过props从父组件传递

​			无法基于已存在于组件中的state和props计算出新的东西

#### 步骤四：验证state应该放在哪里

目的：搞清楚哪个组件应该拥有哪个state

对每一个state进行以下分析：

​		1，看看每一个基于特定state渲染的组件

​		2，寻找他们最近并且共同的父组件

​		3，最后，决定这个state放在哪里

  				1。一般情况直接放在父组件上就可以

​				  2。或者放在父组件的父组件

​				  3。如果找不到，可以创建单独的组件来存放state，并将它添加到他们父组件上层的某个地方。

#### 步骤五：添加反向数据流

# Redux

react常用的状态管理工具（类似Pinia，Vuex），可以独立运行，不绑定react

<img src="D:\webstorm\typora\images\image-20231126082400166.png" alt="image-20231126082400166" style="zoom:50%;" />

<img src="D:\webstorm\typora\images\image-20231213171828374.png" alt="image-20231213171828374" style="zoom: 50%;" />

state：对象，存放我们管理的状态数据

action：对象，描述怎么修改state

reducer：函数，根据action 生成新的 state

## 单独使用

```js
// 1，定义reducer函数（两个参数）
function reducer( state, action ) {
    // state 对象 ，初始的数据状态，
    // action对象 上的属性type，标记当前想要做什么样的修改  action.type
    // reducer作用，根据不同的action，返回新的state
    return state
        // 数据不可变，基于初始state，返回生成新的 state
}
// 2，用reducer函数生成store实例
const store = Redux.createStore(reducer)

// 3,用store实例中的subscribe方法 监测（订阅）数据的变化
store.subscribe( () => {
    // 回调函数 在数据一发生变化就执行
    log('state 变化了')
} )

// 4，用store实例中的dispatch方法，提交一个action对象更新状态
//    Redux中修改数据的唯一方式就是dispatch
store.dispatch({
    type: '' // 和reducer函数中type标记的是一样的
}).

// 5，用store中的getState方法获取最新状态 更新渲染 到视图中
store.getState()  // 可以在第三步中进行获取并更新
```

## 环境，工具

两个插件---Redux Toolkit   和 react-redux

1，Redux Toolkit（RTK）  工具集，简化书写方式，例如：简化store的配置，内置immer可变式状态修改，内置thunk更好的异步创建

2，链接react  和  redux  的中间件

<img src="D:\webstorm\typora\images\image-20231126085231056.png" alt="image-20231126085231056" style="zoom: 33%;" />

```JS
// 要有react项目，没有就创建（npx create-react-app react-redux）
// 安装
npm install @reduxjs/toolkit react-redux
// 启动项目即可
```

## 配合react使用

### 先创建store目录 (@reduxjs/toolkit)

<img src="D:\webstorm\typora\images\image-20231126085756567.png" alt="image-20231126085756567" style="zoom:33%;" />

modules：子模块

index.js：把子模块导入进去，store的入口文件，最后导出store

<img src="D:\webstorm\typora\images\image-20231126165940242.png" alt="image-20231126165940242" style="zoom:33%;" />

```js
// 子模块.js
import { createSlice } from "@reduxjs/toolkit"
// createSlice创建 store
const counterStore = createSlice({
    name: 'counter'，     // store名字
    initialState: {       // 初始状态数据
      count: 0
    }，
    reducer: {
      // 修改状态的方法（同步）  action：参数
      方法名(state， action ) { }
    },
})
// 解构出  创建action对象的函数（actionCreater）
const { 函数名，。。。  } = counterStore.actions
// 获取reducer函数
const counterReducer = counterStore.reducer
// 导出    创建action对象的函数   和   reducer  函数
export { 函数名，。。}
export default counterReducer           // 子模块名
```

```js
// 根store
import { configureStore } from "@reduxjs/toolkit"

import 子模块名 from "./modules/counterStore"

// 创建根store 组合子模块
const store = configureStore({
    reducer: {
        counter: counterReducer
    }
})
// 导出根store
export default store
```

### 为react注入store  (react-redux)

```js
// 1.
// 内置Provider 组件通过 store参数 把创建好的store 实例注入应用中
import store from './store'
import { Provider } from 'react-redux'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider  store={store}>
    <App />
  </Provider>
);

// 2.
// 使用store中的数据
// 要使用钩子函数：useSelector     作用：把store中的数据映射到 组件中
例：const { 数据名 } = useSelector(state => state.counter)


// 3.
// 修改store中的数据
// 使用另一个hook函数   useDispatch     作用：生成dispatch函数 （提交action对象）
import { 创建action的函数 } from ‘ 子模块 ’
import { useDispatch， useSelector } from ‘react-redux’
  // 得到dispatch 函数
  const dispatch = useDispatch()
  // 调用dispatch 提交action (子模块中导出的actionCreater)
  dispatch( action() )
  dispatch( action( 参数 ) )  // 调用时传参, 传递的参数会自动跑到，函数的 action.payload 属性中
例如： 子模块中 函数名（state， action）{
    state = action.payload
}
```

``` js
// 异步操作
// 1.创建store的写法不变，配置好同步修改state的方法
// 2.单独封装一个函数，在这个函数内部return 一个新函数，在新函数中
function 函数名() {
    return async () => {
        //   2.1 封装异步请求获取数据
        const res = await axios.get(url)
        //   2.2 调用同步方法，传入异步数据（获取的数据传参），生成action对象并dispatch提交
        dispatch(同步中生成action对象的方法() )
    }
}
// 3.组件中dispatch写法不变
```

# react-Redux

> 什么是react-redux？ react-redux是一个react插件库,专门用来简化react应用中使用redux。他是从redux封装而来，因此基本原理和redux是一样的，同时存在一些差异。

<img src="D:\webstorm\typora\images\image-20231213171738389.png" alt="image-20231213171738389" style="zoom: 67%;" />

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）

# reactRouter

## 使用

安装reactRouter包

npm i react-router-dom

```js
import {RouterProvider, createBrowserRouter} from 'react-router-dom'
// 创建router实例
const router = createBrowerRouter([
    {
      path: '/login',
      element: <div>我是登录</div>    //即支持组件，可以是常规的元素jsx
    },v
])

// 绑定路由  把根组件替换成RouterProvider组件
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
       <React.StrictMode>
          <RouterProvider router={router}></RouterProvider>
        </React.StrictMode>
);
```

### 分模块

page 页面  例如：login.js    article.js

router 路由模块 index.js  配置路由，把所需要的路由页面导入进去 ，最后把总路由导出

### 路由导航

多个路由之间跳转，有可能携带参数进行通信（两种方式）

```js
// 1，声明式导航 <link to='/article'>文章</link> 例：后台左侧的导航跳转
// 当成  a  标签用即可。      指定to属性跳转到path，组件会被渲染成浏览器支持的a标签
// 传参时，通过字符串拼接即可



// 2.编程式导航 通过hook函数 useNavigate 得到导航方法navigate

const navigate = useNavigate()
navigate('/article')
// 通过调用这个函数 以命令式 跳转

// 3,获取当前路由
window.location.pathname
```

#### 跳转传参

```js
// 以编程式为例，声明式 同理
1，searchParams 传参
	// 在跳转时路由？拼接传递的参数  & 链接多个参数
    navigate('/Article?id=1101&name=jack')
	// 在对应路由 中接收参数  hook函数 useSearchParams 数组中解构出 params对象
    const [params] = useSearchParams()
    // params 身上的get方法获取属性名
    const id = params.get('id')


2，params 传参
	// 跳转时用 / 斜杠
	navigate('Article/1001/jack')
	// 配置路由时要有占位符
	path: '/Article/:id/:name'
	// 接收参数   hook 函数useParams
	const params = useParams()
    const id = params.id
```

#### 嵌套路由

<img src="D:\webstorm\typora\images\image-20231127174815874.png" alt="image-20231127174815874" style="zoom: 67%;" />

``` js
// 1，配置子路由
const router = createRouter([
    {
        path: '/',
        element: <Layout />,
        children: [                   // 子路由配置
        	{ path: 'board', element: <Board /> },
    		{ path: 'about', element: <About /> }
        ]
    },
    {
         path: '/Article',
     	 element: <Article />
    }
])
// 2，配置二级路由出口 （渲染的位置） 用<Outlet/>组件
    2.1 默认二级路由 访问的是一级路由时，默认的二级路由得到渲染
    	在二级路由的配置中 path 换成 index 属性，并设置成 true
    	index: true
    	并在 跳转时，更改跳转的路径为  ‘/’
    	to="/"
    2.2 404路由
    	输入的url都匹配不到对应的path时，显示的404 兜底组件渲染
    	准备一个 NotFound 组件
    	const NotFound() {
            return (
                // 自定义模板
            )
		}
		export default NotFound
		在配置的路由表 末尾，以 * 号作为 path 配置路由
        { path： *， element： <NotFound /> }
     2.3 两种路由模式
     history 和 hash ，ReactRouter 创建路由时一般用的都是 createBrowerRouter，创建hash模式用createHashRouter
     history	url/login      history对象+pushState事件		需要后端支持
     hash       url/#/login    监听hashChange事件				不需要
```

# 数据Mock

在前后端 分离的 开发模式 下，前端可以在 没有 实际的后端接口下，先模拟假数据  进行正常业务的开发

<img src="D:\webstorm\typora\images\image-20231128171131129.png" alt="image-20231128171131129" style="zoom:50%;" />

json - server 实现 数据mock

```js
// 1. 安装 json-server
 	npm i -D json-server
// 2. 准备一个json文件，例： data.json
// 3. 添加启动命令，
	"server": "json-server ./server/data.json --port 8888"
// 4. 可以访问接口了
```

