# VUE-VUEX

> https://vuex.vuejs.org/zh/

为 Vue.js 应用程序开发的**状态管理模式**

集中管理各个组件的公共数据（状态），并保证这些数据的变化过程可追踪

## Store模式

如果应用不是很大，更推荐使用[Store模式](https://cn.vuejs.org/v2/guide/state-management.html#%E7%AE%80%E5%8D%95%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86%E8%B5%B7%E6%AD%A5%E4%BD%BF%E7%94%A8)，这是类 Flux 状态管理的实现

<img src="https://github.com/wangning0/Autumn_Ning_Blog/blob/master/blogs/2016-1-3/flux/pt/flux.png?raw=true" alt="flux.png" style="zoom:50%;" />

Flux的核心思想就是**数据和逻辑永远单向流动**

+ View获取Store的保存的数据，渲染各个页面
+ View需要更改数据时，调用Dispatcher声明的Action
+ Dispatcher将Action分发给Store
+ Store根据Action内容，更新自身完成数据修改

视图层不允许直接修改状态，由状态受到事件自我更新，保证**全过程可以被记录和回溯**

## 安装

+ 直接下载 / CDN 引用
+ NPM / Yarn 下载
+ `@vue/cli`工具新建项目时选择添加

## Store（仓库）

store 是一个保存各个组件的公共状态的容器，有两点不同于全局对象：

+ 响应式：store变化后，view也随之变化
+ 不能直接被修改，只能通过显式**提交变化(commit mutation)**

## 注入所有子组件

首先注入所有子组件，以供子组件使用

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

## State

### 用处

view中`state`用来获得store中的数据

```js
computed: {
    count () {
      return this.$store.state.count
    }
  }
```

### 辅助函数`mapState()`

用来批量获取数据，映射给本作用域的同名属性

```js
computed: {
  ...mapState([
    "count",
    "count2" 	
  ])
}
```

## Getters

### 用处

在store中的`getters`中声明函数，函数返回**经过过滤的状态或处理后得到的数据**，可以传参

暴露为`store.getters`对象

```js
getters: {
    //返回todos中属性为true的todo项
    doneTodos: state => {
      return state.todos.filter(todo => todo.done) 
    },
    //实现传参
    getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
  	},
    getTodosNum: state => {
        return store.getters.doneTodosCount
    }
  }
```

view中通过`getters`获取这些状态或数据

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

### 辅助函数`mapGetters()`

```js
 computed: {
  // 使用对象展开运算符将 getter 混入 computed 原有对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
```

## Mutation

### 用处

在store中的`mutations`中注册事件

每个事件拥有自己的名称（用于指示如何修改字符串）、接收state和用于更改state状态的回调函数

```js
mutations: {
    increment (state) {
      // 变更状态
      state.count++
    },
    increment2 (state, n) {
      state.count += n
  	},
    increment3 (state, payload) {
      state.count += payload.num
  	},
  }
```

### 不能直接调用

这些注册事件**不能被直接调用**，通过调用 `store.commit(mutations-name)`函数使用它们

以下是四种调用形式，需要传入多个参数时，将它们作为一个整体（对象或数组）传入，此时该整体称之为载荷（payload)

```js
store.commit('increment');			// 直接调用
store.commit('increment2',11)		// 附加参数
store.commit('increment3',payload)	// 附加对象参数，用于多个参数
store.commit({						// 对象形式调用
  type: 'increment',
  amount: 10
})
```

<span style="color:red;">**注意**：Mutation 必须是同步函数</span>

### 辅助函数`mapMutations()`

```js
methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`
      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
```

## Action

### 用处

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态
- Action 可以**包含任意异步操作**

```js
mutations: {
  increment (state) {
    state.count++
  }
},
actions: {
  increment (context) {
    context.commit('increment')
  }
}
```

使用参数解构

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

通过`dispatch`方法分发，函数的四种用法和[commit方法](###不能直接调用)一样

```js
store.dispatch('increment')
```

### 辅助函数`mapActions()`

```js
methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
```