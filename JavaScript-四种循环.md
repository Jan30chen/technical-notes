# JavaScript-四种循环

> 参考：https://juejin.im/entry/5a1654e951882554b8373622

## `for`循环

简单for循环，适用遍历对象不发生变化的场景

```js
const arr = [1, 2, 3];
for(let i = 0, len = arr.length; i < len; i++) {
    console.log(arr[i]);
}
```

## `for-in`循环

`for-in`循环遍历的是**对象的属性**，而不是数组的索引

因此，`for-in`遍历的对象便不局限于数组，还可以遍历对象

遍历对象时为遍历键值；

```js
const obj = {a:1, b:2, c:3};
let key;
for(key in obj) {
    console.log("obj[" + key + "] = " + obj[key]);
}
```

遍历数组时遍历“索引”，此处“索引”为字符串类型，其实还是属性

```js
const arr = [1, 2, 3];
let index;
for(index in arr) {
    console.log("arr[" + index + "] = " + arr[index]);
}
```

**注意：`for-in`遍历属性的顺序并不确定**，因此不能用于顺序遍历

总结：

+ 更适合遍历对象
+ 循环开销更大
+ for-in 只会遍历存在的实体，因此遍历稀疏数组效果比普通遍历好

> 稀疏数组：大部分的元素值都未被使用（或都为0），在数组中仅有少部分的空间使用

## `forEach`循环

特点

+ 为数组中每个有效项目**执行回调函数**，最终返回`undefined`
+ 未被赋值或被`delete`方法删除的项目被跳过，但是`null`、`undefined`会被调用
+ 遍历的范围在第一次调用**回调函数前**就会确定

参数

+ `callback`：回调函数，有三个参数
  + `currentValue`：当前遍历到的值
  + `index`：可选，当前遍历到的索引
  + `array`：可选，当前遍历数组

+ `thisArg`：可选，指定回调函数的`this`

```js
// 遍历数组
const arr = [1, 2, 3];
arr.forEach((data, index, array) => { //index, array可选
    console.log(data);
});
// 遍历对象
const obj = {a:1, b:2, c:3};
Object.keys(obj).forEach((key)=>{
    console.log(`key:${key} value:${obj[key]}`)
});
```

---

### every&some

这两个方法可以在*中途停止*，并**返回布尔值**

+ [Array.prototypeevery()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every)： 循环在第一次返回`false`后结束，否则返回`true`
  + 即返回是否<u>所有元素</u>都符合条件
  + 对空数组始终返回`true`

+ [Array.prototypesome()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some)： 循环在第一次返回`true`后结束，否则返回`false`；
  + 即返回是否<u>至少1个元素</u>符合条件
  + 对空数组始终返回`false`

```js
// 只打印1
const arr = [1, 2, 3];
arr.every((data) => {
    console.log(data);
    return false;
});
```

### filter&map

这两个方法都会**生成新的数组**

+ [Array.prototypefilter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)：返回一个新数组，该数组内的元素满足回调函数的条件判断；即当函数返回`true`时，元素被添加入新数组

+ [Array.prototypemap()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)： 对原数组的每个元素分别调用一次函数，返回由这些返回值构成的新数组

注意：当你需要一个新数组时，才调用它们；普通的遍历用`forEach`即可

### reduce

[Array.prototypereduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)： 对数组中的元素依次处理，将上次处理结果作为下次处理的输入，并返回最后的结果

包含两个参数：

+ `callback`：需要执行的函数，其包含四个参数：
  + `accumulator`：累计值
  + `currentValue`：当前处理的值
  + `index`：可选，当前处理的索引号
  + `array`：可选，当前处理的数组本身
+ `initialValue`：**可选**，初始值
  + 当初始值为空，调用数组也为空时，将会出错

**注意**：初始值`initialValue`的存在与否，影响循环的次数

存在时：

+ 数组首位元素也调用`callback`函数，实际调用`length`次
+ 首次调用，`accumulator`被设置为初始值，`currentValue`为元素首位
+ `index`从 0 开始计数

不存在时：

