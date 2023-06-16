# JavaScript-console对象

> 参考：https://juejin.cn/post/7025399805878730788

**`Console`** 对象提供了浏览器控制台调试的接口

+ 可以从任何全局对象中访问到，如 [`Window.console`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/console) 
+ 也可以简单的通过 `console` 引用

调试中经常使用`console.log()`函数打印一些关键信息，但其实`console`全局对象有更多方法

## 基本

### log()

`console.log()`：最原始的打印方法，支持**占位符**

+ 字符串：%s；整数：%d；浮点数：%f
+ 对象：%o、%O
  + 对于一般对象，`%o`、`%O` 作用相同；而对于DOM对象来说，`%o`打印节点及其后代节点，而`%O`打印其对象属性
+ CSS样式：%c

```js
console.log('My Name is %cSKY', 'color: skyblue; font-size: 30px;') 	// 对%c之后的内容，添加样式
console.log('%c ','background-image:url("xxx");line-height:60px;padding:30px 120px;');	// 打印图片，需要用属性撑起行内元素的大小
```

### warn() & error() & info()

同`log()`方法，打印信息，但样式发生改变

此外，`error()`方法可以打印函数的调用顺序（调用栈）

## 打印时间

### time() & timeEnd()

两个方法搭配使用，记录期间运行的时间；多对函数同时使用时，传入字符串参数作为配对标识

```js
console.time("timer1");
console.time("timer2");

setTimeout(() => {
	console.timeEnd("timer1");
}, 1000);

setTimeout(() => {
	console.timeEnd("timer2");
}, 2000);
```

### timeLog()

同样搭配`time()`使用；但是不同于`timeEnd()`，`timeLog()`执行后，不会清除当前计时器

## 分组打印

### group() & groupEnd()

当打印信息较多时，使用分组打印，可嵌套使用；传入参数为字符串，既做标识符，也做为组名

### groupCollapsed()

搭配`groupEnd()`使用，但生成的组默认为折叠状态

## 打印计数

### count()

置于循环体中计数；传入参数为字符串，作为标识符与前缀

### countReset()

搭配`count()`方法，重置计数

## 其他

### table()

对于两层对象（包括对象数组），以表格形式打印；接收两个参数

+ 打印对象/数组
+ 需要展示的属性值们（数组）

```js
console.table(obj, [keys])
```

### clear()

清空控制台输出

### assert()

断言某条件，在条件语句返回`false`才打印错误信息

```js
console.assert(expression, message)
```

### trace()

打印函数的调用顺序（调用栈），与`error()`效果相同

### dir()

接收对象，并返回对象属性

对于DOM对象，该方法同样返回属性；与之相对，`log()`方法会打印DOM结构

### dirxml()

打印XML或HTML元素

### memory

是属性，不是方法，查看当前的内存使用情况