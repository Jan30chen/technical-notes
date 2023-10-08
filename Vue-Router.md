# Vue-Router

作用是将组件映射到路由上

+ 使用`<router-view>`组件确定组件渲染的位置，如`app.vue`文件中
+ 使用`<router-link>`组件创建链接以代替`<a>`标签，这可以在不重新加载页面的情况下更改URL、处理URL的生成和编码

## Router方法

+ [push](https://router.vuejs.org/zh/api/#push)：向历史堆栈中推送，导航到一个新的url
+ [go](https://router.vuejs.org/zh/api/#go)：在历史中前进或后退
+ [resolve](https://router.vuejs.org/zh/api/#resolve)：返回路由地址的标准化版本，含有`href`属性

## History&Hash 模式

> 参考：https://segmentfault.com/q/1010000010340823

VUE-router是一种前端路由，为了构建SPA（单页面应用），需要**在改变视图时，不向后端发出请求**。目前浏览器提供两种模式实现：

+ hash：为地址栏中的**`#`符号**
  + 特点：hash存在于URL中，但不会被包括在Http请求中，不影响后端，改变hash**不会重载页面**
+ history：借助HTML5 History Interface 中新增的 `pushState()` 和 `replaceState()` 方法
  + 两个方法提供了对历史记录进行修改的功能，需要特定浏览器支持
  + 它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求

### 模式比较

调用 `history.pushState()` 相比于直接修改 `hash`，存在以下优势：

+ `pushState()` 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 `hash` 只可修改 `#` 后面的部分，因此只能设置与当前 URL 同文档的 URL；
+ `pushState()` 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 `hash` 设置的新值必须与原来不一样才会触发动作将记录添加到栈中；
+ `pushState()` 通过 `stateObject` 参数可以添加任意类型的数据到记录中；而 `hash` 只可添加短字符串
+ `pushState()` 可额外设置 `title` 属性供后续使用。
+ history模式下的URL更加美观

当然啦，`history` 也不是样样都好。SPA 虽然在浏览器里游刃有余，但真要通过 URL 向后端发起 HTTP 请求时，两者的差异就来了。尤其在用户手动输入 URL 后回车，或者刷新（重启）浏览器的时候。

1. `hash` 模式下，仅 `hash` 符号之前的内容会被包含在请求中，如 `http://www.abc.com`，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。
2. `history` 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 `http://www.abc.com/book/id`。如果后端缺少对 `/book/id` 的路由处理，将返回 404 错误

小结：

结合自身例子，对于一般的 **VUE + VUE-Router + webpack + XXX** 形式的 Web 开发场景，用 `history` 模式即可，只需在后端（Apache 或 Nginx）进行简单的路由配置，同时搭配前端路由的 404 页面支持。

## 动态路由

匹配动态路径，路径的一部分为不定值（变量名），会被赋值为相应变量：

动态字段以冒号开始，下面的例子中`id`为变量；组件中通过`this.$route.params.id`的形式暴露

```js
const routes = [{ path: '/users/:id', component: User }]
```

譬如`/users/johnny` 和 `/users/jolyne`都会匹配到上述路由的范畴

注意：相同动态路由但参数不同的路由跳转，**相同的组件实例被复用**，这会导致**组件的生命周期钩子不会被调用**

#### 所有

使用`*`匹配任意路径，常常用于匹配404页面；注意：此类匹配路由应该放置于最后

```js
{
  path: '/user-*',	// 会匹配以 `/user-` 开头的任意路径
  path: '*'	// 会匹配所有路径
}
```

#### 正则

可以在参数后的括号后，设置一个正则以附加条件，注意要在`\d`前使用**转义符`\`**

```js
const routes = [
  // /:orderId -> 仅匹配数字
  { path: '/:orderId(\\d+)' },
  // /:productName -> 匹配其他任何内容
  { path: '/:productName' },
]
```

#### 重复参数

使用`*`与`+`标记参数为可重复的，该参数会是**一个数组**

```js
const routes = [
  // 匹配至少一个，如/one, /one/two/three
  { path: '/:chapters+' },
  // 匹配任意数量 如/, /one, /one/two/three
  { path: '/:chapters*' },
  // 结合正则，匹配任意数量的数字，如/, /1, /1/2, 等
  { path: '/:chapters(\\d+)*' },
]
```

#### 可选参数

使用`?`将参数标记为可选，与`*`一样都可以匹配0~1个参数，但不能匹配更多

```js
const routes = [
  // 匹配 /users 和 /users/posva
  { path: '/users/:userId?' },
  // 匹配 /users 和 /users/42
  { path: '/users/:userId(\\d+)?' },
]
```

#### sensitive 与 strict 

> strict */*strɪkt*/* adj.严格的
>
> sensitive */*ˈsensətɪv*/* adj.敏感的

这两个属性是和`history`、`routes`属性同级的

+ sensitive：严格区分大小写；`/users/:id`将不匹配`/USERS/posval`
+ strict ：严格区分尾部斜线；`/users/:id`将不匹配`/users/posval/`

## 导航守卫

### 全局前置守卫

使用`router.beforeEach`注册一个全局前置守卫

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫resolve完之前一直处于等待中。

接受三个参数：

+ to: Route：即将进入的目标
+ form: Route：当前导航正要离开的路由
+ next: Function：一定要调用该方法来resolve这个钩子。执行效果依赖next方法的调用参数。
  + ''（空）：进行管道中的下一个钩子。如果全部执行完了，导航变为confirmed状态
  + false：中断当前的导航，重置为from地址
  + '/example'、{ path: '/example' }：跳转到一个不同的地址
  + error：为Error实例时，导航中断。错误被传递给`router.onError()`注册过的回调

确保`next`函数在一个导航守卫中都被**严格调用一次**

### 全局解析守卫

使用`route.beforeResolve`注册一个全局守卫（2.5.0+）

调用时机是，导航被确认<u>之前</u>，所有组件内守卫和异步路由组件被解析<u>之后</u>

### 全局后置钩子

使用`router.afterEach()`注册全局后置钩子

只接受`to`、`from`参数，不接受`next`函数也不会改变导航

### 路由独享的守卫

在路由配置上直接定义`beforeEnter`守卫，参数同全局前置守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

### 组件内的守卫

可以直接在组件中定义以下路由导航守卫：

+ `beforeRouteEnter`
  + 在路由变为`confirm`前调用
  + 此时**不能获取`this`**，因为组件实例还未创建
+ `beforeRouteUpdate`（2.2+）
  + 路由改变，但该组件被复用时调用；如动态参数路径：`/foo/1 => /foo/2`
+ `beforeRouteLeave`
  + 导航离开该路由时调用

`beforeRouteEnter`不可以访问`this`，但可以传一个回调给`next`，将组件实例作为回调方法的参数

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意：`beforeRouteEnter` 支持给 `next` 传递回调函数；而其他两个守卫不能传递回调给`next`，但可以传递路由或`false`

### 完整的导航流程

1. 导航被触发
2. 在失活的组件中调用`beforeRouteLeave`守卫（组件守卫）
3. 调用全局的`beforeEach`守卫（全局守卫）
4. 在重用的组件中调用`beforeRouteUpdate`（2.2+）（组件守卫）
5. 在路由配置里调用`beforeEnter`（路由守卫）
6. 解析异步路由组件
7. 在被激活的组件中调用`beforeRouteEnter`（组件守卫）
8. 调用全局的`beforeResolve`(2.5+)（全局守卫）
9. **导航被确认**，变为`confirm`状态
10. 调用全局的`afterEach`钩子（全局守卫）
11. 触发DOM更新
12. 调用`beforeRouteEnter`守卫中，传递给`next`的回调函数，参数为创建好的组件实例

## 路由对象属性

> 参见：https://v3.router.vuejs.org/zh/api/#%E8%B7%AF%E7%94%B1%E5%AF%B9%E8%B1%A1

`this.$route`：当前激活的路由信息对象，该属性（及其下列子属性成员）为**只读**，但可以进行监测（watch）

> `this.$router`为 router 实例，注意区分这两者

+ `$route.path`：字符串，当前路由的绝对路径
+ `$route.params`：对象，动态路由中获取动态片段与全匹配片段部分
+ `$route.query`：对象，查询参数集合
+ `$route.hash`：字符串，路由的定位`hash`值，带`#`
+ `$route.fullPath`：字符串，完整路径，包括绝对路径、查询参数与`hash`值
+ `$route.matched`：数组，包含当前路由<u>所有嵌套路径路径</u>的**路由记录**
+ `$route.name`：当前路由名称
+ `$route.redirectedForm`：如果存在重定向，则为重定向来源路由名

## 路由元信息

定义路由时可以配置`meta`字段，用来补充路由的信息，可以为对象

```js
{
	path: '/foo',
    component: Foo,
        children: [
            {
                path: 'bar',
                component: Bar,
                // a meta field
                meta: { requiresAuth: true }
            }
        ]
}
```

访问`meta`字段：

通过`$route.matched`，以数组形式访问所有匹配的嵌套路由；再从每个对象中得到`meta`属性

例：检查第一个`meta.requiresAuth`属性为真的路由，执行操作

```js
if (to.matched.some(record => record.meta.requiresAuth)) { ... }
```

## 过渡动效

`<router-view>`作为最基本的动态组件，可以使用`<transition>`组件给它添加一些过渡效果

```html
<!-- 添加切换效果 -->
<transition>
  <router-view />
</transition>
<!-- 使用name属性来设置不同的切换效果，也可以设置name为动态属性 -->
<transition name="slide">
  <router-view />
</transition>
```

## 数据获取

进入路由后，从服务器获取数据。有两种方法：

+ 导航完成**后**获取：在`created()`等生命周期函数中，请求服务器
+ 导航完成**前**获取：使用前置守卫，在`next()`函数中请求服务器。此时用户还留在旧页面

## 滚动行为

> 注意：该功能在支持[`history.pushState`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/pushState)的浏览器中使用

自定义路由切换时，页面滚动到什么位置

创建一个Router实例时，提供`scrollBehavior`方法：

```js
const router = new VueRouter({
    routes,
    scrollBehavior(to, from, savedPosition) {
        // return 滚动到的位置 
    }
})
```

`savePosition`方法：

+ 接收的参数
  + `to`：路由对象
  + `from`：路由对象
  + `savedPosition`：通过`popstate`导航（浏览器的 前进/后退 功能）时，该对象才可用

+ 返回值：位置对象信息

  + `{ x: number, y:number }`

  + `{ selector: string, offset? : { x: number, y: number }}`（offset只限于2.6.0+）

  + [`falsy`](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)值或空对象：不滚动

  + `savedPosition`：在按下 后退/前进 按钮时，就会像浏览器的原生表现那样

    ```js
    scrollBehavior (to, from, savedPosition) {
      if (savedPosition) {
        return savedPosition
      } else {
        return { x: 0, y: 0 }
      }
    }
    ```

  + 模拟“滚动到锚点”

    ```js
    scrollBehavior (to, from, savedPosition) {
      if (to.hash) {
        return {
          selector: to.hash
        }
      }
    }
    ```

### 异步滚动

> 2.8.0+

也可以返回一个Promise

```js
scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve({ x:0, y:0 }, 500)
        })
    })
}
```

### 平滑滚动

添加`behavior`选项至返回的位置对象中，启动原生平滑滚动：

```js
scrollBehavior(to, from, savedPosition) {
    if(to.hash) {
        return {
            selector: to.hash,
            behavior: 'smooth'
        }
    }
}
```

## 路由懒加载

异步加载组件，只有当访问到`path`时，才加载该组件：

```js
// AMD 异步
{
    path: '/home',
	component: resolve => require(['../home.vue'], resolve),
}    
// ES 异步
{
    path: '/home',
	component: import('../home.vue')
}  
```

`component`属性接受一个返回Promise组件的函数

### 组件按组分块

#### webpack

使用魔法注释`/* webpackChunkName: <module-name> */`，将指定的组件打包在同名异步块（chunk）中；

如下面的例子，这些路由将都会被打包到`group-user.chunk.js`中

```js
const UserDetails = () =>
  import(/* webpackChunkName: "group-user" */ './UserDetails.vue')
const UserDashboard = () =>
  import(/* webpackChunkName: "group-user" */ './UserDashboard.vue')
const UserProfileEdit = () =>
  import(/* webpackChunkName: "group-user" */ './UserProfileEdit.vue')
```

#### Vite

在`vite.config.js`中，定义`rollupOptions`分块

```js
export default defineConfig({
  build: {
    rollupOptions: {
      // https://rollupjs.org/guide/en/#outputmanualchunks
      output: {
        manualChunks: {
          'group-user': [
            './src/UserDetails',
            './src/UserDashboard',
            './src/UserProfileEdit',
          ],
        },
      },
    },
  },
})
```

## 导航故障

在使用`router-link`组件时，Vue Router 会自动调用`router.push`触发导航；使用`router-link`组件发生的导航失败，**不会打印出错误**

以下情况会导致导航失败，并使用户留在当前页面：

+ 用户已经位于目标路由
+ 导航守卫通过调用`next(false)`中断了导航
+ 导航守卫抛出了错误，或调用了`next(new Error())`

### 故障检测

*导航故障*是一个Error实例（但会有附加属性），用如下函数检测一个错误是否为导航故障：

+ `isNavigationFailure(failure, type)`
  + `failure`：待检测的错误
  + `type`：是否为某种错误，取值可以为`NavigationFailureType`对象的四种属性：
    + `redirected`：在导航守卫中调用了 `next(newLocation)` ，重定向到了其他地方。
    + `aborted`：在导航守卫中调用了 `next(false)` 中断了本次导航。
    + `cancelled`：在当前导航还没有完成之前又有了一个新的导航。比如，在等待导航守卫的过程中又调用了 `router.push`。
    + `duplicated`：已经位于目标路由，导航被阻止。

```js
import VueRouter from 'vue-router'
const { isNavigationFailure, NavigationFailureType } = VueRouter

// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    // 向用户显示一个小通知
    showToast('Login in order to access the admin panel')
  }
})
```

### 故障属性

如果为检测导航故障，则可以获取到 `to` 和 `from` 属性，表示失败的导航目的路径与当前路径，都是规范化的路由位置

```js
// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    failure.to.path // '/admin'
    failure.from.path // '/'
  }
})
```
