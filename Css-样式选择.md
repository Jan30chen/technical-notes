# Css-样式选择

> 参考：
>
> https://www.w3school.com.cn/cssref/css_selectors.asp
>
> https://juejin.cn/post/6844904087012507656

以下介绍CSS的选择器，关于样式的继承性详见[此处](./Css-样式继承.md)

## 基础选择

| 选择器     | 例子       | 范围                            |
| ---------- | ---------- | ------------------------------- |
| 标签选择器 | `p`        | 所有`<p>`元素标签               |
| 类选择器   | `.myClass` | 类名为`myClass`的元素           |
| id选择器   | `#myId`    | ID 为`myId`的元素               |
| 兄弟选择器 | `div+p`    | 作为`<div>` 同级元素的`<p>`元素 |
| 子代选择器 | `div>p`    | 作为`<div>`子代元素的`<p>`元素  |
| 后代选择器 | `div p`    | 作为`<div>`后代元素的`<p>`元素  |
| 前后选择器 | `ul~p`     | 前面有`<ul>`元素的`<p>`元素     |

## 条件选择

| 选择器   | 例子       | 范围                               |
| -------- | ---------- | ---------------------------------- |
| 全选择器 | `*`        | 全选                               |
| 或选择器 | `p, div`   | 同时选择所有`<p>`或`<div>`标签元素 |
| 与选择器 | `p.class1` | 具有`class`类名的`<p>`标签元素     |

## 属性选择

使用属性选择器来选择带有某些属性的元素

并且可以使用这些关键词，进行部分检索：^, $, *

| 选择器          | 例子              | 范围                                  |
| --------------- | ----------------- | ------------------------------------- |
| 属性选择器①     | `[target]`        | 有`target`属性的元素                  |
| 属性选择器②     | `[target=_blank]` | 有`target`属性，且值为`_blank`的元素  |
| 属性选择器-开头 | `[href^="https"]` | `href`属性的值以`https`**开头**的元素 |
| 属性选择器-结尾 | `[href$=".pdf"]`  | `href`属性的值以`.pdf`**结尾**的元素  |
| 属性选择器-包含 | `[href*="baidu"]` | `href`属性的值**包含**`baidu`的元素   |

## 伪类选择&伪元素

### 伪类

伪类定义了一些元素的特殊样式

+ 处于某种特殊状态的元素（被选中、被悬浮、获得焦点等）
+ 符合某些条件的集合元素（无效元素、子元素等）

#### 状态伪类

```css
a:link {color:#FF0000;} 	/* 未访问的链接 */
a:visited {color:#00FF00;} 	/* 已访问的链接 */
a:hover {color:#FF00FF;} 	/* 鼠标悬浮时的链接 */
a:active {color:#0000FF;} 	/* 已点击过的链接 */
```

**注意：**同时具有这些伪类的元素，需要注意**书写顺序**：

+ `a:hover`必须被置于`a:link`和`a:visited `之后，才是有效的
+ `a:active`必须被置于`a:hover`之后，才是有效的

#### 函数伪类

> 选择器列表：以逗号分隔的多个选择器；如`span, div, p`
>
> 可容错选择器列表：选择器列表的部分错误会导致整条规则失效；可容错的列表则会跳过错误的部分

接受一个可容错选择器列表作为参数，并返回处理结果；其优先级为参数中的选择器

+ [`:is()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is)：匹配列表中所匹配到的元素；可以用于分发多个子选择器以相同的样式
+ [`:not()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not)：匹配列表中**未**匹配到的元素
+ [`:where()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:where)：匹配列表中所匹配到的元素，但将**优先级降为0**
+ [`:has()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:has)：如果存在满足匹配列表的元素，设置自身的样式

例：

```css
/* :is */
div :is(p, a) { /* … */ }
div p, div a { /* … */ }	/* 分发到参数中每个选择器 */

/* :has */
h1, h2 { margin: 0 0 1.0rem 0; }		/* 底部间距为1 */
h1:has(+ h2) { margin: 0 0 0.25rem 0; }	/* 如果h1后紧跟着h2元素，那么h1底部间距为0.25 */
```

