# JavaScript-正则

> 参考：https://juejin.im/post/5b5db5b8e51d4519155720d2#heading-21
>
> 在线正则：https://regex101.com/

## 特殊字符

### 字符匹配

|    字符    |                含义                 |
| :--------: | :---------------------------------: |
|   \d、\D   |          匹配数字和非数字           |
|   \w、\W   | 匹配单字（数字和字母）和匹配非单字  |
|   \s、\S   | 匹配空（空格和制表符tab）和匹配非空 |
|     .      | 匹配所有（匹配`.`使用转义字符`\.`） |
| \f、\n、\r |     匹配换页符、换行符、回车符      |

+ 汉字并不属于`\w`的范畴，一般可以使用`\S`匹配

### 数量匹配

|    字符    |       含义       |
| :--------: | :--------------: |
|     ?      |   匹配0个或1个   |
|     +      |  匹配1个或更多   |
|     *      |   匹配任意数量   |
| {min, max} | 匹配[min, max]个 |
|    {N}     |     匹配N个      |

+ 可以使用缺省的`{min,}`或`{,max}`来匹配不大于或不小于的次数

### 位置匹配

|  字符  |                  含义                  |
| :----: | :------------------------------------: |
|   ^    |           匹配一行的开头位置           |
|   $    |           匹配一行的结尾位置           |
| \b、\B | 匹配单词边界和空格间的位置和非上述位置 |

`\b`的匹配位置：

<img src=".\MDimg\reg\reg2.jpg" style="float:left;" />

`\B`的匹配位置：

<img src=".\MDimg\reg\reg1.jpg" style="float:left;" />

### 断言匹配

|   字符   |       含义       |
| :------: | :--------------: |
| (?=...)  |   正向零宽断言   |
| (?!...)  | 正向否定零宽断言 |
| (?<=...) |   反向零宽断言   |
| (?<!...) | 反向否定零宽断言 |

零宽断言：

+ 断言语句为整体，不可拆分使用，使用`?`+(`<`)+`=/!`+`...`的形式，其中`...`代指所给内容

+ 匹配紧挨着**所给内容**的位置，分为正反向：

  + 正向：所给内容**之前**的位置

  <img src=".\MDimg\reg\零宽断言1.jpg" style="float:left;" />

  + 反向：所给内容**之后**的位置

  <img src=".\MDimg\reg\零宽断言2.jpg" style="float:left;" />

+ 将`=`换为`!`，则反选匹配位置：

  + 正向否定：

  <img src=".\MDimg\reg\零宽断言3.jpg" style="float:left;" />

  + 反向否定：

  <img src=".\MDimg\reg\零宽断言4.jpg" style="float:left;" />

### 选择符

| 字符 | 含义                                                         |    例     | 含义                         |
| :--: | ------------------------------------------------------------ | :-------: | ---------------------------- |
|  //  | 标识在该符号中为一个正则                                     |    /a/    | 匹配a的正则                  |
|  []  | 表示**或**逻辑（单个字符），并且其中的转义字符不需要转义（例如直接用`.`匹配句号） | [ab]；[.] | 匹配a或b；匹配句号           |
|  -   | 在开头，表示匹配连字符`-`；不在开头，表示连字符              |   [a-z]   | 匹配a到z的字母、匹配小写字母 |
|  ^   | 不在`[]`中匹配位置；在`[]`中表示**非**逻辑                   |   [^ab]   | 匹配**除了a和b**的字符       |
|  \|  | 与`()`组合表示**或**逻辑                                     | (ab\|ba)  | 匹配ab或ba字符               |

### 转义符

需要匹配上述选择匹配符、位置匹配符等特殊符号时，使用`\`进行转义

### 附加标识-flag

放在整个正则式的后面，如`/w+/g`

| 标识 |              含义              |
| :--: | :----------------------------: |
|  g   |      匹配全部符合的字符串      |
|  i   |          不区分大小写          |
|  m   | 多行匹配，每行都带有句尾与句首 |

## 分组捕获

### 分组

使用`()`对正则进行分组

```js
var r = /\d{3}-(\d{3})-(\d{4})/	//分为两组
var tel = 212-555-1234
//分组，Group0为整个正则
//212-555-1234   Group0
//555            Group1
//1234           Group2
```

### 使用分组

#### 替换时使用

用`$1`、`$2`等表示分组

#### 正则本身中使用

用`\1`、`\2`等表示分组

```js
// 匹配连续两个相同字符
var s = "is is"
var r = /(\w+)\s\1/
r.test(s) //true
```

#### 只分组不捕获

在分组中（括号中）使用`?:`，表示此处只分组，但不会被捕获

如下例子中

+ 第一个括号中的内容无法获取，也不参与计数
+ 第二个括号，通过`$1`捕获，而非`$2`

```js
/(?:\d{1,3}\.){3}(\d{1,3})/
```

## `.*`贪婪匹配

`.*`通配符是**贪婪的**，会匹配到最后一个满足的符号，在其后加`?`可以禁止贪婪

```js
var s = "[123][456]"
var r1 = /\[.*\]/	//贪婪匹配
var r2 = /\[.*?\]/	//非贪婪匹配
s.replace(r1,"abc")		//abc，全部被替换
s.replace(r2,"abc")	//abc[456]，只有前半部分被替换
```

## JavaScript中的正则函数

### 新建正则

```js
/ab+c/i					//	直接声明
new RegExp('ab+c', 'i')	//	函数声明1
new RegExp(/ab+c/, 'i')	//	函数声明2
```

### `reg.test()`

正则函数，检测字符串是否符合正则，返回布尔值

```js
var r = /\d{3}/;
var a = '123';
var b = '123ABC';
var c = 'abc';

