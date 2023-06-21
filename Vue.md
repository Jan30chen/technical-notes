# VUE

> 基于MVVM模式，用以开发由数据驱动、组件化的、双向数据绑定的单页应用程序的渐进式框架

开发单页应用程序

组件化开发

双向数据绑定

MVVM通过数据双向绑定让数据自动地双向同步

渐进式框架

运用虚拟DOM技术

## 对于MVVM的理解

> MVVM是 Model、View、View Model 的缩写

+ Model：数据层
+ View：用户操作页面
+ View Model ：业务逻辑层
  + 提供View需要的数据，响应View要求的操作
  + 实现View与Model的双向绑定

## 常用指令

![](D:\坚果云\MD笔记\MDimg\0b46ec8b051246858211c4c7ec129fb3~tplv-k3u1fbpfcp-zoom-in-crop-mark_4536_0_0_0.webp)

+ `v-model`：实现表单控件和数据的双向绑定
+ `v-html`：更新元素的`innerHtml`
+ `v-show`：条件渲染，决定元素是否可见（元素一定存在）
+ `v-if`：条件渲染，决定元素是否存在（懒渲染，为True时才渲染，元素不一定存在）
+ `v-for`：循环渲染
+ `v-on:<event>/@<event>`：绑定一个事件，触发事件后执行该事件的处理函数
+ `v-blid:<prop>/:<prop>`：动态地绑定一个或多个特性，或一个组件 prop 到元素

## 常用事件

+ `@click`：点击事件
+ `@dblclick`：双击事件

## 修饰符

> 参考：https://segmentfault.com/a/1190000016786254#item-2

使用`.xxx`的形式修饰 VUE 常用指令

- 表单修饰符
- 事件修饰符
- 鼠标按键修饰符
- 键值修饰符
- v-bind修饰符

#### v-model 表单修饰符

##### `.lazy`

正常情况是输入时实时更新数据，添加该修饰符使输入框失去焦点之后再更新数据

##### `.trim`

去掉输入框左右的空格，中间的不会被去除

##### `.number`

将输入内容解析为数字类型更新

以数字开头，之后再输入字母，字母不被记录；但是以字母开头，该属性失效

#### v-on 事件修饰符

完整的事件机制是：捕获阶段->目标阶段->冒泡阶段；事件冒泡默认从子组件向上到祖先元素向上冒泡

##### `.stop`

阻止事件冒泡

##### `.prevent`

阻止事件的默认行为；比如阻止点击按钮提交表单

##### `.self`

绑定事件的元素本身触发事件才触发成功，子元素触发无效

##### `.once`

只触发一次事件

##### `.capture`

从祖先到元素本身反方向冒泡事件

#### 鼠标按钮修饰符

- `@click.left`： 左键点击
- `@click.right` ：右键点击
- `@click.middle`：中键点击

#### 键盘修饰符

用法：`@keyup.keyCode`

key Code为对应键码，或者可以为别名

```
//	别名-普通键
.enter 
.tab
.delete //(捕获“删除”和“退格”键)
.space
.esc
.up
.down
.left
.right
```

```
//	别名-系统修饰键，需要和其他键搭配使用，单独不成立
.ctrl
.alt
.shift
.meta // 菜单键
```

## 生命周期

### 总览

创建前	>	创建后	>	载入前	>	载入后	>	更新前	>	更新后	>	销毁前	>	销毁后

### 钩子函数

>  钩子（hook）就是一些具有既定生命周期的框架工具，在生命周期的各个阶段预留给用户执行一些特定操作的口子，这其实是一种**面向切面编程** 

#### `beforeCreate()`

挂载元素`$el`和`data`都是`undefined`

#### `created()`

`data`初始化

#### `beforeMount()`

`$el`初始化，首次调用相关`render`函数生成组件模板

编译模板并根据`data`生成html，但是html还没有被挂载

> `render()`函数：绝大多数情况下使用模板来创建你的 HTML。然而在一些场景中需要 JavaScript 的完全编程的能力。这时你可以用**渲染函数**，它比模板更接近编译器。

#### `mounted()`

*`el`被`vm.$el`替换，挂载在实例上**后**使用*

用上阶段编译好的html文件替换指向的DOM对象

此过程中进行ajax操作

#### `beforeUpdate()`

*数据更新前调用*

可以更改状态，不会触发重渲染过程

#### `updated()`

*数据更新后，导致虚拟DOM重新渲染和打补丁后调用*

