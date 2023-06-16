# JavaScript-Promise

> 参考：https://www.runoob.com/w3cnote/javascript-promise-object.html

Promise，中文名为“承诺”，ES6中原生提供了该对象

## 特点与缺点

### 特点

+ Promise是一个异步操作，有三种状态：
  + `pending`：初始还没有处理的状态；**只能通过异步操作的结果**更改状态为`resolved`或`rejected`
  + `resolved`： 意味着操作成功；初始状态通过`resolve()`函数改变而来
  + `rejected`： 意味着操作失败；初始状态通过`reject()`函数改变而来
+ 状态一旦被改变便**凝固**了，不再发生变动，有且只有下面两种情况：
  + 从`Pending`变为`Resolved` 
  + 从`Pending`变为`Rejected`

### 缺点

+ 无法取消执行，新建之后立即执行
+ 不设置回调函数的情况下，内部错误不会抛到外面
+ 当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）

## 创建和调用

### 新建

> [Promise() 构造器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)：
>
> `new Promise(executor)`用于包装不支持promise（返回值不是`Promise`）的函数
>
> 被包装函数（executor）接受两个函数参数（参数名任意）；两个函数的作用相当于`return`，返回其中参数，并分别将该`Promise`转化为成功与失败状态
>
> 注意与`Promise.resolve()`与`Promise.reject()`函数区分

```js
var promise = new Promise(function(resolveFunc, rejectFunc) {
    // doSomething()为异步操作，但是本身不关心下面的操作
    if(doSomething()){	
        resolveFunc() //相当于一个return函数，然后转换将要返回的Promise状态为 resolved
    } else{
        rejectFunc()  //相当于一个return函数，然后转换将要返回的Promise状态为 rejected
    }
});
```

### 调用结果

```js
// onFulfilled,onRejected都为绑定的回调函数
promise.then(onFulfilled).catch(onRejected)
// 简略合并写法
promise.then(onFulfilled, onRejected)
```

### 栗子

```js
function ajax(URL) {
	return new Promise(function (resolve, reject) {
		var req = new XMLHttpRequest(); 
		req.open('GET', URL, true);
		req.onload = function () {
		if (req.status === 200) { 
				resolve(req.responseText);
			} else {
				reject(new Error(req.statusText));
			} 
		};
		req.onerror = function () {
			reject(new Error(req.statusText));
		};
		req.send(); 
	});
}
var URL = "/try/ajax/testpromise.php"; 
ajax(URL).then(function onFulfilled(value){
	document.write('内容是：' + value); 
}).catch(function onRejected(error){
	document.write('错误：' + error); 
});
```

函数`ajax()`返回一个`promise`对象，调用函数获取返回对象

使用`then()`获取返回值，或使用`catch()`获取错误

```js
var p1 = new Promise(function(resolve, reject){
  // ... some code
});
 
var p2 = new Promise(function(resolve, reject){
  // ... some code
  resolve(p1);
})
```

`resolve()`的参数也可以是另一个`promise`对象

## 方法

> 实例方法：需要通过构造器构造对象实例，在对象实例上访问的方法
>
> 静态方法：可以直接在对象的构造器上进行访问的方法

### 实例方法

#### `then`方法：链式操作

[`Promise.prototype.then`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)方法*返回的是一个新的 Promise 对象*，因此可以采用链式写法

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // proceed
});

getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // 对comments进行处理
});
```

#### `catch`方法：捕捉错误

[`Promise.prototype.catch`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)方法是`Promise.prototype.then(null, rejection)`的别名，也*返回一个新的Promise对象*

用于指定发生错误时的回调函数，不单单是捕捉第一个Promise（`getJSON`操作）中的错误，也包括之前的所有Promise（包括`then`函数返回的Promise，即`then()`中的错误也会就近于下一个`catch()`函数中被接收）

```js
getJSON("/posts.json").then(function(posts) {
  // some code
}).catch(function(error) {
  // 处理前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});

getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前两个回调函数的错误
});
```

合并写法

```javascript
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(
    function(comments) {
  		// some code
	},
    function(error) {
  		// 处理前两个回调函数的错误
	}
);
```

Promise 对象的错误具有"冒泡"性质，会一直向后传递，直到被捕获为止。也就是说，错误**总是会被下一个 catch 语句**捕获

#### `finally`方法：最终执行

[`Promise.prototype.finally`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)方法指定一个回调函数，于`Promise`对象结束后，无论状态如何都会被调用

该方法不接受任何参数，并返回原`Promise`

注意和`try…catch…finally`代码块做区分：

```javascript
promise1.then().catch().finally(() => {...})	// 接受一个回调函数
try{...} catch{...} finally{...}				// 直接后续代码块
```

### 静态方法

#### `all&allSettled&race&any`方法：包装多个Promise对象

将多个`promise`对象组合为新的`promise`对象（也就是接收一个`iterable`类型，其中的元素为`Promise`类型）

+ [`Promise.all()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)：结果为且关系，只有**全部成功**才返回成功状态，否则返回失败状态
  + 返回**一个**`Promise`实例
  + 成功状态：返回一个`Promise`实例，兑现值（`resolve`状态的回调中所传入值）为包含所有传入`promise`的结果数组
  + 失败状态：返回第一个返回失败的`Promise`实例
+ [`Promise.allSettled()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)：当所有`promise`对象**都完成后**（resolved或rejected）返回
  + 返回一个`Promise`实例
    + 已经兑现：传入的`iterable`里是空的
    + 异步兑现：兑现值是对象数组，每个对象描述每个传入的`promise`的结果，具有以下属性：
      + `status`：字符串，标识该`promise`的最终状态，`fulfilled`或`rejected`
      + `value`或`reason`：`fulfilled`状态下存在`value`；`rejected`存在`reason`
+ [`Promise.race()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)：结果为竞速关系，总是返回**第一个改变状态**的`promise`的结果
+ [`Promise.any()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)：（实验性语法）结果为或关系，返回**第一个兑现状态**的`promise`对象
  + 如果没有兑现的`promise`，则变为拒绝状态，拒绝原因（传入reject回调的值）为一个[`AggregateError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)实例（错误集合）

#### `resolve&reject`方法：转化为Promise对象

将现有对象转为Promise实例对象：

+ [`Promise.resolve()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)：转化为已经成功的状态的实例
+ [`Promise.reject()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)：转化为已经失败的状态的实例

## async/await 关键字

> 参见：https://es6.ruanyifeng.com/#docs/async#await-%E5%91%BD%E4%BB%A4

ES7的新关键字，让异步代码转为更易理解的形式

### async

使用`async`关键字置于函数声明前，将其转化为异步函数（async function）

异步函数的返回值**总是一个`Promise`对象**（即使没有return语句），可以通过`then()`方法调取结果

### await

在异步函数内（也就是使用`async`声明的），使用`await`关键字放在任何异步、基于 promise 的表达式前

异步函数运行到此，代码将会**暂停直到`Promise`完成**，并获得结果值；在此期间， JS 会执行其他（非该处代码）等待中的代码

表达式的返回结果：

+ 不为`Promise`对象，则直接返回该对象
+ 成功的`Promise`对象（resolved状态），返回本来传递到`resolve`回调函数中的值，然后异步函数继续运行
+ 失败的`Promise`对象（rejected状态），抛出一个`Promise`对象的异常原因，可以使用`try…catch`语法接收该异常

