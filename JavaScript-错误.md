#   JavaScript-错误

## Error 实例对象的属性

- **message**：错误提示信息
- **name**：错误名称（非标准属性）
- **stack**：错误的堆栈（非标准属性）

## 原生错误类型

### 语法错误：SyntaxError 对象

`SyntaxError`对象是解析代码时发生的语法错误。

```js
// 变量名错误
var 1a;
// Uncaught SyntaxError: Invalid or unexpected token

// 缺少括号
console.log 'hello');
// Uncaught SyntaxError: Unexpected string
```

上面代码的错误，都是在语法解析阶段就可以发现，所以会抛出`SyntaxError`。第一个错误提示是“token 非法”，第二个错误提示是“字符串不符合要求”。

### 引用错误：ReferenceError 对象

`ReferenceError`对象是引用一个不存在的变量时发生的错误。

```js
// 使用一个不存在的变量
unknownVariable
// Uncaught ReferenceError: unknownVariable is not defined
```

另一种触发场景是，将一个值分配给无法分配的对象，比如对函数的运行结果或者`this`赋值。

```js
// 等号左侧不是变量
console.log() = 1
// Uncaught ReferenceError: Invalid left-hand side in assignment

// this 对象不能手动赋值
this = 1
// Uncaught ReferenceError: Invalid left-hand side in assignment
```

上面代码对函数`console.log`的运行结果和`this`赋值，结果都引发了`ReferenceError`错误。

### 范围错误：RangeError 对象

`RangeError`对象是一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是`Number`对象的方法参数超出范围，以及函数堆栈超过最大值。

```js
// 数组长度不得为负数
new Array(-1)
// Uncaught RangeError: Invalid array length
```

### 类型错误：TypeError 对象

`TypeError`对象是变量或参数不是预期类型时发生的错误。比如，对字符串、布尔值、数值等原始类型的值使用`new`命令，就会抛出这种错误，因为`new`命令的参数应该是一个构造函数。

```js
new 123
// Uncaught TypeError: number is not a func

var obj = {};
obj.unknownMethod()
// Uncaught TypeError: obj.unknownMethod is not a function
```

上面代码的第二种情况，调用对象不存在的方法，也会抛出`TypeError`错误，因为`obj.unknownMethod`的值是`undefined`，而不是一个函数。

### URI 错误：URIError 对象

`URIError`对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及`encodeURI()`、`decodeURI()`、`encodeURIComponent()`、`decodeURIComponent()`、`escape()`和`unescape()`这六个函数

+ `escape()`目前已经不推荐使用
+ `encodeURI()`可以用来对完整的URI字符串进行编码处理。而`encodeURIComponent()`可以对URI中一个部分进行编码，从而让这一部分可以包含一些URI保留字符
+ `encodeURI()`不编码字符：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z
+ `encodeURIComponent()`不编码字符：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z

```JS
decodeURI('%2')
// URIError: URI malformed
```

### Eval 错误：EvalError 对象（被弃用！）

`eval`函数没有被正确执行时，会抛出`EvalError`错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。新版本请使用 `SyntaxError`代替该错误

### 构造错误实例

以上这6种派生错误，连同原始的`Error`对象，都是构造函数。开发者可以使用它们，手动生成错误对象的实例。这些构造函数都接受一个函数，代表错误提示信息（message）。

```js
var err1 = new Error('出错了！');
var err2 = new RangeError('出错了，变量超出有效范围！');
var err3 = new TypeError('出错了，变量类型无效！');

err1.message // "出错了！"
err2.message // "出错了，变量超出有效范围！"
err3.message // "出错了，变量类型无效！"
```

## 自定义错误

除了 JavaScript 原生提供的七种错误对象，还可以定义自己的错误对象。

```js
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();
UserError.prototype.constructor = UserError;
// 或者
export expError extends Error {
	constructor(code, message) {
		……
	}
}
```

上面代码自定义一个错误对象`UserError`，让它继承`Error`对象。然后，就可以生成这种自定义类型的错误了。

```js
new UserError('这是自定义的错误！');
```

## 捕获错误

### 同步捕获 & 用法

#### throw 语句

`throw`语句的作用是手动中断程序执行，抛出一个错误。

