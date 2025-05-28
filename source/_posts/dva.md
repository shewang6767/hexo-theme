---
title: 初学react
date: 2024-03-15 16:25:23
tags:
  - umi-dva
---
### model的数据管理

在umi搭建的项目中的 **约定式的 model 组织方式**如下：

![img](https://img-blog.csdnimg.cn/8f3a45b903cc4a5b948c8d12b0646e38.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjk3NzM4,size_16,color_FFFFFF,t_70)

#### 1，model里有些啥？

model里其实就是一个对象，我们把这个对象导出

```js
export default {
  //namespace命名空间，相当于给model取个名字，但是各个model的namespce是不能重复的
  namespace: 'test',
  //state我理解为是数据仓库，就是存数据的地方，model里的数据都是存放在这里的
  state : {
    name: 'wang'
  },
  /*reducers把数据存到仓库（存到state）里的唯一方法，我们修改state里的数据不能直接像this.name='liu'这样去修改，而必须通过调用reducers里的方法，在之后会详细讲到*/
  reducers:{
 	//必须return，否则会报错
  },
  /*异步方法，简单来说我们的异步请求就写在这里*/
  effects: {

  },
  /*订阅，在这里我的理解就是监听页面的，比如监听到进入了某某页面就让它执行相关代码之类的*/
  subscriptions: {

  }
}
```

#### 2，connect详解

怎么把model和我们写的组件关联起来呢？只需要用到connect就可以啦

```js
import React from 'react';
import { connect } from 'dva'       // 首先从dva中导入connect

class Index extends React.Component {
  render() {
    return (
      <div>
        <div className='box'>
        </div>
      </div>
    );
  }
}

//通过connect把model和我们写的Index关联起来，之后会解释mapStateToProps，先这样写，test是model的命名空间
const mapStateToProps = ({test}) => {
  return {
    ...test
  }
}
export default connect(mapStateToProps)(Index);
```

connect接收四个参数

> **connect(mapStateToProps, mapDispatchToProps,mergeProps,options)**
>
> mapStateToProps允许我们将   store   中的数据作为 props 绑定到组件上。
> mapDispatchToProps将       action   作为props绑定到组件上。

```js
mapStateToProps(state, ownprops) {    // ownprops 容器组件的props（未映射state）
    return { obj }   // 必须返回一个对象      // model的state，对象名就是model的命名空间
}

mapStateToProps(state){
    data: state.base
}
// 很明显了，我们在mapStateToProps 里返回什么，组件的props就会接收到什么
```

#### 3，dva数据流向

简单理解：

- 通过页面或者订阅（subscription ） 用 dispatch 调用reducer和effects里面的函数
- effects里实现services请求，把数据通过reducer放进state
- 在connect和组件连接，在页面中拿到model的数据
- 页面调接口 =>  dispatch effects
- 页面改model数据  => dispatch  reducer

#### 4，reducer

```js
// 与组件关联后，用法如下
this.props.dispatch({type: 'test/save',payload: {msg: '你好呀'}})
```

![img](https://img-blog.csdnimg.cn/aaa98fb2a1854c0aba24558d7c71c374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjk3NzM4,size_16,color_FFFFFF,t_70)

#### 5，effects

```js
*函数名 (action, effects) { 		// action跟上面一样，{ put，call，select } = effect
    // call 调异步接口，
    // put 调reducer方法
}
```