此时DOM已经更新，可以执行依赖于DOM的操作

但是应该尽量避免此阶段更改状态，避免更新无限循环

该钩子函数在服务器渲染期间不被调用

#### `beforeDestroy()`

*实例销毁前调用*

此时实例依然可用

#### `destroyed()`

*实例销毁后调用*

移除所有事件监听器、子实例

该钩子函数在服务器渲染期间不被调用

#### `activated()`

已被缓存的`<keep-active>`组件激活时调用，创建首次也会触发

`<keep-active>`组件的生命周期：

+ 首次进入组件：`beforeRouteEnter` > `beforeCreate` > `created` > `mounted` > **`activated`**  > ... ... > `beforeRouteLeave` > **`deactivated`**
+ 再次进入组件：`beforeRouteEnter` > **`activated`** > ... ... > `beforeRouteLeave` > **`deactivated`**

#### `deactivated()`

离开`<keep-active>`组件时调用

## 常用的数据

### `data`

储存本页面的数据，注意使用`return`返回对象的形式，不然复用组件会出问题

### `props`

页面不会有，但是组件基本都会有，主要用于传数据

### `watch`

监视页面某一个值的变化后进行操作，可以写成函数放在这里；函数名与被监视变量名相同，函数的参数分别为**变化后和变化前的值**；比起`computed`更适合做更复杂的操作（如异步），适用于一个数据变化会导致多个操作的情况

### ` computed ` 

对于某个变量需要经过一定程度的处理而返回，可以写成函数放在这里；实质是一个变量通过`get`函数处理后返回，也可以同时写出`get`和`set`函数，一个获取该变量，一个执行该变量被赋值时的操作；适用于多处操作会导致该数据变化的情况

### `methods`

`methods` 将被混入到 VUE 实例中，使得整个页面可以调用：可以直接通过 VM 实例访问这些方法，或者在**指令表达式中使用**，**方法中的 this 自动绑定为 VUE 实例**；`click`事件、`change`事件都放在这里

### `components`

引入组件的时候，要在这里声明

### `beforeRouteEnter`

在页面进入前要执行的一些操作放在这里，比如请求数据 

### `filters`

过滤器，用来将一些值格式化输出显示

## VUE 双向绑定数据、响应式原理

#### view 更新 data：事件监听

如input输入，监听input即可

#### data 更新 view：数据劫持 + 发布者-订阅者模式

数据劫持：

通过`Object.defineProperty()`修改对象本身自带的`get()`与`set()`方法，在其中加入发布者

发布者订阅模式：

多个订阅者订阅发布者，在发布者发布时（即对象数据发生变化的时候）执行相应的操作更新视图

### 组件间传值通信

#### `props` 和 `$emit`

`props` 以**单向**数据流的形式完成**父子组件**的通信 

即父组件使用`props` 将数据流向子组件，子组件的数据不能流回父组件

子组件通过`$emit`调用父组件的函数传值

```js
// 父组件
VUE.component('parent', {
  template:`
    <div>
      <p>this is parent component!</p>
      <child :message="message" v-on:getChildData="getChildData"></child>
    </div>
  `,
  data() {
    return {
      message: 'hello'
    }
  },
  methods:{
    // 执行子组件触发的事件
    getChildData(val) {
      console.log(val);
    }
  }
});

// 子组件
VUE.component('child', {
  template:`
    <div>
      <input type="text" v-model="myMessage" @input="passData(myMessage)">
    </div>
  `,
  /**
   * 得到父组件传递过来的数据
   * 这里的定义最好是写成数据校验的形式，免得得到的数据是我们意料之外的
   *
   * props: {
   *   message: {
   *     type: String,
   *     default: ''
   *   }
   * }
   *
  */
  props:['message'], 
  data() {
    return {
      // 这里是必要的，因为你不能直接修改 props 的值
      myMessage: this.message
    }
  },
  methods:{
    passData(val) {
      // 数据状态变化时触发父组件中的事件
      this.$emit('getChildData', val);
    }
  }
});
    
var app=new VUE({
  el: '#app',
  template: `
    <div>
      <parent />
    </div>
});