```js
if (x < 0) {
  throw new Error('x 必须为正数');
}
// Uncaught ReferenceError: x is not defined
```

上面代码中，如果变量`x`小于`0`，就手动抛出一个错误，告诉用户`x`的值不正确，整个程序就会在这里中断执行。可以看到，`throw`抛出的错误就是它的参数，这里是一个`Error`实例。

`throw`也可以抛出自定义错误。

```js
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

throw new UserError('出错了！');
// Uncaught UserError {message: "出错了！", name: "UserError"}
```

上面代码中，`throw`抛出的是一个`UserError`实例。

实际上，`throw`可以抛出任何类型的值。也就是说，它的参数可以是任何值。

```js
// 抛出一个字符串
throw 'Error！';
// Uncaught Error！

// 抛出一个数值
throw 42;
// Uncaught 42

// 抛出一个布尔值
throw true;
// Uncaught true

// 抛出一个对象
throw {
  toString: function () {
    return 'Error!';
  }
};
// Uncaught {toString: ƒ}
```

对于 JavaScript 引擎来说，遇到`throw`语句，程序就中止了。引擎会接收到`throw`抛出的信息，可能是一个错误实例，也可能是其他类型的值。

#### try…catch 结构

一旦发生错误，程序就中止执行了。JavaScript 提供了`try...catch`结构，允许对错误进行处理，选择是否往下执行。

```js
try {
  throw new Error('出错了!');
} catch (e) {
  console.log(e.name + ": " + e.message);
  console.log(e.stack);
}
// Error: 出错了!
//   at <anonymous>:3:9
//   ...
```

上面代码中，`try`代码块抛出错误（上例用的是`throw`语句），JavaScript 引擎就立即把代码的执行，转到`catch`代码块，或者说错误被`catch`代码块捕获了。`catch`接受一个参数，表示`try`代码块抛出的值。

如果你不确定某些代码是否会报错，就可以把它们放在`try`代码块之中，发生错误时通过`catch`代码块进行处理。

```js
try {
  f();
} catch(e) {
  // 处理错误
}
```

上面代码中，如果函数`f`执行报错，就会进行`catch`代码块，接着对错误进行处理。

`catch`代码块捕获错误之后，程序不会中断，会按照正常流程继续执行下去。

```js
try {
  throw "出错了";
} catch (e) {
  console.log(111);
}
console.log(222);
// 111
// 222
```

上面代码中，`try`代码块抛出的错误，被`catch`代码块捕获后，程序会继续向下执行。

`catch`代码块之中，还可以再抛出错误，甚至使用嵌套的`try...catch`结构。

```js
var n = 100;

try {
  throw n;
} catch (e) {
  if (e <= 50) {
    // ...
  } else {
    throw e;
  }
}
// Uncaught 100
```

上面代码中，`catch`代码之中又抛出了一个错误。

为了捕捉不同类型的错误，`catch`代码块之中可以加入判断语句。

```js
try {
  foo.bar();
} catch (e) {
  if (e instanceof EvalError) {
    console.log(e.name + ": " + e.message);
  } else if (e instanceof RangeError) {
    console.log(e.name + ": " + e.message);
  }
  // ...
}
```

上面代码中，`catch`捕获错误之后，会判断错误类型（`EvalError`还是`RangeError`），进行不同的处理。

---

注意：`try…catch…`语句<span style="color:red;">**不能捕获异步异常**</span>

以下代码错误会被抛出，但不会被`catch`所捕获

```js
try {
  setTimeout(() => {
    let len = undefined.length	
  }, 1000);
} catch (error) {
  console.error(error)
}
```

#### finally 代码块

`try...catch`结构允许在最后添加一个`finally`代码块，表示不管是否出现错误，都**必须在最后运行**的语句。

```js
function cleansUp() {
  try {
    throw new Error('出错了……');
    console.log('此行不会执行');
  } finally {
    console.log('完成清理工作');
  }
}

cleansUp()
// 完成清理工作
// Error: 出错了……
```

上面代码中，由于没有`catch`语句块，所以错误没有捕获。执行`finally`代码块以后，程序就中断在错误抛出的地方。

---

```js
function idle(x) {
  try {
    console.log(x);
    return 'result';
  } finally {
    console.log("FINALLY");
  }
}

idle('hello')
// hello
// FINALLY
// "result"
```

