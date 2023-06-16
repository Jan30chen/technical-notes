# JavaScript-数组操作

> 参考：https://www.jianshu.com/p/66b04163948b

## 数组方法

### 创建

```js
var arr = new Array();
var arr = new Array(size);
var arr = new Array(element,element...element)
```

第二种方法声明数组长度，但并非其长度限制（实际上所有的数组都是变长的）

### 访问及修改

```js
var arr_value = arr[0];
arr[1] = new_element;
```

### 添加元素

被添加的元素item，可以为一个到多个元素

```js
// 添加元素至尾部，再返回数组长度
arr.push(item)
// 添加元素至开头，原元素依次后移，再返回数组长度
arr.unshift(item)
// 删除指定位置后的0个元素（不删除），再插入新元素，返回被删除的元素（也就是""）
arr.splice(index, 0, item)
```

### 删除元素

```js
// 移除最后一个元素，并返回该元素值
arrayObj.pop();
// 移除最前一个元素，并返回该元素值，数组中元素自动前移
arrayObj.shift();
// 删除从指定位置deletePos开始，指定数量deleteCount的元素，数组形式返回所移除的元素
arrayObj.splice(deletePos,deleteCount);
```

### 截取

```js
// 以数组的形式返回数组的一部分，范围是[start,end)
arrayObj.slice(start, [end])
```

### 合并

```js
// 将多个数组或字符串连接为一个数组，返回连接好的新的数组
arrayObj.concat(item);
// 实例
var a = [1,2,3];
a.concat("4");	//[1,2,3,"4"]
a.concat(4,5);	//[1,2,3,4,5]
a.concat([4,5])	//[1,2,3,4,5]
a.concat("4",5,[6]);	//[1,2,3,"4",5,6]
a.concat([4,[5]])	////[1,2,3,4,[5]]
```

### 拷贝（深）

```js
array.slice();	//返回剪切后的新数组，此处为从头到尾
array.concat();	//返回连接后的新数组，此处为不连接新数组
```

### 排序

```js
arrayObj.reverse(); //反转元素（最前的排到最后、最后的排到最前），修改原数组并返回
arrayObj.sort(); //对数组元素排序，修改原数组并返回
```

### 字符串化

```js
arr.toString();	//带有逗号（分隔符）
arr.join();	//推荐
```

### 查找

```js
arr.indexOf(item);	//查找项目item第一次出现的位置（索引），不存在返回-1
arr.lastIndexOf(item);	//查找项目item最后一次出现的位置，不存在返回-1
```

## 数组属性

### length 属性

+ 表示数组的长度，数组得索引范围为[0, length-1]
+ 可以通过赋值更改：
  + 当被设置得更大时，会多出空元素，实际无影响
  + 当被设置得更小时，多余元素被裁去
+ **不建议直接修改Array的大小，访问索引时要确保索引不会越界**，因为访问越界不会报错

### prototype 属性

用 prototype 属性提供对象的类的一组基本功能，对象的新实例“继承”赋予该对象原型的操作

### constructor 属性

指向自己的构造函数，即Array

```js
arr.constructor == Array; //true
```

## 判断是否为数组

+ ~~`typeof`操作符~~

```js
array instanceof Array //是否为类的实例
```

  ```js
Object.prototype.toString.call(arr).indexOf("Array")
  ```

```js
Array.isArray()
```