其他：

+ 在`vue`带有`scoped`属性的css中，使用`:is`渲染的选择器不会加上额外的标识符；见 [巧用:is()或:where()伪类让scoped的style依然全局匹配](https://www.zhangxinxu.com/wordpress/2022/09/css-is-where-scoped-style/)
+ 降低选择器优先级：
  + 使用`:where`伪类
  + 使用[`@layer`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@layer)规则

#### 位置伪类

前缀：

+ `first-`：选择首元素
+ `last-`：选择末元素
+ `nth-`：下标选择元素
+ `only-`：选择唯一元素

后缀：

+ `-child`：选择其父元素的子元素
+ `-of-type`：指定其父元素指定类型的子元素

组合如下：

| 选择器                  | 作用                                          |
| :---------------------- | :-------------------------------------------- |
| `p:first-child`         | 选择p元素，且是其父元素的第一个子元素         |
| `p:first-of-type`       | 选择p元素，且是其父元素的第一个p类型子元素    |
| `p:last-child`          | 选择p元素，且是其父元素的最后一个子元素       |
| `p:last-of-type`        | 选择p元素，且是其父元素的最后一个p类型子元素  |
| `p:nth-child(n)`        | 选择p元素，且是其父元素的第n个子元素          |
| `p:nth-of-type(n)`      | 选择p元素，且是其父元素的第n个子元素          |
| `p:nth-last-child(n)`   | 选择p元素，且是其父元素的倒数第n个子元素      |
| `p:nth-last-of-type(n)` | 选择p元素，且是其父元素的倒数第n个p类型子元素 |
| `p:only-child`          | 选择p元素，且是其父元素唯一一个子元素         |
| `p:only-of-type`        | 选择p元素，且是其父元素唯一一个子元素         |

**注意**：

+ 所谓第几个元素，是该元素于**兄弟元素之间**的位置，而非选择该元素下的第几个元素
+ 所谓p类型是指元素标签类型，附加的类名、ID，并不能使用`-of-type`后缀选择器
  + 如`.small:first-of-type`，是不能选择所有类名为`small`中的第一个元素的
  + 上述用法，实际相当于`*.small:first-of-type`，选择了不限类型中的元素中的第一个，且类名为`small`

提示：

`nth-`可以使用如下值：(an+b)

+ a、b都要为整数，且（n=0，1，2，3...）
+ 寻找奇数号元素：`:nth-child(2n+1)`，可写为`:nth-child(odd)`
+ 寻找偶数号元素：`:nth-child(2n)`，可写为`:nth-child(even)`

#### 表单伪类

+ `:checked`：被选中的表单元素
+ `:disabled`：禁用的表单元素
+ `:enabled`：启用的表单元素
+ `:focus`：获得焦点的表单元素

#### 其他伪类

+ [:root](https://www.w3school.com.cn/cssref/selector_root.asp)：根元素
+ [:empty](https://www.w3school.com.cn/cssref/selector_empty.asp)：没有子元素的元素（只包含文本也算）
+ [::selection](https://www.w3school.com.cn/cssref/selector_selection.asp)：被用户选中的元素，定义后会覆盖原有默认样式（蓝底白字）

### 伪元素

不同于伪类，伪元素会渲染实际的元素

+ `::before`：在被选择的元素内，**开始处**插入伪元素
+ `::after`：在被选择的元素内，**结束处**插入伪元素

**注意：**伪元素是被选中元素的子元素，而非同级元素

其他：

+ 使用伪元素搭配`content`属性，决定插入元素的内容

+ 也常常使用伪元素`::after`搭配`clear`属性，在浮动元素中添加空元素清除浮动

+ 为了兼容旧版本，也可以使用单冒号，但是推荐使用**双冒号**便于区分伪类和伪元素