+ 数组首位元素不调用`callback`函数，即**首次调用被跳过**，实际调用`length-1`次
+ 首次调用被跳过，直接进入第二次调用；此时`accumulator`被设置为元素首位，`currentValue`为元素次位
+ `index`从 1 开始计数

### find&findIndex

返回***第一个*符合条件**的元素，只返回单体：

+ [Array.prototype.find()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find)：返回元素**本身**
+ [Array.prototype.findIndex()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Glob:al_Objects/Array/findIndex)：返回元素**索引**

---

附：上述函数的简单实现

```js
// 遍历执行
Array.prototype.forEach1 = function(fn) {
  for(let i=0;i<this.length;i++)
    fn(this[i]) 
}
// 遍历执行，保存结果为新的数组
Array.prototype.map1 = function(fn) {
  let newArr = []
  for(let i=0;i<this.length;i++) 
    newArr.push(fn(this[i]))
  return newArr
}
// 遍历执行，累加结果
Array.prototype.reduce1 = function(fn) {
  let sum = this[0]
  for(let i=1;i<this.length;i++) 
    sum = fn(sum, this[i])
  return sum
}
// 遍历执行，返回筛选后的数组
Array.prototype.filter1 = function(fn) {
  let newArr = []
  for(let i=0;i<this.length;i++) 
    fn(this[i]) && newArr.push(this[i])
  return newArr
}
// 遍历执行，直至返回false或遍历完毕
Array.prototype.every1 = function(fn) {
  for(let i=0;i<this.length;i++)
    if(!fn(this[i])) return false
  return true
}
// 遍历执行，直至返回true或遍历完毕
Array.prototype.some1 = function(fn) {
  for(let i=0;i<this.length;i++)
    if(fn(this[i])) return true
  return false
}
```

## `for-of`循环

> for-of 是 ES6 新引入的循环

其他循环的缺点

- `forEach`不能 break 和 return
- `for-in`缺点更加明显
  - 遍历数组中的元素，还会遍历自定义的属性，甚至原型链上的属性都被访问到
  - 遍历数组元素的顺序可能是随机的

`for-of`的特点：

- 除了数组，还可以遍历类数组对象和其他可迭代对象，如`string`、`map`与`set`、函数中的`arguments`、`Dom`集合
- 与`forEach`不同的是，它可以正确响应 break、continue 和 return 语句以关闭迭代器
- **但是不支持普通对象**，普通对象请用`for-in`

```js
const arr = ['a', 'b', 'c'];
for(let data of arr) {
    console.log(data);
}
```

## 中断&停止

> 参考：https://segmentfault.com/a/1190000020176190

|       方法        |    break     |   continue   | return true  | return (false) |
| :---------------: | :----------: | :----------: | :----------: | :------------: |
|     `for循环`     | **结束循环** | 跳出本次循环 |    不合法    |     不合法     |
|    `for...in`     | **结束循环** | 跳出本次循环 |    不合法    |     不合法     |
|    `for...of`     | **结束循环** | 跳出本次循环 |    不合法    |     不合法     |
| `Array.forEach()` |    不合法    |    不合法    | 跳出本次循环 |  跳出本次循环  |
|   `Array.map()`   |    不合法    |    不合法    | 跳出本次循环 |  跳出本次循环  |
|  `Array.some()`   |    不合法    |    不合法    | **结束循环** |  跳出本次循环  |
|  `Array.every()`  |    不合法    |    不合法    | 跳出本次循环 |  **结束循环**  |
| `Array.filter()`  |    不合法    |    不合法    | 跳出本次循环 |  跳出本次循环  |

+ `for`、`for..,in`与`for...of`三种循环，使用`break`结束循环，使用`continue`跳出本次
+ `forEach`、`map`与`filter`方法，使用`return`语句跳出本次循环
+ `some`方法，于`return true`语句后结束循环
+ `every`方法，于`return false`语句后结束循环

## 总结

+ 适用`for`循环：遍历内容不发生改变，长度已知且固定的**对象**与**数组**
+ 适用`for-in`：遍历**对象**，且不要求顺序；遍历**稀松数组**
+ 适用`forEach`：遍历**数组** ，对每个元素执行回调
+ 适用`for-of`：需要*顺序*遍历**数组**时
