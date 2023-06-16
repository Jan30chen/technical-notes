# JavaScript-字符串操作

> 参考：https://juejin.cn/post/6913691919386312712

## 重复字符串

使用`str.repeat(num)`对字符串进行重复，返回值为重复后的字符串

```js
's'.repeat(5)	//'sssss'
```

## 填充字符串

+ `str.padStart(num, pad)`：使用填充符，将字符串**从开头填充**至指定长度
+ `str.padEnd(num, pad)`：使用填充符，将字符串**从末尾填充**至指定长度

## 拆分为字符数组

使用扩展操作符

```js
[...'apple']	// ["a", "p", "p", "l", "e"]	
```

使用`split()`函数

```js
'apple'.split('')	// ["a", "p", "p", "l", "e"]	
```

注意分隔符为空字符串，而不是为空

## 获取字符长度

一般使用`length`属性即可

```js
'apple'.length	//5
```

对于有些生僻字，可能需要先变为字符数组，再获取长度

```js
'𩸽'.length			//2
[...'𩸽'].length	//1
```

## 反转字符串

转化为字符数组，使用`reverse()`函数，最后拼接起来

```js
[...'god'].reverse().join("")	//dog
```

## 首字母大写

使用下标获取首字母，使用`substr()`等截取字符串函数获取到结尾，拼接

```js
'peach'[0].toUpperCase() + 'peach'.substr(1)
```

PS：截取函数的使用

> + `str.slice(begin, end)`：截取并返回，下标在 [begin, end) 范围的字符串
>   + 单个参数表示从该下标获取至末尾
>   + begin > end 时，获得一个空字符串
>   + 负数代表从后获取下标
>
> + `str.substring(begin, end)`：截取并返回，下标在 [begin, end) 范围的字符串
>   + 单个参数表示从该下标获取至末尾
>   + begin > end 时，交换它们进行获取
>   + 负数会被视为 0
> + `str.substr(begin, length)`：截取并返回，从下标 begin 开始长度为 length 的字符串

PPS：使用 css 属性

> `text-transform`属性设置文本的大小写，根据不同场景选择 css 与 js 方法

## 在字符串中检查特定序列

### 包含

使用`includes()`函数

```js
'Hello, world!'.includes('Hello')	//true
'Hello, world!'.includes('Hi')	//false
```

### 以序列开头

使用`startsWith()`函数，注意start有s

```js
'Hello, world!'.startsWith('Hello')	//true
```

### 以序列结尾

使用`endsWith()`函数，同样注意s

```js
'Hello, world!'.endsWith('world')	//false，少了感叹号
```

## 替换所有出现序列

使用`replace()`函数，搭配全局查找的正则表达式`/g`

```js
"I like apples. You like apples.".replace(/apples/g, "bananas")	// "I like bananas. You like bananas."
```

使用`replaceAll()`函数，但只支持较新版本浏览器

```js
"I like apples. You like apples.".replaceAll("apples", "peach")	// "I like peach. You like peach."
```

## 移除字符前后空白

使用`trim()`及所属函数，移除字符前后的空白（空白字符、行终止符字符）；不改变原字符

+ `str.trim()`：移除字符前后的空白
+ `str.trimStart()/trimLeft()`：移除字符前的空白
+ `str.trimEnd()/trimRight()`：移除字符后的空白

```js
"   water melon   ".trim()	//"water melon"
```