r.test(a)  	//	true
r.test(b) 	//	true
r.test(c) 	//	false
```

### `str.match()`

字符串函数，返回数组，包含符合正则的**第一个**字符串（不加标识符），全局加`g`

```js
var r1 = /compus/
var r2 = /\w+/
var r3 = /\w+/g
var str = "compus, I know something about you"

str.match(r1)  	//	["compus"]
str.match(r2) 	//	["compus"]
str.match(r3)	//	["compus", "I", "know", "something", "about", "you"]
```

### `reg.exec()`

正则函数，每次调用返回**下一个**匹配结果数组（包含本次结果的各种信息，非全部结果），直到为`null`，**类似迭代器**

```js
var str = "Here is a Phone Number 111-2313 and 133-2311" ;
var srg = /(\d{3})[-.]\d{4}/g;
var result = srg.exec(str);	//  返回["111-2313", "111", index: 23, input: "Here is a Phone Number 111-2313 and 133-2311", groups: undefined]
while(result !== null) {
    console.log(result);
    result = srg.exec(str);
}
```

### `str.split()`

字符串函数，按照某个标识符（正则）划分字符串成为数组

```js
var s = "unicorns and rainbows And, Cupcakes"
var result1 = s.split(' ');
//["unicorns", "and", "rainbows", "And,", "Cupcakes"]
var result2 = s.split(/\s/);	
//["unicorns", "and", "rainbows", "And,", "Cupcakes"]
var result3 = s.split(/[,\s]/);	
//["unicorns", "and", "rainbows", "And", "", "Cupcakes"]
```

### `str.replace()`

字符串函数，不修改字符串而返回**新字符串**，全局替换需要添加`g`

基本用法是`newStr = str.replace(reg, replace|function)`：

+ reg：正则
+ replace：待替换字符串
+ function：替换函数

#### 使用替换字符

替换所有元音字母为两倍

```js
var s = "Hello,My name is Vincent."
var result = s.replace(/([aeiou])/g,"$1$1")
//"Heelloo,My naamee iis Viinceent."
```

#### 使用回调函数

长度为4的单词改为全部大写

```js
var s = "Hello,My name is Vincent. What is your name?"
var newStr = s.replace(/\b\w{4}\b/g,replacer)
console.log(newStr)
function replacer(match) {
    console.log(match);
    return match.toUpperCase();
}
/*
name
What
your
name
Hello,My NAME is Vincent. WHAT is YOUR NAME?
*/

```

当正则有分组时，回调函数的参数第一个为总正则，后面依次为各个**组正则**

```js
function replacer(match,group1,group2) {
    console.log(group1);
    console.log(group2);
}
```

## 常用正则

> 参考：https://juejin.cn/post/6999768570570178596

### 查找

匹配手机号码：第一位为1，第二位为3-9范围，剩余9位数字

```js
let reg = /^[1]([3-9])[0-9]{9}$/
```

匹配身份证号：十五位数字，或前十七位为数字，最后一位为数字或"X"，没有对中间日期做校验

```js
let reg = /^(\d{15}$)|(\d{17}(\d|X))$/i
```

是否包含中文

```js
const checkChineseRegex = /[\u4E00-\u9FA5]+/
```

密码校验：六位及以上，至少包含大写字母、小写字母与数字中的两种

```js
let reg = /((?=.*\d)(((?=.*[A-Z])|(?=.*[a-z])))|((?=.*[A-Z])(?=.*[a-z])))^[a-zA-Z\d]{6,}$/
```

### 替换

数字添加千分号（非浮点数）：

```js
let reg = /(?!^)(?=(\d{3})+$)/g
```

数字添加千分号（通用）：

> 123456789 => 123,456,789
>
> 123456789.123 => 123,456,789.123

```js
const formatNum = (numStr) => {
    const regex = new RegExp(`(?!^)(?=(\d{3})+${numStr.includes('.')?'\\.':'$'})`, 'g')
    return numStr.replace(regex, ',')
}
```

手机号分隔：

> 17800000000 => 178-0000-0000

```js
let mobileReg = /(?=(\d{4})+$)/g
mobile.replace(mobileReg, '-')
```

驼峰变换

```js
const hump = (str) => {
    let reg = /[\s-_]+(\w)/
    return str.replace(reg, (match, group1) => {
      return group1.toUpperCase()
    })
}
```

获取网址的参数

```js
const getValue = (url, name) => {
  const reg = new RegExp(`[?&]${name}=([^&]*)(&|$)`)
  return url.match(reg)[1]
}
```

获取img标签中的图片路径

```js
const getPath = (dom) => {
  const reg = /<img[^>]+src="((?:https?:)?\/\/[^"]+)"[^>]*?>/gi
  let imgs = []
  dom.replace(reg, (match, group1) => {
    console.log(group1);
    group1 && imgs.push(group1)
  })
  return imgs
}
```

转义函数

```js
const escapeHTML = str =>
  str.replace(
    /[&<>'"]/g,
    tag =>
      ({
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        "'": '&#39;',
        '"': '&quot;'
      }[tag] || tag)
  );
```



