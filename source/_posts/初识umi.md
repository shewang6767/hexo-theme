---
title: 初学react
date: 2024-03-15 16:25:23
tags:
  - umi
---
```js
// 现有node环境，保持在10.13版本以上
$ node -v
v10.13.0

// 推荐yarn管理依赖
# 国内源
$ npm i yarn tyarn -g
# 后面文档里的 yarn 换成 tyarn
$ tyarn -v

# 阿里内网源
$ tnpm i yarn @ali/yarn -g
# 后面文档里的 yarn 换成 ayarn
$ ayarn -v


// 创建项目  在新建的文件夹下
$ yarn create @umijs/umi-app
// 安装依赖
yarn install   或   yarn
// 启动
yarn start
```

## Umi 项目

```js
.
├── package.json    //包含插件和插件集，以 @umijs/preset-、@umijs/plugin-、umi-preset- 和 umi-plugin- 开头的依赖会                       被自动注册为插件或插件集
├── dist       // 执行umi build后打包好的文件，后期要部署到服务器
├── mock       // 存储mock文件，模拟的一些数据，，此处的js和ts文件会被解析成mock文件
├── public     // 此处的文件会被copy到输出路径，变通的数据资源目录，和无需打包的资源
└── src       // 源码
    ├── .umi               // 临时文件，如入口文件，路由等，不要提交到仓库，它会在dev和build时重新生成
    ├── layouts/index.tsx       //约定式路由时的全局布局文件
    ├── pages                   // 存放所有路由组件
     	├── index.less
        └── index.tsx
    ├── models                  // 数据流
    ├── wrappers                // 权限管理
    └── app.ts（.js）       // 运行时配置文件，在这儿可扩展运行时的能力，如修改路由，修改render方法
	├── global.css         // 约定的全局样式文件，自动引入，也可以用 global.less
├── .umirc.ts        // 配置文件，包含 umi 内置功能和插件的配置   🔺
├──  config / config.js    // 同 .umirc.ts 二选一就行
├── .editorconfig    // 编辑器高亮支持
├── .env             // 环境变量，
├── .gitignore       // git忽略文件
├── tsconfig.json    // ts的配置文件
├── typings.d.ts     // ts文件
```

### 配置文件

```js
.umirc.ts 或  config/config.ts   配置项目和插件，支持es6
// 如果项目复杂也可以拆分
congig/route.ts   路由配置
config/config.ts
// 本地临时配置，可新建.umirc.local.ts, 最后会和。umirc.ts做deep merge（深合并）形成最终配置
 .umirc.local.ts仅在umi dev时有效，umi build时不会被加载


 // js代码提示
 ？

 // 多份环境多份配置

 通过指定UMI_ENV=cloud
```

### 运行时配置

```
// 约定src/app.tsx为运行时配置
区别于上是 运行在浏览器端，注意不要引入node依赖
```

formatMessage

1. 调用`formatMessage`函数，并将参数对象作为参数传递给它。
2. `formatMessage`函数将根据消息ID查找相应的消息文本，并对其进行格式化处理。
3. 函数返回格式化后的消息文本，可以在应用程序中使用该文本。



### 构建时配置

<img src="D:\webstorm\typora\images\image-20231128174303368.png" alt="image-20231128174303368" style="zoom:67%;" />

```js
// umirc.ts 文件
import { defineConfig } from 'umi';
export default defineConfig({
    nodeModulesTransform: {    // node modules 目录下依赖文件的编译方法
        type: 'none'   // （none 或 all） none快 兼容性低 all 慢 兼容性高
    }，
    routes: [{
    	path:'/', component:'@/pages/index'
	},
    fastRefresh:{},    // 快速刷新，可以保持组件状态，同时编辑提供及时反馈
    devServer: {
        port: 8081,   // .env权限更高
        https: true   // https 安全访问
    },
    title: 'UMI3',     // 标题
    favicon: '地址'    // 线上，线下都可以
	dynamicImport: {
        loading: '@/components/loading', // 按需加载时指定的loading
    },
    mountElementId: 'app',  // 指定渲染的html根元素
    theme    // pc端主题色
			// 手机端主题：v2版本跟pc端配置一样， v5版本可以在src下配置全局的样式文件global.less
});
```

### 模板约定

