# JavaScript-JSON

## 定义

JSON(JavaScript Object Notation)：是一种轻量级的数据交换格式，采用完全独立于语言的文本格式，是理想的数据交换格式

## 作为字符串和对象

JSON可以以**数组传递**，也可以以**对象传递**：

+ 在数据传输流程中，JSON是以文本，即字符串的形式传递的

+ 而 JavaScript 操作的是JSON对象
+ 所以，JSON对象和JSON字符串之间的相互转换是关键

```js
// 分别声明JSON字符串与对象
var jsonStr = '{ "name": "haorooms", "sex": "man" }'
var jsonObj =  { "name": "haorooms", "sex": "man" }
// 检查类型
Object.prototype.toString.call(jsonStr)	// outcome:"[object String]"
Object.prototype.toString.call(jsonObj)	// outcome:"[object Object]"
```

## 转化

> 参考：https://www.hangge.com/blog/cache/detail_1534.html

### 字符串到对象

首先新建一个JSON字符串

```js
var jsonStr = '[{"CityId":18,"CityName":"西安"},{"CityId":53,"CityName":"广州"}]'
```

#### JSON.parse()

```js
var jsonObj = JSON.parse(jsonStr);
```

+ ie8(兼容模式)、ie7与ie6不适用

#### json2.js

```js
var jsonObj = JSON.parse(jsonStr);
```

+ json2.js 提供了 json 的序列化和反序列化方法，完美支持各个浏览器
+ 使用时我们首先要将 json2.js 引用进来，源码地址：https://github.com/douglascrockford/JSON-js

#### jQuery

```js
var jsonObj = $.parseJSON(jsonStr);
```

项目中有使用 **jQuery**，直接使用 $.parseJSON() 方法

#### eval()

```js
var jsonObj = eval( '(' + jsonStr + ')');
```

- 使用 eval() 转换时需要在 json 字符外包裹一对小括号。
- ie8(兼容模式)、ie7与ie6不适用

### 对象到字符串

首先新建一个JSON对象

```js
var jsonObj = {
  "CityId":"18",
  "CityName":"西安2"
};
```

#### JSON.stringify()

```js
var jsonStr = JSON.stringify(jsonObj);
```

+ ie8(兼容模式)、ie7与ie6不适用

---

[JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 方法可以将 Javascript 对象（包括数组、值）序列化为 JSON 字符串

参数：该方法接受至多三个参数

+ value：需要序列化的对象
+ replace：可选，序列化之前执行的操作
  + 如果为函数，则对象的每个属性都会被该函数处理与替换
  + 如果为数组，则数组中含有的属性名才会被序列化
+ space：可选，指定缩进用的占位字符串
  + 如果为数字，则占位符为相应长度的空白字符串，上限为0
  + 如果为字符串，则视为占位符，超过10位的部分丢弃

注意：

+ 值调用`toJSON()`方法被转化，日期类型值等同于调用`Date.toISOString()`方法
+ `null`与`NaN`都转化为`null`
+ 字符串、数字与布尔值的包装量，都会自动转化为对应的原始值
+ 对于`undefined`、函数或`Symbol`类型
  + 作为对象中的值出现，序列化将跳过该键，不出现于 JSON 中；这会导致后端对键取值时发生错误，需要传递未定义量时，请提前置为空字符串`''`
  + 出现在数组中，被转化为`null`（实际变为字符串）
  + 被单独转换，返回`undefined`（非字符串，即返回未定义对象）

针对对象中`undefined`被跳过的问题，使用`replace`参数提前置换掉来解决

```js
const obj = { a:1, b:undefined }
JSON.stringify(obj)	 // '{"a":1}'
JSON.stringify(obj, (key, value) => {	//	'{"a":1,"b":""}'
  if(typeof value === 'undefined')
    return ''
  return value
})
```

#### json2.js

```JS
var jsonStr = JSON.stringify(jsonObj);
```

+ 支持旧版本

