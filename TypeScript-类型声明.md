# TypeScript-类型声明

> 参考：https://juejin.cn/post/6844903925338865678

在 typescript 中，变量都需要声明类型使用

声明类型时，使用**小写字母**开头的形式

### 基本类型

首先是常见的：数字（浮点数）、字符串与布尔类型

```typescript
const num:number = 1;
const str:string = 'str';
const bool:boolean = true;
```

`null`与`undefined`，还有ES6中新增的标志（`Symbol`）类型

```typescript
const myNull:null = null;
const myUndefined:undefined = undefined;
const mySymbol:symbol = Symbol('symbol');
```

### 特殊类型

声明为`any`类型时，不约束类型

```typescript
const any:any = "any types"
```

`unknown`类型类似`any`，可接受任何类型；但使用该类型变量时，必须显式地指定其类型或可以推断出来

```typescript
try {
	...
} catch(err: unknown) {
	if (err instanceof Error) {
		// 此处可以推断出err为Error类型
	}
}
```

`void`类型一般用于**无返回类型**的函数声明，函数无具体返回值，但可以返回

```ts
function printUsername (name: string): void {
    console.log(name);
}
```

`never`类型表示一个**永远不能返回成功**的函数：

+ 永远不返回的函数，譬如函数体内包含`while(true)`
+ 永远抛出错误的函数

```typescript
// 永远不返回
function foo1():never { 
    while(true) {}
}
// 永远抛出错误
function foo2():never { 
	throw new Error('Not Implemented') 
}
```

### 数组类型

**推荐的声明方法：**

```typescript
const nums:number[] = [1, 2, 3, 4]
const dates:Date[] = [new Date(), new Date()]
```

不推荐的声明方法：

```ts
const strs:Array<string> = ["a", "b", "c"]
```

### 接口类型

使用`inferface`关键词，定义接口，也可以用来声明**函数、类与对象**等数据类型