默认的umi项目目录没有html，自己创建再pages下的 document.ejs 书写自己的代码

自定义项目入口

组件库:

<img src="D:\webstorm\typora\images\image-20231211224005505.png" alt="image-20231211224005505" style="zoom: 33%;" />

移动端antd-mobile-v2或者 antd-mobile-v5两个版本：@umijs/preset-react包 更新到最新

### 图片和其他资源引入：

1，传到CDN，在js或者css输入绝对地址，（数据图片）

2，放到项目目录，在js或css中引入，（写死的）相对路径，如果图片小于10kb，还会被转化为base64

<img src="D:\webstorm\typora\images\image-20231212131536159.png" alt="image-20231212131536159" style="zoom:67%;" />

### less-样式模块化

没有内置sass

<img src="D:\webstorm\typora\images\image-20231212132043349.png" alt="image-20231212132043349" style="zoom:50%;" />

<img src="D:\webstorm\typora\images\image-20231212132614045.png" alt="image-20231212132614045" style="zoom:50%;" />

### hooks + 函数式 编写组件

```js
function 组件() {}
const 组件 = (props) => {
    // 使用hooks

    // 定义 函数  变量
    return jsx
}

useMemo(() => [}, [依赖项])  // 类似于useEffect
useCallback()
```

### 路由，动态，权限，约定式

#### 配置:

<img src="D:\webstorm\typora\images\image-20231212142053195.png" alt="image-20231212142053195" style="zoom:50%;" />



<img src="D:\webstorm\typora\images\image-20231212142842774.png" alt="image-20231212142842774" style="zoom: 50%;" />

```js
export default [
    {
        path: '/',
        component: '@/layouts/base-layouts',
        routes: [                   // 子路由
            { path: '/login', component: '@/pages/login' }，
            { path: '/reg', component: '@/pages/reg' },
    		{
                path: '/goods',
                wrappers: ['@/wrappers/auth'],        // 授权路由配置项wrappers  ，访问goods，会先满足auth的条件
                component: '@/layouts/aside-ayouts',
                routes: [
                    { path: '/goods'， component: '@/pages/goods'},
                    { path: '/goods/:id', component: '@/pages/goods/goods-detail'}
                ]
            },
    		{ path: '/', redirect: '/login'}
        ]
    }
]
```



#### 页面跳转，参数接收

##### 传参：

<img src="D:\webstorm\typora\images\image-20231212144328918.png" alt="image-20231212144328918" style="zoom:50%;" />

<img src="D:\webstorm\typora\images\image-20231212151956365.png" alt="image-20231212151956365" style="zoom: 50%;" />

<img src="D:\webstorm\typora\images\image-20231212152547223.png" alt="image-20231212152547223" style="zoom:50%;" />

##### 接收：

<img src="D:\webstorm\typora\images\image-20231212153049279.png" alt="image-20231212153049279" style="zoom: 50%;" />

1，路由上下文

<img src="D:\webstorm\typora\images\image-20231212153022013.png" alt="image-20231212153022013" style="zoom:50%;" />

2，hooks

```js
import { useHistory, useLocaltion, useParams, useRouteMatch } form 'umi'
export default GoodsDetail() {
    const { sreach } = useLocaltion;
    const { id } = useParams
}
```

### 数据模拟umi-mock

mock目录： 在根目录下，或者在page的目录下（同时在这目录下的mock加上下标_mock）

<img src="D:\webstorm\typora\images\image-20231212154023015.png" alt="image-20231212154023015" style="zoom: 50%;" />