上面代码说明，`try`代码块没有发生错误，而且里面还包括`return`语句，但是`finally`代码块依然会执行。注意，只有在其执行完毕后，才会显示`return`语句的值。

---

下面的例子说明，`return`语句的执行是排在`finally`代码之前，只是等`finally`代码执行完毕后才返回。

```js
var count = 0;
function countUp() {
  try {
    return count;
  } finally {
    count++;
  }
}

countUp()
// 0
count
// 1
```

上面代码说明，`return`语句的`count`的值，是在`finally`代码块运行之前就获取了。

---

下面是`finally`代码块用法的典型场景。

```js
openFile();

try {
  writeFile(Data);
} catch(e) {
  handleError(e);
} finally {
  closeFile();
}
```

上面代码首先打开一个文件，然后在`try`代码块中写入文件，如果没有发生错误，则运行`finally`代码块关闭文件；一旦发生错误，则先使用`catch`代码块处理错误，再使用`finally`代码块关闭文件。

下面的例子充分反映了`try...catch...finally`这三者之间的执行顺序。

```js
function f() {
  try {
    console.log(0);
    throw 'bug';
  } catch(e) {
    console.log(1);
    return true; // 这句原本会延迟到 finally 代码块结束再执行
    console.log(2); // 不会运行
  } finally {
    console.log(3);
    return false; // 这句会覆盖掉前面那句 return
    console.log(4); // 不会运行
  }
  console.log(5); // 不会运行
}

var result = f();
// 0
// 1
// 3
result
// false
```

上面代码中，`catch`代码块结束执行之前，会先执行`finally`代码块。

`catch`代码块之中，触发转入`finally`代码块的标志，不仅有`return`语句，还有`throw`语句。

```js
function f() {
  try {
    throw '出错了！';
  } catch(e) {
    console.log('捕捉到内部错误');
    throw e; // 这句原本会等到finally结束再执行
  } finally {
    return false; // 直接返回
  }
}

try {
  f();
} catch(e) {
  // 此处不会执行
  console.log('caught outer "bogus"');
}
//  捕捉到内部错误
```

上面代码中，进入`catch`代码块之后，一遇到`throw`语句，就会去执行`finally`代码块，其中有`return false`语句，因此就直接返回了，不再会回去执行`catch`代码块剩下的部分了。

### 异步捕获错误

不能直接捕获异步错误，因为异步代码实际并不处于`try`语句上下文中执行

```js
try {
  ;(async () => {
    throw new Error('err') // uncaught
  })()
} catch (e) {
  console.log(e)
}
```

> 立即执行函数前的分号，用以避免执行时意外组合发生错误

捕捉异步函数的错误，可以：

+ 调用`catch`函数进行捕获
+ 直接在异步函数本身中捕获

```js
;(async () => {
  throw new Error('err')
})().catch((e) => {
  console.log(e) // caught
})

;(async () => {
  try {
    throw new Error('err')
  } catch (e) {
    console.log(e) // caught
  }
})()
```

#### 多个异步

对于循环带来的多个异步函数，使用`Promise.all()`

```js
Promise.all(
    [1, 2, 3].map(async (i) => {
        if( i!= 1)
            throw new Error(`err${i}`)
    })
).catch (e =>  {
  console.log(e) // caught err2
})
```

#### 多层异步

而 Promise 中如果还包含一层异步函数，在该函数中使用`reject`方法，返回错误

```js
new Promise((res, rej) => {
  setTimeout(() => {
    rej('err') // reject Promise
  }, 0)
}).catch((e) => {
  console.log(e) // caught
})
```

或者拆分为Promise Chain的形式，在`then`函数中抛出错误

```js
new Promise((res, rej) => {
  setTimeout(res, 1000) // 1
})
  .then((res, rej) => {
    throw Error('err')
  })
  .catch((error) => {
    console.log(error)
  })
```

### 全局捕获

添加事件监听器，监视错误

+ `window.addEventListener('error')`
  + 捕获所有同步、异步的**运行错误**，但无法捕捉语法、接口或资源加载的错误
+ `window.addEventListener('unhandledrejection')`
  + 捕捉 Promise 中抛出，但未被`.catch`捕获的错误