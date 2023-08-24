# JavaScript-数据类型

## 数据类型

![]( https://user-gold-cdn.xitu.io/2018/6/21/164202ad14868ff6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 )

### Undefined

未定义的值

### Null

空值

### Boolean——布尔值

`true`或 `false` 

### String——字符串

被单引号或双引号包括的一串字符

### Number——数值

 JavaScript只有**一种数值类型**，没有其他语言中类似`int`与`float`之分

```javascript
//以下三种值相同，类型也全部相同
var x1 = 2000
var x2 = 2000.00
var x3 = 2e3
```

### Object——对象

对象有许多子类型，属于**可变类型**

#### 常见子类型

+ Array——数组
+ Function——函数
+ Date——日期
+ RegExp——正则
+ [Set——元组](https://es6.ruanyifeng.com/#docs/set-map)
  + `Set`相当于没有重复元素的`Array`，判定的标准为严格相等符号`===`
    + 例如`5`与`'5'`不为重复元素
    + 特殊：`NaN`一般情况下不等于本身，但是在`Set`中**视为相等**，不能添加多个`NaN`
  + **遍历顺序遵循添加顺序**
  + 创建：`new Set()`
  + 属性：`size`-长度
  + 操作方法：
    + `add(item)`：添加值，返回当前`Set`对象，使用链式多次调用添加，而不是传入多个参数
    + `has(item)`：搜索值，返回布尔值
    + `delete(item)`：删除值，返回布尔值，表示是否执行成功
    + `clear()`：清空，无返回
  + 遍历方法：
    + `values()`：返回值列表（实际就是返回成数组）
      + 亦可使用`[...set]`直接进行展开
      + `keys()`方法返回值亦是值列表
    + `forEach()`：遍历`Set`的所有成员。
+ [Map](https://es6.ruanyifeng.com/#docs/set-map)
  + 键值可以为任何类型，相比`Object`只能为`string`
    
    + 但是可变类型做键值，之后无法取到值，因为尽管外观类似，两个可变元素也不为同一值
    
  + **遍历顺序遵循添加顺序**
  
  + 创建：`new Map()`
  
    + 可以使用二维数组来新建Map
  
    ```js
    new Map([["key1", "value1"], ["key2", "value2"]])
    ```
  
  + 属性：
  
    + `size`：键值对数量；注意`length`属性为 0
  
  + 操作方法：
    + `set(key, value)`：赋值或更新，返回当前`Map`对象，可以使用链式多次调用添加
    + `get(key)`：取值，返回对应值或`undefined`
    + `has(key)`：搜索值，返回布尔值
    + `delete(item)`：删除值，返回布尔值，表示是否执行成功
    + `clear()`：清空，无返回
    
  + 遍历方法：
    + `keys()`：返回键名的遍历器。
    + `values()`：返回键值的遍历器。
    + `forEach()`：遍历`Map`的所有成员。

#### 常见创建方式

+ 直接量(字面量)创建
+ 构造函数创建
+ Object.create创建对象
+ ES6 的 class 创建对象 

### Symbol——标志（ES6新增）

 生成一个全局唯一的值

```javascript
let s = Symbol();
```

可以加入字符串，但只是为了区分不同的symbol，算“注释”

```javascript
let s1 = Symbol('a');	// Symbol(a)
let s2 = Symbol('b');	// Symbol(b)
let s3 = Symbol('b');	// 虽然也是Symbol(b)，但和s2不一样
s2 == s3	// false,不相同
```

可以转为字符类型

```js
s1.toString();	// 'Symbol(a)'
```

## 空值

### 概念

 JavaScript可以被叫做空值有四种，都可以被转化成布尔值`false`：

1. null：**null类型**；此处没有对象
2. undefined：**undefined类型**；尚未定义的对象
3. NaN：**数值类型**；非数字值的数字类型对象，注意与0区分
4. ""：**字符类型**；空字符串

实际上的类型：

```js
Object.prototype.toString.call(null)		//"[object Null]"
Object.prototype.toString.call(undefined)	//"[object Undefined]"
Object.prototype.toString.call(NaN)			//"[object Number]"
Object.prototype.toString.call("")			//"[object String]"
```

转化为数值：

```javascript
Number(null)		//0
Number(undefined)	//NaN
Number(NaN)			//NaN
Number("")			//0
```

### 详细对比

#### null

**表示"没有对象"，即该处不应该有值。**典型用法是：

1. 作为函数的参数，表示该函数的参数不是对象
2. 作为对象原型链的终点

```javascript
Object.getPrototypeOf(Object.prototype)	// 获取对象类型原型的原型，为null
```

#### undefined

**表示"缺少值"，就是此处应该有一个值，但是还没有定义。**典型用法是：

1. 变量被声明了，但没有赋值时，就等于undefined
2.  调用函数时，应该提供的参数没有提供，该参数等于undefined
3. 对象没有赋值的属性，该属性的值为undefined
4. 函数没有返回值时，默认返回undefined

```javascript
var i;
i // undefined

function f(x){console.log(x)}
f() // undefined

var  o = new Object();
o.p // undefined

var x = f();
x // undefined
```

#### NaN

全局属性 **`NaN`** 的值表示不是一个数字（Not-A-Number）

+ NaN 属性的初始值就是 NaN，和 [`Number.NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/NaN) 的值一样
+ 现代浏览器中（ES5中）， `NaN`具有 不可配置，不可写的属性
+ 通常我们编程时不会去使用它，它一般被用于Math的某个方法失败后、试图转化字符串**失败后的返回值**

```js
Math.sqrt(-1)	//NaN，试图对负数开平方
parseInt("num101")	//NaN，转化字符串遇到非数字
```

**NaN不等于自身！**使用`==`或是`===`进行比较都将返回`false`，使用下列方法判断一个值是否为NaN：

+ `isNaN()`：对象为NaN，或转化为数字类型将会是NaN时，返回`true`
+ `Number.isNaN()`：当且仅当对象为NaN时，才返回`true`

```js
isNaN(NaN)			//true
Number.isNaN(NaN)	//true

isNaN(parseInt("num101"))			//true
Number.isNaN(parseInt("num101"))	//false
```

#### 空字符串

表示一个**没有内容的字符串对象**

## 获取对象类型

### typeof 方法

```js
// 两种用法都可以
typeof(1)	// number
typeof "string"	// string
```

特点：返回某对象的类型，字符串类型

缺点：只能检测原始类型：boolean、number、string、undefined、function、object；对于array、null以及自定义类型等无效，都返回"object"

### instanceof 方法

```js
1 instanceof Number	//	false
new Number(1) instanceof Number	// true
[] instanceof Array	//	true
```

特点：返回前者是否属于后者类型的*布尔值*，可以适用于自定义类型对象；注意要使用**大写开头**的类型构造函数

缺点：只对于对象有用，数字等类型会返回`false`；但如果使用构造函数（`Number()`、`Boolean()`、`String()`）构造的对象，返回`true`

---

该方法能进行子类型的确认，因此可以被用于`catch`语句中，区分不同的错误进行不同的处理

### toString() 方法

```js
Object.prototype.toString.call(new Set()) // "[object Set]"
Object.prototype.toString.call(new SyntaxError())	// "[object Error]"
```

特点：以字符串类型"[object *type*]"的方式，返回`type`即为类型，大部分类型都可以区分

缺点：不能判断自定义类型对象，只能判断第一层类型（例如能返回`Error`类型，但不会返回具体错误）

原理：

根据MDN文档所述：

> 每个对象都有一个 `toString()` 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，`toString()` 方法被每个 `Object` 对象继承。如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object *type*]"，其中 `type` 是对象的类型。

从中得到的信息为：

最初Object的原型上，`toString()` 方法是一个返回本对象数据类型的函数；在接下来的原型链中可能会被重写为其他用处（譬如转化为字符串）。

那么我们只要直接去拿到最初的`toString()` 方法，再用`call()`函数将`this`指向某个对象，即可拿到该对象的类型值。