```js
// 例：
export default {
    // 支持值为 object和Array
    'GET /umi/goods': {
        success: true,
        errorCode: 'xx',
		errorMessage: 'ooo',
        showType: 1,
        traceId: 'i',
		data: [
			{ id: 1，name: '韭菜' },
    		{ id: 2, name: '西红柿' },
   		 ],
	},
}
// 写成函数
export default {
 'post /api/users/create': (req, res) => {
     res.send('ok');      // 发送数据
 }
}

2，模拟延时
import { delay } from 'roadhog-api-doc';
export default delay(
	{  和上面写的一样  },     // 接口
	2000,  //时间
)

3，登录验证，判断管理员
export default {
	'POST /umi/login': (req, res) => {
        const { username, password } = req.body;
        if (username==='alex'&& 	password === 'alex123') {
            res.send({
				err: 0,
				msg: '成功'
                currentAuthority:  'user'
            });
        } else if (username ===  'admin' && password === 'admin123') {
            res.send({
				err: 0,
				msg: '成功'
                currentAuthority:  'admin'
            });
        } else {
            res.send({
				err: 1,
				msg: '失败'
            });
        }
    },
};

4，模拟数据增删改查
// 增加
export default {
	'POST /umi/login': (req, res) => {
        console.log(req.body);
        res.send(
        Mock.mock({
            'data|1': [
                {
                    code: 0,
                    data: { ...req.body, a: 2 },
                    msg: '成功'，
                }，
                {
                	code: 1,
                	msg: '失败'
                }，
            ]，
        }).data，
      )；
    }，
    // 删除
    'DELETE /umi/login': (req, res) => {
        console.log(req.params.id);     // 拿到删除的id
        res.send(
        Mock.mock({
            'data|1': [
                {
                    code: 0,
                    data: { task_id: 123 },    // 删除数据
                    msg: '成功'，
                }，
                {
                	code: 1,
                	msg: '失败'
                }，
            ]，
        }).data，
    );
    }
    // 改
    'PATCH /umi/list/:id': (req, res) => {
        console.log(req.body);     // 修改的数据
        res.send(
        Mock.mock({
            'data|1': [
                {
                    code: 0,
                    data: { ...req.body },  // 修改后的数据和原本的数据进行一个合并
                    msg: '成功'，
                }，
                {
                	code: 1,
                	msg: '失败'
                }，
            ]，
        }).data，
    );
    }
}
```

### 数据模拟 json-server

第三方

新建jsonserver， jsonserver下的db.js

###  fetch请求

```js
// fetch，js自带
const login = async () => {
    let res = await fetch('/umi/login', {
        method: 'post',
        // header: {}     原生fetch不会携带默认的请求头，需要手动添加
        body: 'username=alex&password=alex123'
    });
    let data = await res.json();
    console.log('fetch login', data)
}
```

### umi-request请求

```js
import { request } from 'umi'
// 用法 request(url, options)   ~~axios.get(url,options)    res => res.data
const Goods = async () => {
    let res = await request('/umi/goods')
    console.log(res)
}
// 带参数
const Login = async () => {
    let res = await request('/umi/login', {
        method: 'post',
        data: {
            username: 'zs',
            password: 'admin123',
        }
    })
    console.log(res)
}
```

### umi-useRequest请求

```js
// 必须返回一个data字段，如果没有，可能会产生拿不到数据的情况
// 可以在config文件中，配置request: { darafield: '', }  设置为空，这样就不会受到是否有data字段的影响。
使用useRequest
import { useRequest } from 'umi'
// umi-mock
const { data, error, loading } = useRequest('/umi/goods')
// data: 拿到的数据， error: 产生的错误， loading: 加载中

// 线上接口
const { data, error, loading } = useRequest( url )
// Access-Control-Allow-Origin 跨域
/* 在config 中引入一个代理的模块  proxy.js 在这个文件中配置代理内容
*/
// 参数 json-server
const { data, error, loading, run1, run2 } = useRequest({
    url: '/umi/login',
    method: 'post',
    data: {
        username: 'alex',
        password: 'alex123',
    }
},
// umi-useRequest 可以跟第二个参数
{ manual: true }  // 手动通过运行run触发   通过调用run1来触发请求
})

// 轮询
const { data, loading, error } = useRequest(
	(_limit) => ({       // 接收参数， 也可以直接写成对象 { }
        url: '',
        params: { _limit },
    }),
    {
        pollingInterval: 1000,   // 轮询一秒读一次
        pollingWhenHidden: false, // 屏幕不可见时，暂停轮询
    },
)
```

### dva

dva = React-Router + Redux + Redux-saga

