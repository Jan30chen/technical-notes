# JavaScript-ES6新特性

> 参考：
>
> [ES6中常用的10个新特性讲解](https://juejin.im/post/6844903618810757128)
>
> [ECMAScript 6 简介](https://es6.ruanyifeng.com/#docs/intro)

## ES6是什么？

ES6是**ECMAScript 6.0**的简称，是下一代 JavaScript 标准

### ECMAScript 和 JavaScript 的关系

前者是后者的制定标准，后者是前者的一种实现形式（另外的 ECMAScript 实现还有 JScript 和 ActionScript等语言）

### ES6 与 ES2015 的关系

ES2015表示这种后接年份的表达形式，特指该年的正式版本

ES6泛指 5.1版本后的 JavaScript，表示下一代 JavaScript 标准的概念；所以其包含ES2015、ES2016、ES2017等具体标准

---

注：借助Bable转码器，可以实现ES6到ES5的转码，从而兼容较低版本的环境运行

## ES6常见新特性

### `const`和`let`声明

+ 两者作用只限于当前块级作用域（花括号），都不会被变量提升
+ `const`表示声明常量，声明和赋值必须在同一行进行，且不能再被更改
  + 如果声明对象常量，对象中的值可以被修改（对象指向内存地址不变）
  + 请使用全大写加下划线的形式命名，以标识这是一个常量
+ `let`表示声明局部变量，在同一个作用域内不能重复声明同一个变量名 

```js
//	常量：声明时不赋值会抛出语法错误，修改会抛出类型错误，
const NUM1;	//SyntaxError: missing = in const declaration
const NUM1 = "NUM1";	//正常运行
NUM1 = "NUM2";	//TypeError: invalid assignment to const 'NUM1'

//	对象常量：修改其中属性不会报错，修改对象本身会抛出类型错误
const ARRAY1 = {"name":"Tom"}	
ARRAY1['name'] = "Jerry"	//正常运行
ARRAY1 = {"name":"Tom"}	//TypeError: invalid assignment to const 'ARRAY1'

//	局部变量：修改不会报错，重复声明会抛出语法错误 
let NUM2 = "NUM2"
NUM2 = "num2"
let NUM2 = "NUM3"	//SyntaxError: redeclaration of let NUM2
```

### 模板字符串

使用模板字符串直接嵌入变量：

+ 使用反引号包裹字符串
+ 使用`${}`插入变量到指定位置

```js
// ES6前的用法
$("body").html("This demonstrates the output of HTML \
content to the page, including student's\
" + name + ", " + seatNumber + ", " + sex + " and so on.");
//	更便捷的ES6用法
$("body").html(`This demonstrates the output of HTML content to the page, 
including student's ${name}, ${seatNumber}, ${sex} and so on.`);
```

### 箭头函数

使用 **括号包裹参数 + `=>` + 花括号包裹函数体** 的形式，快速声明一个函数；

+ 参数只有一个时，可以省略括号
+ 函数体只有一条语句时，可以省略`return`语句和花括号

```js
//	ES6之前
var add = function (a, b) {
    return a+b;
};
//	ES6
var add = (a, b) => { return a+b };
//	省略写法
var square = a => a*a;
```

### 函数的默认参数

使用默认参数，可以使得函数在缺省参数的情况下运行

```js
// ES6之前
function printText(text) {
    text = text || 'default';
    console.log(text);
}

// ES6
function printText(text = 'default') {
    console.log(text);
}

printText('hello'); // hello
printText();// default
```

### Spread / Rest 操作符（...）

用于函数使用时，为Spread操作符，会将数组扩展成多个变量后传入

```js
function foo(x,y,z) {
  console.log(x,y,z);
}

let arr = [1,2,3];
foo(...arr); // 1 2 3
```

用于函数声明时，为Rest操作符，会将超出形参数量的变量转化为数组传入

```js
function foo(...args) {
  console.log(args);
}
foo( 1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

### 二进制和八进制字面量

在数字前面添加`0o`或`0O`，可以得到一个八进制数字；添加`0b`或`0B`，可以得到一个二进制数字

```js
let oValue = 0o10;
console.log(oValue); // 8
let bValue = 0b10; 
console.log(bValue); // 2
```

### 对象和数组解构赋值

> **解构赋值**语法是一种 Javascript 表达式。通过**解构赋值,** 可以将属性/值从对象/数组中取出,赋值给其他变量

#### 数组

通过等式两边同样的位置，快速赋值多个变量

```js
let [a, b] = [1, 2];
console.log(a, b);	//1 2
```

搭配rest参数使用：

```js
let [a, b, ...rest] = [1, 2, 3, 4, 5];
console.log(a, b, rest)	//1 2 Array(3) [ 3, 4, 5 ]
```

#### 对象

快速取出对象中的变量

```js
// 对象
const student = {
    name: 'Sam',
    age: 22,
    sex: '男'
}

// ES5；
const name = student.name;
const age = student.age;
const sex = student.sex;

// ES6
const { name, age, sex } = student;
```

### `for...of` 和 `for...in`循环

`for...of` 循环：

+ 只能遍历数组，不能遍历对象
+ 循环中可以 break 和 return

`for...in` 循环：

+ 用来遍历对象属性
+ 遍历顺序不确定

更多见[四种for循环](JavaScript-四种for循环.md)

### 类与超类

ES6 已经支持`class`关键词和`super()`函数。

但`class`并不是新的对象继承模型，而是原型链的语法糖形式

#### 类的使用

+ `class`关键词创建的对象，实质还是函数对象
+ 使用`static`关键词声明静态方法

> 静态方法：只能通过类本身调用，不能通过类的实例调用的方法

```js
class Student {
  constructor() {
    console.log("I'm a student.");
  }
 
  study() {
    console.log('study!');
  }
 
  static read() {
    console.log("Reading Now.");
  }
}
 
console.log(typeof Student); // function；声明对象实质还是函数类型
let stu = new Student(); // "I'm a student."；创建对象调用构造函数
stu.study(); // "study!"
stu.read(); //  TypeError: stu.read is not a function；错误，无法使用类实例调用
```

#### 超类与继承

+ 使用`extends`关键词，使得一个子类继承一个父类

+ 子类的构造函数中需要调用`super()`函数继承父类的`this`对象，在此之前调用`this`对象会抛出引用错误

+ 使用`super.parentMethodName()`的形式调用父类方法

```js
class Phone {
  constructor() {
    console.log("I'm a phone");
  }
}
 
class MI extends Phone {
  constructor() {
    super();
    console.log("I'm a phone designed by xiaomi");
  }
}
 
let mi8 = new MI();
// outcome:
// I'm a phone
// I'm a phone designed by xiaomi
```

注意事项：

+ 类的声明不会被变量提升，使用之前请先声明
+ 类中定义函数，不需要使用`function`关键词

### Promise对象

[详见](./JavaScript-Promise.md)