```

## 深度作用选择器

> 参考：https://vue-loader.vuejs.org/zh/guide/scoped-css.html
>
> https://www.cnblogs.com/CyLee/p/10006065.html

### 原因

为样式文件添加`scoped`关键词之后，编译的`vue`组件将每个元素添加`[data-v-xxx]`属性，同时，样式也随之添加对应属性，确保样式被局限于这些元素中；但是对于引入组件，`data-v`属性只会被添加**在第一层**上

所以当样式被添加`scoped`关键词后，是不能够直接对引入组件的样式内部进行修改的

例子：内部的`weui-cells`元素不被选中

```html
<!-- 编译后的html文件 -->
<div data-v-xxx class="outside">
    <!-- 组件中元素 -->
    <div class="weui-cells"></div>
</div>
```

```less
// 编译前
<style scoped>
.outside .weui-cells {
    // ...
}
</style>
// 编译后
<style>
.outside[data-v-xxx] .weui-cells[data-v-xxx]{ //... }
</style>
```

### 解决

使用深度作用选择器：

+ `>>>`：适合CSS文件，对预编译器可能不起作用（less等）
+ `/deep/`：适用预编译器，上一条的别名
+ `::v-deep`：同上一条

```less
// 编译前
<style scoped>
.outside /deep/ .weui-cells {
    // ...
}
</style>
// 编译后
<style>
.outside[data-v-xxx] .weui-cells{ //... }
</style>
```

通过 `v-html` 创建的 DOM 内容不受`scoped`样式影响，但是依然可以通过深度作用选择器来设置样式

## 过滤器

### 说明

过滤器用以对原始数据处理并返回

### 使用

html中使用：双花括号插值、`v-bind`表达式中，使用`|`符号置于原始数据后

```html
<!-- 文本过滤 -->
<p>{{ message | filterA }}</p>
<!-- 属性过滤 -->
<p :id='rawId | filterB'>text</p>
<!-- 多重过滤 -->
<p>{{ message | filterA | filterB }}</p>
<!-- 带有参数的过滤，实际过滤器定义时，参数顺序是(rawText, arg1, arg2) -->
<p>{{ message | filterA(arg1, arg2) }}</p>
```

methods中使用：可以使用全局定义的过滤器

```js
this.$option.filters['filterA'](value)
```

### 创建

+ 于组件的选项中创建过滤器，局部过滤器优先度更高

```js
// 首字母大写的过滤器
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

+ 于全局创建过滤器

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```

## 全局API/实例方法

> `Vue.set`：全局的方法
>
> `vm.$set`：Vue所构造的实例vm上的实例方法

#### [set()](https://v2.cn.vuejs.org/v2/api/#Vue-set)与[delete()](https://v2.cn.vuejs.org/v2/api/#Vue-delete)

操作响应式对象中的属性，保证可以触发视图更新

```
Vue.set( target, propertyName/index, value )
Vue.delete( target, propertyName/index )
```

#### [forceUpdate()](https://v2.cn.vuejs.org/v2/api/#vm-forceUpdate)

迫使Vue实例重新渲染

#### [nextTick()](https://cn.vuejs.org/api/general.html#nexttick)

在下次 DOM 更新循环结束之后执行延迟回调；用于确保获取最新的DOM

```typescript
$nextTick(callback?: (this: ComponentPublicInstance) => void): Promise<void>
```

与全局`this.$nextTick()`的区别：组件传递给 `this.$nextTick()` 的回调函数会带上 `this` 上下文，其绑定了当前组件实例

#### [emit()](https://cn.vuejs.org/api/component-instance.html#emit)

在当前组件触发一个自定义事件。可以附加任意数量额外的参数

```
$emit(event: string, ...args: any[]): void
```

##### `.sync`与`$emit(update:xxx)`

> 参考：[Vue .sync修饰符与$emit(update:xxx)](https://www.cnblogs.com/wjw1014/p/13785934.html)

实现父子组件中变量的双向绑定

```vue
<!-- 子组件中 -->
this.$emit('update:message',valc);
<!-- 父组件中 -->
<comp :message.sync="bar"></comp>
```

**注意**：父组件绑定时一律使用**短横线**写法，子组件使用**驼峰**写法接收和使用

```vue
<!-- 子组件中 -->
this.$emit('update:father-num',100);  //有效
//this.$emit('update:fatherNum',100); // 无效
<!-- 父组件中 -->
<father v-bind:father-num="test" v-on:update:father-num="test=$event" ></father>
```

## API

+ 全局配置
+ 全局API
+ 选项
  + 数据
  + DOM
  + 生命周期钩子
  + 资源
  + 组合
  + 其他
+ 实例方法
  + property
  + 数据
  + 事件
  + 生命周期
+ 指令
+ 特殊attribute
+ 内置组件

## 其他

### 组件的`name`属性

> 参考：https://blog.csdn.net/weixin_39015132/article/details/83573896

作用有三种：

+ 可以递归调用自身
+ 方便在`devtool`寻找该组件
+ 使用`keep-alive`功能时，使用`includes`与`excludes`指定该属性以启用/禁用

### `process.env.NODE_ENV`

> 参考：[一文弄懂如何在 Vue 中配置 process.env.NODE_ENV](https://juejin.cn/post/6844904069136416781)

> process /ˈprəʊses/  *n.* 进程
>
> development /dɪˈveləpmənt/ *n.* 开发
>
> production*/*prəˈdʌkʃ(ə)n*/* *n.* 生产
>
> environment*/*ɪnˈvaɪrənmənt*/* *n.* 环境

`vue.config.js`中的变量`process.env.NODE_ENV`

该变量决定应用运行的模式（开发、生产或测试），因此决定创建何种`webpack`配置

#### Node.js中

`process.env`是`Node.js`中的环境对象，指示当前系统的环境变量

> 在window平台，即是 我的电脑 > 右键菜单属性 > 高级系统设置 > 环境变量 处所设置的

`NODE_ENV`为其中的变量值，指示`node`当前的环境（生产/开发），默认为空

#### `Vue`中

除了在系统中配置，`Vue`项目提供了自身的配置，详见`Vue CLI`中的[模式和环境变量](https://cli.vuejs.org/zh/guide/mode-and-env.html)

##### 默认

默认情况下，使用`Vue CLI`生成的项目有三个模式：

```bash
# development模式
vue-cli-service serve
# test模式
vue-cli-service test
# production模式
vue-cli-service build
vue-cli-service test:e2e
```

可以使用`--mode`选项，覆盖默认的模式

```bash
# 于development模式构建
vue-cli-service build --mode development
```

如果没有配置`.env`文件，此时**`NODE_ENV`等于模式值**

---

注：通常上述的命令，会在`package.json`中被重写

```json
"scripts": {
	"serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
}
```

##### `.env`文件

可以在根目录中放置`.env`文件来指定环境变量，也包括`NODE_ENV`变量

+ `.env`：在全部环境中载入
+ `.env.[mode]`：在指定环境中载入，优先级比`.env`要高

注：添加`.local`后缀，会使`git`忽略该文件，以使文件只存在于本地

`.env`文件中只包含环境变量的键值对

```
FOO=bar
VUE_APP_NOT_SECRET_CODE=some_value
```

注意：<u>只有 `NODE_ENV`，`BASE_URL` 和以 `VUE_APP_` 开头的变量</u>将通过 `webpack.DefinePlugin` 静态地嵌入到*客户端侧*的代码中

+  `NODE_ENV`
+ ``BASE_URL` ：等于`vue.config.js`中的`publicPath`属性

这些客户端环境变量可以在`public/index.html`中通过[HTML插值](https://cli.vuejs.org/zh/guide/html-and-static-assets.html#%E6%8F%92%E5%80%BC)的方式使用

```html
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
```

### 数据变化视图未更新

> 参考：[Vue 数据更新了但页面没有更新的 7 种情况汇总及延伸总结](https://segmentfault.com/a/1190000022772025)

以下情况，不会触发视图更新：

+ 实例创建时，`data`没有初始化该`property`
+ 对象`property`的添加和删除
  + 使用`vm.$set`、`vm.delete`修改属性，或对该对象整个赋新值（`Object.assign()`）
+ 通过数组索引修改数组项
  + 使用`vm.$set`、`vm.delete`，或`splice()`方法
  + `Object.defineProperty()`可以检测数组的变化，但检测不到删除操作，也不能检测到新增的元素（下标）
+ 直接修改数组长度
  + 使用`splice()`方法
+ 异步更新前，操作DOM：`Vue`更新DOM为**异步执行**，同一个事件循环中的数据变更将被去重；所以赋值后直接获取DOM属性，会获取未变更的值
  + 在`vm.$nextTick()`方法中去获取
+ 循环嵌套层级太深（待定）
+ 路由参数变化时，页面不更新：引用相同组件时（如动态路由），路由`param`变化时组件不更新
  + 使用`watch`监听`$route`，参见[响应路由参数的变化](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#%E5%93%8D%E5%BA%94%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0%E7%9A%84%E5%8F%98%E5%8C%96)
  + 为`<router-view>`绑定`key`属性