- 路由： [React-Router](https://github.com/ReactTraining/react-router/tree/v2.8.1)
- 架构： [Redux](https://github.com/reactjs/redux)
- 异步操作： [Redux-saga](https://github.com/yelouafi/redux-saga)

状态管理。dva是一个基于 [redux](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Freduxjs%2Fredux) 和 [redux-saga](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fredux-saga%2Fredux-saga) 的数据流方案。

dva 内置fetch，react-router，也可说是一个轻量级框架

早期dva本身是一个轻量级框架，现在dva当作插件被  umi框架  已经内置了

|                                |  redux   |     dva      |
| :----------------------------: | :------: | :----------: |
|            状态数据            |  state   |    state     |
|            行为描述            |  action  |    action    |
|      同步的、无副作用业务      | reducer  |   reducer    |
| 异步的、有副作用（定时器）业务 | creators |    effect    |
|      通讯请求修改状态函数      | dispatch |   dispatch   |
|      通讯请求获取状态函数      | connect  |   connect    |
|          从源获取数据          |    无    | subscription |

##### 数据流向

![image-20231213114316919](D:\webstorm\typora\images\image-20231213114316919.png)

##### 使用：全局局部

```js
// 全局的数据
在 src 下的models文件夹
这个文件夹下的所有js文件都会认为是公共的全局的数据
//  1，建立对应的模块。例：global.js   对外暴露一个对象
export default {
    namespace: '文件名即可'，  // 不允许重名
    state: {				// 初始化全局数据
    	title: '',
    	text: '',
    	login: false,
    	a: '全局'
	},
    reducers: {				// reducers 处理同步业务
        setText(state, action) {     // action 接收的参数，action.payload
            // 更新并返回修改后的数据
            return {
                ...state,
                text: '',
            }
        }
    },
    effects: {				// effects 处理异步业务   状态机函数？？？
        *login( action, { call, put, select }) {      // action.payload 接收的参数，可以解构 { payload }
            const data = yield call(request, { payload });   // call() 触发异步请求 select() 从上面的公共仓库获取
            yield put({ type: '', payload: data })			//  put() 异步取回数据后，调用reducer同步修改数据，数据
        }                                                   // yield 类似于await
    }
}
// 2，在组件中使用
导入 connect
import { connect } from 'umi'
在暴露组件时，对组件进行修饰
const 组件名(props) {}   // props 接收state
export default connect( (state) => ({
    // 取全局， 重命名属性名
    text: state.golbal.text,
}) )( 组件名 );

// 3，dispatch 触发
props.dispatch({
    type: '模块名(命名空间) / 某一个方法'，
    payload: {},  // 参数会在函数的 action.payload 中
})
```

```js
// 局部的数据
如果页面的数据不多，可以建一个页面独享的数据，在pages当前页面的model.js
如果页面数据很多，也可以建一个models文件夹，存放多个模块数据
当前的页面是可以向上访问全局数据的，但不能向下访问。
// model.js中
对外暴露一个对象
export default {
    namespace: '',
    state: {},
    reducers: {},
    effects: {},
}
// 组件中使用
export default connect( (state) => ({
    // 取局部model
    namespace: state.golbal.text,
}) )( 组件名 );
```

```js
// connect 把model和component 连接起来
// dva 提供了 connect 方法。如果你熟悉 redux，这个 connect 就是 react-redux 的 connect
例：
export default connect(({ products }) => ({
  products,
}))(Products);

@connect语法糖
import { connect } from 'dva';

function mapStateToProps(state) {
  return { todos: state.todos };
}
connect(mapStateToProps)(App);
// connect 方法返回的也是一个 React 组件，通常称为容器组件。因为它是原始 UI 组件的容器，即在外面包了一层 State。
// connect 方法传入的第一个参数是 mapStateToProps 函数，mapStateToProps 函数会返回一个对象，用于建立 State 到 Props 的映射关系
```

```js
// 丢弃connect
如果是子组件中也使用数据，不必用connect
1，获取dispatch
import { useDispatch, useSelector } from 'umi'
const dispatch = useDispatch();        // 用于触发同步或异步来修改数据
2, 获取数据
const { namespace.state } = useSelector( () => ({ namespace }) )
```

```js
// subscriptions
定义数据源的变化，
收纳一些自定义函数
fn({ dispatch, history }) { // 业务逻辑 }
```

##### action

> 是一个js对象。改变state的唯一途径。
>
> action必须带type属性指明行为，发起action需用dispatch函数
>
> 需要注意的是 `dispatch` 是在组件 connect Models以后，通过 props 传入的。

```js
dispatch({
  type: 'add',
});
```

##### dispatch 函数

>  dipatch 可以看作是触发这个行为的方式，而 Reducer 则是描述如何改变数据的。

```js
dispatch({
  type: 'user/add', // 如果在 model 外调用，需要添加 namespace
  payload: {}, // 需要传递的信息
});
```

##### Effect

> Action 处理器，处理异步动作，基于 Redux-saga 实现。Effect 指的是副作用。根据函数式编程，计算以外的操作都属于 Effect，典型的就是 I/O 操作、数据库读写。
>
> Effect 是一个 Generator 函数，内部使用 yield 关键字，标识每一步的操作（不管是异步或同步）。
>
> dva 提供多个 effect 函数内部的处理函数，比较常用的是 `call` 和 `put`。
>
> - call：执行异步函数
> - put：发出一个 Action，类似于 dispatch

```js
function *addAfter1Second(action, { put, call }) {
  yield call(delay, 1000);
  yield put({ type: 'add' });
}
```



##### dva的几个规则:

1、通过dispatch调用namespace/effects
2、state(状态)
3、effects (异步操作)

\- 函数必须带*，也就是生成器。
\- 第一个参数，可以拓展为{payload, callback}
\- 第二个参数，call和put
\- call 就是调用 async的action函数
\- put就是调用reducers的函数来更新state。

4、reducers

5、dva是以model为单位的，所有的应用逻辑都在上面

### 运行时配置

1. 渲染前的权限校验



2. 动态路由读取、添加



3. 路由监听，埋点统计



4. 拦截器

### 组件权限

配置开启。同时需要 `src/access.ts` 提供权限配置。

```js
export default {
  access: {},
  // access 插件依赖 initial State 所以需要同时开启
  initialState: {},
};

// 我们约定了 src/access.ts 为我们的权限定义文件，该文件需要默认导出一个方法，导出的方法会在项目初始化时被执行。该方法需要返回一个对象，对象的每一个值就对应定义了一条权限。如下所示：
// src/access.ts
export default function (initialState) {
  const { userId, role } = initialState;

  return {
    canReadFoo: true,
    canUpdateFoo: role === 'admin',
    canDeleteFoo: (foo) => {
      return foo.ownerId === userId;
    },
  };
}
```

##### API

```js
useAccess
//我们提供了一个 Hooks 用于在组件中获取权限相关信息，如下所示
import React from 'react';
import { useAccess } from 'umi';

const PageA = (props) => {
  const { foo } = props;
  const access = useAccess();

  if (access.canReadFoo) {
    // 如果可以读取 Foo，则...
  }

  return <>TODO</>;
};

export default PageA;
```

```js
// 配合 Access 组件可以很简单的实现页面内的元素的权限控制。
import React from 'react';
import { useAccess, Access } from 'umi';

const PageA = (props) => {
  const { foo } = props;
  const access = useAccess(); // access 的成员: canReadFoo, canUpdateFoo, canDeleteFoo

  if (access.canReadFoo) {
    // 如果可以读取 Foo，则...
  }

  return (
    <div>
      <Access
        accessible={access.canReadFoo}         // 是否有权限，通常通过 useAccess 获取后传入进来。
        fallback={<div>Can not read foo content.</div> }       / //无权限时的显示，默认无权限不显示任何内容。
      >
        Foo content.
      </Access>
    </div>
  );
};
```

```js
// optimus-ui/access.js
export default initialState => {
  const { initFunctions, currentUser } = initialState;

  return {
    /*
      routePath is not required, if not provided, the key will be used as pathname
      when routePath has variable ex: /xxx/xxx/:externalRef, routePath must be provided

      parent Component ex:
        import { useRouteMatch } from 'umi'
        const { path } = useRouteMatch();
        checkPermission(key, path))
    */
    checkPermission: (key, routePath = undefined) => {
      const path = routePath || window.location.pathname;
      const groups = currentUser['cognito:groups'] || [];
      if (groups.includes('Administrator') || groups.includes('admin')) {
        return true;
      }
      if (initFunctions instanceof Map) {
        const keys = initFunctions.get(path);
        if (Array.isArray(keys) && keys.includes(key)) return true;
        return false;
      }
      return false;
    },
  };
};
```
