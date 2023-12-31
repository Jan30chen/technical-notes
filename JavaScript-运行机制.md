# JavaScript-运行机制

> 参考：https://juejin.im/post/6844904050543034376

## 进程与线程

进程：CPU资源分配的最小单位，一个可以独立运行且拥有自己的资源空间的**任务程序**

线程：CPU调度的最小单位，线程就是程序中的一个**执行流**，一个进程可以有多个线程，线程负责进程的不同功能

+ 当一个进程只有一个线程运行时，称为单线程进程；只有前面的程序运行完成后，才能继续后面的程序
+ 当一个进程包含多个运行线程时，称为多线程进程；多个线程并行执行，执行不同的任务

## 单线程的 JS 与多进程的浏览器

JS 作为浏览器脚本语言，多线程操作DOM会带来同步问题；

但是可以采取主线程控制多个子线程（Web Worker线程）的方法，提高多核CPU的利用率，但子线程不得操作DOM；

本质上 JS 还是属于单线程的范畴。

---

不同于 JS，浏览器是多进程的：

+ Browser进程：每个浏览器只有唯一一个
  + 是浏览器的**主进程**，负责协调、主控：
  + 负责浏览器界面显示，与用户交互。如前进，后退等
  + 负责各个页面的管理，创建和销毁其他进程
  + 将渲染(Renderer)进程得到的内存中的Bitmap(位图)，绘制到用户界面上
  + 网络资源的管理，下载等
+ 第三方插件进程：每使用一种插件，就创建一个插件线程
+ GPU进程：每个浏览器只有唯一一个，用于3D渲染
+ **渲染进程**（Renderer进程）：每个Tab标签页都独立拥有一个
  + 俗称浏览器内核，主要作用为页面渲染，脚本执行，事件处理等
  + 渲染进程内部是**多线程**，详见下节

## 多线程的渲染进程

### GUI 渲染线程

+ 负责渲染浏览器界面，包括：
  + 构造DOM树，渲染Render树
  + 布局和绘制，包括重绘（颜色属性发生变化）和回流（布局位置发生变化）
+ GUI渲染线程与JS引擎线程是**互斥**的
  + 当JS引擎执行时GUI线程会被暂时挂起
  + GUI更新会被保存在一个队列中，等到JS引擎空闲时立即被执行

### JS 引擎线程

+ JS引擎线程就是 JS 内核，负责解析、运行 JavaScript 脚本程序（例如V8引擎）
+ JS引擎一直等待着任务队列中任务的到来，然后加以处理
+ 浏览器同时只能有一个JS引擎线程在运行JS程序，所以JS是单线程运行的
+ JS引擎线程会阻塞GUI渲染线程，当 JS 解析慢时，也会造成页面渲染缓慢

### 事件触发线程

+ 控制事件循环，管理着事件队列
+ 负责将触发的事件交由其他线程处理（如`setTimeOut`事件转交给定时器触发线程），包括事件绑定、异步操作
+ 当异步操作有了结果后，将回调函数放入事件队列队尾等待，等待 JS 引擎的处理

### 定时器触发线程

- 对`setInterval`与`setTimeout`进行计时，在满足时间后，交由事件触发线程添加至事件队列
- W3C在HTML标准中规定，规定要求`setTimeout`中，低于4ms的时间间隔视为4ms

### 异步请求线程

+ 每当一个`XMLHttpRequest`对象被联通后，生成一个新建线程
+ 检测到状态变更（http状态变化）时，如果设置有回调函数，异步线程就产生状态变更事件，将这个回调再放入事件队列中

## 事件循环

JS 运行时，分为同步与异步任务，分别由主线程与事件触发线程管理

主线程：也就是 JS 引擎线程，运行所有的同步任务，形成**执行栈**

事件触发线程：在当异步任务或其他线程有结果后，将回调事件放入**任务队列**，以供主线程调用

当主线程完成执行栈中所有任务后，读取任务队列中的事件到执行栈中，执行栈完成后再次读取执行栈，是为事件循环

> 执行栈每次读取一个任务队列事件完成，而不是一次读取所有任务队列事件到执行栈

### 宏任务与微任务

#### 宏任务(macrotask)

每次完成执行栈的任务，以及读取一个任务队列事件至执行栈完成，该过程为一次**宏任务**；每一次宏任务会从头到尾一次执行完毕

常见的宏任务

- 主代码块
- `setTimeout`和`setInterval`
- `setImmediate()`-Node
- `requestAnimationFrame()`-浏览器

#### GUI渲染

前面已知，JS 引擎线程会阻塞 GUI 渲染线程运行，那么是事件循环间何时进行渲染呢？

答案是在宏任务之间，插入**微任务**进行渲染

```
宏任务 -> GUI渲染 -> 宏任务 -> ...
```

#### 微任务(microtask)

在当前宏任务执行完毕后，会在渲染前，将期间产生的**微任务**都一次处理完

```
宏任务 -> 微任务 -> GUI渲染 -> 宏任务 -> ...
```

常见微任务

- `Promise.then()`
- `catch`与`finally`代码块
- `Object.observe`
- `MutationObserver`
- `process.nextTick()`-Node

### 注意

微任务与宏任务的任务队列不共享，分属不同队列。

运行时，先于第一个任务队列读取一个宏任务，完成后，再清空第二个微任务任务队列

---

GUI渲染时，对同一属性的多次修改进行优化合并（如多次修改背景颜色）时：

+ 在一次宏任务中：优化合并，被覆盖的属性不会被渲染
+ 处于一次宏任务与微任务组合中：优化合并
+ 分处不同的宏任务：不会优化合并

原理：同一的宏任务和微任务，都是在渲染前执行，可以让渲染线程有机会进行优化

### 总览图

![](D:\坚果云\MD笔记\MDimg\16fb7ae3b678f1ea_tplv-t2oaga2asx-zoom-in-crop-mark_4536_0_0_0.png)