详见[接口部分](##接口)

```ts
interface Name {
  first: string;
  second: string;
}

let username: Name = {
  first: 'John',
  second: 'Doe'
};
```

### 联合类型

一个值可以为多种类型，此时可以声明为联合类型

```ts
let tag: string | number 
tag = "str"
tag = 1
```

---

方便起见，可以将联合类型声明为一个新类型，有两种方法：

+ 定义类型：`type`关键词

  ```ts
  type StrOrNum = string | number;
  
  let sample: StrOrNum;
  sample = 123;
  sample = '123';
  ```

+ 定义接口：`inferface`关键词

  ```ts
  interface Name {
    first: string;
    second: string;
  }
  
  let username: Name = {
    first: 'John',
    second: 'Doe'
  };
  ```

### 元组类型

固定长度且每个元素都固定类型的类型，常常用于限制函数的输入输出类型

```ts
let nameNumber: [string, number] = ['Jenny', 221345];
```

### 字面量类型

指定类型，取值为固定范围的常量

```ts
type selectType = "a" | "b" | "c"	// 限定选择题类型只能有三个选项
let answer:selectType = "a"
```

实际上，可以认为`boolean`类型为`true`与`false`的字面量类型

```ts
type boolean2 = true | false
```

### 断言

指出某一变量的明确类型，使用`as`关键词

```ts
id as number
```

用处：

+ 将联合类型断言为其中一种类型
+ 将父类断言为更具体的子类
+ 断言任何类型为`any`
+ 断言`any`为具体类型
+ 两次断言强制转换类型

#### 非空断言

> 参见：[TypeScript 中的代码清道夫：非空断言操作符](https://juejin.cn/post/6844904149897723917)

变量名后使用`!`符号，断言该变量一定不为`null`或`undefined`

```typescript
// 用于普通类型
function simpleExample(a: number | undefined) {
   const b: number = a; // COMPILATION ERROR: undefined is not assignable to number.
   const c: number = a!; // OK
}

// 用于函数类型
type NumGenerator = () => number;

function myFunc(numGenerator: NumGenerator | undefined) {
   const num1 = numGenerator(); //compilation error: cannot invoke an object which is possibly undefined
   const num2 = numGenerator!(); //no problem
}
```

## 常见问题

### 缺少声明文件

会导致声明使用报错，此时应该寻找相应文件放入types文件夹下

### 全局变量

一些库会在 window 上添加全局变量

> [`declare var`](https://ts.xcatliu.com/basics/declaration-files.html#declare-var) 声明全局变量

```ts
// globals.d.ts
// 例子：jQuery，现实中jQuery是有.d.ts
declare const jQuery: any;
declare const $: typeof jQuery;
```

### 非 JS 资源

当导入这些资源时，需要告知 ts 如何识别

> [`declare module`](https://ts.xcatliu.com/basics/declaration-files.html#declare-module) 扩展模块

```
declare module '*.vue' {
  import Vue from 'vue';
  export default Vue;
}

// html
declare module '*.html';
// css
declare module '*.css';

```

### 断言/强制类型转换

有时候报错是因为对象实际上的类型与预期的类型有差别，导致找不到方法

此时使用断言，告知 ts 确认转换为另一种类型

~~第一种：使用尖括号~~，不推荐，与 JSX 语法不兼容

```ts
const convertArrType: string[] = <Array<string>>[].concat(['s']);
```

**第二种：使用as关键字**，推荐

```ts
const fixArrNever: string[] = ([] as string[]).concat(['s']);
```

### 可选属性和默认属性

函数中，可能有可选的属性与带默认值的属性

可选属性使用`?`声明，默认属性的类型可以根据默认值确定

```ts
function eventHandler = (
  evt: CloseEvent | MessageEvent | Event,
  socket?: Socket,
  type = 'WebSocket'
) => any;
```

### 添加/忽略校验

+ 白名单模式：在文件头部使用`// @ts-check`，将文件列入**校验**名单（默认所有文件都不校验）
+ 黑名单模式：在文件头部使用 `// @ts-nocheck`，将文件列入**不校验**名单（默认校验所有文件）
+ 使用`// @ts-ignore`，**单独忽略某行**不进行校验

## 泛型

+ **泛型**是一种特殊类型，没有固定的类型，是一种**概念类型**；
+ 如同函数接受形参一样，泛型**接受一种任意类型，并在之后指代该类型**，用于接受不能提前预知类型的情况；
+ 与`any`有所不同，前者实际上放弃了对类型的限制，而 泛型 还保留着传入类型的信息

---

### 泛型函数

示例，一个不限制传入类型，但传入类型与传出类型要相同的**泛型函数**`identity`；

此处的`T`便是 泛型 ，函数接受一种不定类型T，并限定返回这种T类型

```ts
function identity<T>(arg: T): T {
    return arg
}
```

调用该函数：

+ 使用`<>`指定泛型类型，并传入形参
+ 只传入形参，编译器自动根据形参确认泛型（称为*类型推论*）

```ts
let output = identity<string>("myString");	// 第一种
let output2 = identity("myString");			// 第二种
```

### 泛型变量

在使用被指定为泛型的变量时，需要考虑到泛型被指定为任何一种类型的可能性；

有些类型不支持的方法，泛型也不能使用。

```ts
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}
```

如上示例，存在 T 被指定为`number`类型的可能性，此时调用`lenght`属性会出错，所以此处也会报错

## 接口

接口（interface）规定一个对象所需要实现的方法或变量，类似抽象类的概念，也就是**自定义一种对象类型**

下列代码定义了一个`LabelledValue`类，该类对象必须包含一个`label`属性，且值类型为字符串

```ts
interface LabelledValue {
  label: string;
}
```

在下列代码中，该函数需要传入一个`LabelledValue`类型的对象

```ts
function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}
```

类型检查器会对传入的对象进行检查：

+ 只关注对象中相应属性的存在，与其值的类型是否正确
+ 不对属性的顺序做限制

---

### 可选属性

在属性名后，添加标识符`?`来指定一个可选属性

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}
```

### 只读属性

在属性名前，添加`readonly`标识符来指定一个只读属性

只读属性只允许对象创建时，对其值进行修改

```ts
interface Point {
    readonly x: number;
    readonly y: number;
}
```

赋值一个对象字面量给`Point`类对象。此后x、y不能再被改变

```ts
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

---

TypeScript具有`ReadonlyArray<T>`类型，它与`Array<T>`相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

甚至**不能**用只读数组去赋值另一个数组，但是可以使用`as`进行断言重写

```typescript
a = ro as number[];
```

> `const`：用于定义一个变量为只读
>
> `readonly`：用于定义对象中的属性为只读

### 接口-函数类型

除了描述一个普通对象，接口也可以描述一个**函数对象**

采用下列样式定义，**每个参数都需要名字与类型**

```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

类型检查时，函数的**参数名不需要遵守接口**，但是**参数的顺序需要遵守接口**

```typescript
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

也可以不指定参数的类型，遵守接口定义，并注意**函数返回值也需要遵守接口**

```typescript
let mySearch: SearchFunc;
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```

### 可索引的类型

当属性类型确定，无确定名称时，定义该种类型属性，所返回的值类型；属性支持数字与字符串两种类型

譬如下面的例子，当属性名为数字类型时，返回值为字符串类型：

```typescript
interface StringArray {
  [index: number]: string;
}
```
