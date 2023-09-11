# Html

## Html-空格

+ `&#32;`：英文半角空格，行尾换行
+ `&#160;`,`&nbsp;`,`&#xA0;`：英文半角空格，行尾不换行
+ `&#8197;`：四分之一中文宽度
+ `&#8194;`,`&ensp;`：半个中文宽度
+ `&#8195;`,`&emsp;`：中文宽度

## Html-元素嵌套规则

1. 块元素可以包含行内元素、块元素；行内元素只能包含其他行内元素
2. `<p>`标签、`<h>`标签中，不作为容器使用，不能包含其他块元素
3. `<li>`标签可以很大，可以作为块元素容器使用
4. 并列的兄弟元素应该同为块元素或行内元素

## Html-lang属性

在Html5中，可以使用于任何标签，用于指明其内容的使用语言

最常使用于`<html>`标签，用于指明当前页面使用了什么语言

简体中文一般使用`zh-CN`代码，更准确可以使用`zh-cmn-Hans`；英文使用`en`

## Html-`data-*`属性

在html标签上，我们可以自定义属性名并赋值，并可以于脚本（JavaScript）中获取该属性；该属性被称为[全局属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/data-*)，以`data-*`为前缀

Html标签添加属性：

```html
<div id="div1" data-div-id="value1"></div>
```

JavaScript 中，使用[`HTMLElement.dataset`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)获取属性值：

```js
document.getElementById('div1').dataset.divId
```

**注意：**html上，属性使用**短横线**命名法；而在 Javascript 中，属性为**驼峰**命名法

## Html-`Table`边框

外边框-[`frame`](https://www.w3school.com.cn/tags/att_table_frame.asp)属性：

+ void：无
+ 单边：above：上；below：下；lhs：左；rhs：右
+ 双边：hsides：上下；vsides：左右
+ 四边：box；border

内分割线-[`rules`](https://www.w3school.com.cn/tags/att_table_rules.asp)属性：

+ none：无线条
+ rows：展示横线
+ cols：展示竖线
+ all：展示所有

# Css

## Css-布局

> 参见：[介绍 CSS 布局](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Introduction)

布局分类：

+ 正常布局流：浏览器默认布局方式，在无Css的情况下，所采用的布局；
+ `display`属性：改变元素默认的布局方式，或者定义为特殊的布局方式；元素仍然处于正常布局流中
  + 块、内联、行内块
  + 弹性、行内弹性
  + 网格、行内网格
+ `float`属性：使元素浮动，从正常布局流中将元素移除，
+ `position`属性：使元素在正常布局流中所处的位置，移动到另一个位置；可能会脱离布局流
+ Css 表格布局：在非`<table>`标签上，应用table布局属性
+ 多列布局：使用 [`column-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count)和[`column-width` ](https://developer.mozilla.org/en-US/docs/Web/CSS/column-width)属性，将内容分为几列和控制它们的宽度

## Css-隐藏元素

|     类型     |                        属性                        | 是否渲染 | 是否影响子代 |                其他说明                |
| :----------: | :------------------------------------------------: | :------: | :----------: | :------------------------------------: |
| Html标签属性 |                      `hidden`                      |    ×     |      √       |   会被 Css 的`display`属性**覆盖**掉   |
|   CSS属性    |                  `display: none`                   |    ×     |      √       |         可以在 Dom 树上被获取          |
|   CSS属性    |         `opacity: 0`、`filter:opacity(0)`          |    √     |      √       |      元素存在，但整体不透明度为 0      |
|   CSS属性    |                `visibility: hidden`                |    √     |      √       | 同上，子元素可以重写为` visible`以展示 |
|   CSS属性    | `background: (X,X,X,0)`、`background: transparent` |    √     |      ×       |      元素存在，父元素背景为透明色      |

## Css-清除浮动

> 参考：https://segmentfault.com/a/1190000004865198

### 清除浮动的定义

在非IE浏览器（如Firefox）下，当容器的高度为auto，且容器的内容中有浮动（float为left或right）的元素，在这种情况下，容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象，叫**浮动溢出**

为了防止这个现象的出现而进行的Css处理，就叫**Css清除浮动**。

![图片](http://images.cnitblog.com/blog/349636/201310/23224343-9668661a8f63445699e0a8c24a64662b.jpg)

### 清除浮动的方法

> `clear`属性：定义元素的哪一边框不与浮动元素相接触

+ 方法一：在父容器中，添加带`clear:both`属性的空元素
+ 方法二：为父容器添加`overflow:hidden`或`overflow:auto`属性；IE6中要额外添加`zoom:1`或设置高度；原理是使父容器具有 BFC
+ 方法三：使父容器也变为浮动元素
+ 方法四：浮动元素后的元素直接添加`clear:both`属性
+ **方法五（推荐）：使用Css的`:after`伪元素，设置为不可见块元素并添加`clear:both`属性**

```css
.clearfix:after{
  content: "020"; 
  display: block; 
  height: 0; 
  clear: both; 
  visibility: hidden;  
}
.clearfix {
  /* IE6触发 hasLayout */ 
  zoom: 1; 
}
```

## Css-顶部塌陷

> 参考：https://blog.csdn.net/weixin_40612082/article/details/80461818

当父元素中，最顶上的子元素设置了`margin-top`后，这部分外间距并不会被算入父元素中，导致父元素缺失该部分的高度

实际上，底部也会出现同样的塌陷现象，但是左右不会出现这种情况

---

如图所示，分别为塌陷的父元素、子元素的实际大小与添加边框后不再塌陷的图片

![](https://i.loli.net/2021/05/17/9IqGf3ut8UHNCmw.png)![](https://i.loli.net/2021/05/17/tPbhgLuYiR1aC5W.png)![](https://i.loli.net/2021/05/17/hBmJGPSQC3EzM9i.png)

### 解决

+ 父元素添加`border`属性，最小1px；同理添加`padding`亦可
  + `border`颜色可设置为`transparent`，透明
+ 父元素添加伪元素（影响小）：

```css
.parent::before{
	content: "";
	display: table;
}
```

+ 父元素设置`display: table`
+ 父元素添加`overflow: hidden`
+ 父元素添加`position: fixed`

## Css-超出省略

### 单行

```css
div{
    width:200px; 
    border:1px solid #000000;
    /* 超出隐藏、不换行、显示省略号 */
    overflow:hidden;
    white-space:nowrap; 
    text-overflow:ellipsis;
}
```

### 多行（兼容性欠佳）

```css
div {
  overflow: hidden;
  text-overflow: ellipsis;
  /* 最多多少行 */
  -webkit-line-clamp: 2;
  /* 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 */
  display: -webkit-box;
  /* 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 */
  -webkit-box-orient: vertical;
}
```

## Css-选择器

### 选择器种类

> 更多参考：https://www.w3school.com.cn/cssref/css_selectors.asp

| 选择器               | 举例                            | 范围                                                         |
| -------------------- | ------------------------------- | ------------------------------------------------------------ |
| 类选择器             | `.myClass`                      | 类名为 myClass 的元素                                        |
| id选择器             | `#myId`                         | Id为 myId 的元素                                             |
| 标签选择器           | `p`                             | `<p>`元素标签                                                |
| 兄弟选择器           | `div+p`                         | 作为`<div>` 同级元素的`<p>`元素                              |
| 子选择器             | `div>p`                         | 作为`<div>`子代元素的`<p>`元素                               |
| 后代选择器           | `div p`                         | 作为`<div>`后代元素的`<p>`元素                               |
| 通配符选择器         | `*`                             | 全选                                                         |
| 属性选择器           | `[target]`、`[target="_blank"]` | ①带有 target 属性的元素；②带有 target 属性，且值为`_blank`的元素 |
| 伪类选择器           | `a:hover`                       | a 元素，且处于被鼠标悬浮状态                                 |
| 伪元素选择器         | `a::before`                     | a 元素中插入伪元素内容至尾部                                 |
| 排除选择器(反选伪类) | `:not(p)`                       | 选择非 p 元素                                                |
| 索引选择器           | `:nth-child(an+b)`              |                                                              |

索引选择器是css伪类：  `:nth-child(an+b)`

+ 寻找范围为当前元素的所有**兄弟元素**，也就是说，要保证*所要查找的元素们在同一祖先元素中*
+ a、b都要为整数，且（n=0，1，2，3...）
+ 寻找奇数号元素：`:nth-child(2n+1)`，可写为`:nth-child(odd)`
+ 寻找偶数号元素：`:nth-child(2n)`，可写为`:nth-child(even)`
+ 寻找前m位元素：`:nth-child(-n+m)`，原理是负号结果无效

### 选择器优先级

以下优先级由高到低：

+ `!important`属性
+ 标签内联`style`属性
+ `id`选择器
+ `class`选择器
+ `tag`选择器
+ 继承来的属性
+ 通配符

## Css-各类形状

### 三角形

> 原理：实现一个宽高为零，边框`border`为一定长度的方块，将两或三条边设为透明像素，得到一个三角形

```css
.triangle{
    width: 0;
    height: 0;
    /* transparent = RGBA(0,0,0,0) = 全透明黑色 */
    border-top: 40px solid transparent;
    border-left: 40px solid transparent;
    border-right: 40px solid transparent;
    border-bottom: 40px solid #ff0000;
}
```

### 平行四边形

> 原理：使用transform属性的[skew](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/skew)方法，将矩形元素倾斜；如果内部元素不需要倾斜，使用相反度数再倾斜回来

```css
.rhomboid {
	width: 100px;
	height: 30px;
	transform: skew(-15deg);
    background-color: pink;
}
.rhomboid > .content {
    transform: skew(15deg);
}
```

## Css-BFC

> 参考：https://zhuanlan.zhihu.com/p/25321647

### BFC概念

Formatting context(格式化上下文) 是 W3C Css2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用

BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。

**具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。**

### BFC触发

满足**任意一条**即可触发：

1. 根元素`<html>`
2. 浮动元素：`float`属性设置为除`none`以外的值
3. 行内块元素`display: inline-block`；该属性相当于`display: inline flow-root`，`flow-root`属性会生成块级元素盒，建立新的BFC
4. 绝对定位元素：`position`设置为`absolute`或`fixed`
5. `display`为``table-cell`、`flex`或是`grid`
6. `overflow`除了默认值`visible`以外的值，如`hidden`、`auto`或`scroll`

**提示**：对于需要触发BFC的元素，2~3条可以直接设置于元素本身，生成BFC；4~6条需要为元素外加一个父元素，将属性设置于父元素上方才有效

### BFC特性

#### margin重合

同一个BFC中，上下元素margin重合，导致间距小于两者margin之和

```css
/* 各有100px间距，但由于发生重合，实际两者间距为100px */
#div1{
    width: 100px;
    height: 100px;
    background: red;
    margin-bottom: 100px;
}
#div2{
    width: 100px;
    height: 100px;
    background: green;
    margin-top: 100px;
}
```

将它们放在不同的BFC中，可以解决这个问题

#### 清除浮动

容器触发BFC后，将会包裹着浮动元素，借此清除浮动

#### 避免覆盖

当浮动元素A与普通元素B在一处时，A会覆盖在B上（不会覆盖B上的文字）

<img src="https://pic4.zhimg.com/80/v2-dd3e636d73682140bf4a781bcd6f576b_720w.png" alt="img" style="zoom: 50%;" />

如果让B触发BFC，则不会被覆盖

<img src="https://pic3.zhimg.com/80/v2-5ebd48f09fac875f0bd25823c76ba7fa_720w.png" alt="img" style="zoom:50%;" />

## Css-媒体查询

### 媒体查询

#### 定义基础属性

首先规定属性，`width = device-width`：宽度等于当前设备的宽度

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

属性说明：

+ `viewport`：虚拟视口，移动端浏览器生成的一块渲染区域，会自动生成一块尺寸比实际大小更宽的区域（如果后续没有指定width），网页内容在此区域进行渲染，再进行缩小以适应屏幕；此举是为了解决未针对移动端优化的网页在移动端上的显示问题。
+ `width=device-width`：设置viewport的宽度为设备实际宽度，即渲染区域等于实际屏幕
+ `initial-scale=1.0`：设置最初加载时的缩放比例，默认为1
+ `maximum-scale=1.0`：设置用户放大的最大比例为1
+ `user-scalable=no`：设置不允许用户手动缩放

媒体类型：描述该规则用于何种设备，可选

+ `all`：默认值，所有设备
+ `screen`：适用于屏幕（正常浏览模式下）
+ `print`：适用于打印预览模式

#### 使用

##### CSS中

Css文件中：使用`@media`关键字，后接媒体类型、媒体查询规则

```css
/* 浏览器宽度等于960像素时执行 */
@media screen and (max-device-width:960px){
    body{
        background:red;
    }
}
/* 浏览器宽度小于960像素时执行 */
@media screen and (max-width:960px){
    body{
        background:red;
    }
}
/* 浏览器宽度大于1200像素时执行 */
@media screen and (min-width:1200px){
    body{
        background:orange;
    }
}
/* 定义960~1200像素范围内使用 */
@media screen and (min-width:960px) and (max-width:1200px){
    body{
        background:yellow;
    }
}
```

**注意顺序：**

+ 使用`min-width`时，数值由小到大依次列出
+ 使用`max-width`时，数值由大到小依次列出
+ 最好的方法还是指定区间，避免某些样式被其他样式覆盖

**逻辑操作符说明：**

`and`：当每条查询都为真时，该媒体查询才为真

`not`：否定媒体查询，新版本会否定至`,`前，旧版本只能否定整条查询；必须规定媒体类型

`only`：防止早期浏览器忽略`and`后的查询条件；必须规定媒体类型

`,`：当多条查询任意条为真时，该媒体查询为真

##### 样式标签上

使用`media=`指定

```html
<!-- 当视口宽度不大于320像素时，使用x.css；不小于321像素且不大于375像素时，使用y.css -->
<link rel="stylesheet" type="text/css" href="x.css" media="only screen and (max-width:320px)"/>
<link rel="stylesheet" type="text/css" href="y.css" media="only screen and (min-width:321px) and (max-width:375px)"/>
```

##### Javascript中

> 媒体查询字符串：如`'(max-width: 600px)'`

使用[`matchMedia()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/matchMedia)方法，可以获取到当前窗口是否满足某个媒体查询字符串的条件

### 容器查询

根据元素容器大小，应用不同的样式；容器查询是媒体查询的特殊形式：

+ 使用`container-type`属性定义一个局限上下文
+ 使用`@container`关键字定义容器查询

## Css-宽度

Css中的`width`可以被指定为以下值：（斜体还属于实验阶段，要注意兼容性）

+ 绝对数值：单位可以为`px`，也可以为字长`em`
+ 相对百分比：详见 [Css-百分比长度](##Css-百分比长度)
+ `auto`：默认值，根据不同类型的元素，计算并选择一个宽度
  + 块元素会被设置为**可用宽度**，也就是父元素的100%
  + 绝对定位、浮动元素、`inline-block`元素或`table`项，表现为*`fit-content`*属性
  + 行内元素设置`width`属性无效
+ *`max-content`*：**最大宽度（包裹宽度）**：始终为元素内容实际尺寸，忽视父元素长度；内容过长时，没有换行符不会换行，即使超出父元素；行内元素被设置为`white-space:nowraps`时，也会出现类似情况
+ *`min-content`*：**最小宽度**，单词间空格都会换行，保证宽度最小，实际宽度大约就是句子中最长的单词文本长度；应用在中文句子时，会每1~2字换行
+ *`fit-content`*：
  + 当包裹宽度小于可用宽度，也就是子元素不大于父元素时，**使用包裹宽度**；反之则**使用可用宽度**并自动换行
  + 还有一种特殊情况，当可用宽度过小，小于最小宽度时，会**使用最小宽度**

---

一般来说`fit-content`不常用，但也有应用场景：

```css
/* 利用fit-content关键词，实现块元素自水平居中 */
div#fit {
    width: fit-content;
    margin: 0 auto;
}
```

这种方法不需要父元素设置`text-align:center`，同时居中元素内部也不会被`text-align`属性的继承性所影响

## Css-百分比长度

> 注意：如果设置百分比的属性是可继承属性，子元素只会继承**计算后的静态数值**，而不是动态百分比数值

> 说明：包含某元素的元素，称为该元素的**包含块**，一般为该元素的直接父元素

+ `width`和`height`
  + 相对于子元素的**包含块的相应属性**：包含某元素的元素，称为该元素的**包含块**，一般为该元素的直接父元素
  + 直接父元素未定义高度，子元素如果不为绝对定位，那么设置百分比，等同于设置为`auto`
  + 根元素`<html>`的包含块为初始包含块，是有定义高度的（视口高度），所以`<html>`可以直接使用百分比
  + `<body>`定义百分比前，需要先设置`<html>`的高度，如`html, body{height:100%;}`

+ `margin`和`padding`：在垂直方向和水平方向都是相对于**包含块的宽度**，而与包含块高度无关

+ `border-radius`：是相对于**自身宽度**，与父元素无关

+ `background-position`：基于**包含块与该元素的尺寸差**，可以为**负值**

+ `font-size`：基于直接父元素的**同属性**

+ `line-height`：基于自身`font-size`

+ `vertical-align`：基于自身`line-height`

## Css-雪碧图

**图像合并**（**Image sprites**），又称精灵图、雪碧图

通过将所有单张图片合并在一张图片中，减少Http请求

如何使用：

+ 使用类选择器设置背景图片，`background: url(myfile.png);`
+ 在`url()`中添加`x、y`坐标
+ 或直接使用`background-position`属性

## Css-盒模型属性

用以解决添加边框或内边距导致元素实际面积变大的问题的属性：

`box-sizing`属性

+ `content-box`：默认值，标准盒子模型
  + `width` = 内容的宽度
  + `height` = 内容的高度
  + 宽度和高度的计算值，不包含内容的边框（border）和内边距（padding）
+ `border-box`
  + `width` = `border` + `padding` + 内容的宽度
  + `height` = `border` + `padding` + 内容的高度
  + 宽度和高度的计算值，包括内容，内边距和边框，但不包括外边距

## Css-自定义滚动条

> 参考：https://segmentfault.com/a/1190000012800450
>
> https://juejin.cn/post/7036617475164733454

Chrome 浏览器：使用`::-webkit-scrollbar`伪元素

```css
body::-webkit-scrollbar {
    width: 10px;
    height: 10px;
}
/* 需要先设置::-webkit-scrollbar */
body::-webkit-scrollbar-thumb {
    background: hsl(0deg 0% 90%);
    border-radius: 5px;
}
```

FireFox 浏览器：

```css
html {
    scrollbar-width: thin;
    scrollbar-color: #e6e6e6 transparent;	/* 滚动条与背景颜色 */
}
```

## Css-文本换行

有关文本换行，存在两个Css属性：

+ `overflow-wrap`：以前叫作`word-wrap`，指示长单词是否能断开换行 
  + `normal`：只能在单词间的空隙处断行，即使单词超出范围
  + `break-word`：遇到一行都无法容纳的长单词，允许行尾换行
+ `word-break`：指示单词如何断开换行
  + `normal`：**默认。**对于非 CJK 文本，只允许在单词空隙间换行，单词过长时直接溢出；而对于CJK 文本，可以在任意字符间断行
  + `break-all`：总是试图利用剩余空间，可在任意字符间断行
  + `keep-all`：非 CJK 文本同`normal`效果，但不会对 CJK 文本进行断行

> CJK 文本：中日韩文字文本

两者区别在于，当单词过长导致该行空间不足时：

+ `overflow-wrap`会先试图把单词放在下一行，如果一整行都放不下才会采用换行
+ `word-break`只要遇到行尾，便直接”折断“单词

## Css-滚动条抖动

排版好的页面（一般主体为水平居中的页面），当（右侧）滚动条出现与消失时，会造成页面整体偏移一段，造成抖动感

第一种方法：使用`vw`计算属性

设置主体元素的**父元素**样式

```css
.main-father {
	margin-left: calc(100vw - 100%);
}
```

原理：

+ 100vw 代表**页面可视区域宽度**，100% 代表**页面可用区域宽度**，前者比后者多出滚动条的宽度；
+ 使用`calc()`函数，当无滚动条时取为0，有滚动条时取为滚动条宽度
+ 将左侧外间距设置为该值，从而平衡两边距离，继续让主体居中

注意事项：

1. `calc()`函数表达式中，运算符号两边要加上空格，以免混淆意思
2. 不要在`less`等预编译器（其他的没实验）里使用，其中的表达式会被直接计算为常量（0vw），不能动态适应

---

第二种方法：[scrollbar-gutter](https://developer.mozilla.org/en-US/docs/Web/CSS/scrollbar-gutter)属性

> 低版本兼容差

scrollbar-gutter 属性可以提前空出滚动条的位置，即使当前没有滚动条：

+ auto：默认，不留空，滚动条出现会挤压内容
+ stable：右侧提前留空
+ both-edges：右侧提前留空，左侧相应空出等同间距

---

第三种方法：[overflow: overlay](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow)属性

> 仅在基于WebKit（例如，Safari）和基于Blink的（例如，Chrome或Opera）浏览器中受支持

行为与`auto`属性相同，但绘制滚动条时不占用空间

---

第四种方法：

```css
html {
  overflow-y: scroll; /* 兼容IE8 */
}

:root {
  overflow-y: auto;
  overflow-x: hidden;
}

:root body {
  position: absolute;
}

body {
  width: 100vw;
  overflow: hidden;
}
```

## Css-竖直对齐

[vertical-align](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align) 用在**行内元素**或表格单元格（table-cell）上，指定其在父元素中的竖直对齐方式

## Css-文字装饰

> decoration /ˌdekəˈreɪʃn/： n. 装饰
>
> solid/ˈsɒlɪd/：adj. 结实的；一致的
>
> dotted/ˈdɒtɪd/：adj. 加附点的
>
> dashed/dæʃt/：n. 虚线
>
> wavy/ˈweɪvi/：adj. 波动起伏的

### [text-decoration](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)

该属性用于设置文本的修饰线，包括上下划线、删除线或闪烁

包含四种属性

+ [`text-decoration-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-line)：装饰线位置，可以同时指定多个属性（如果可以并存）
  + none：无效果
  + underline：下划线
  + overline：上划线
  + line-through：删除线/贯穿线
  + ~~blink：文本闪烁，不推荐使用~~

+ [`text-decoration-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-color)：装饰线颜色

+ [`text-decoration-style`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-style)：装饰线样式
  + solid：连续的直线
  + double：连续的两层直线
  + dotted：点状间断线
  + dashed：段状间断性；虚线
  + wavy：波浪线

+ [`text-decoration-thickness`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-thickness)：装饰线厚度，为新属性

*！该属性不具有继承性，只是单纯地应用样式在范围内，这意味着子元素不能重置其样式，但是可以在此基础上添加新的样式*

### [text-emphasis](https://developer.mozilla.org/en-US/docs/Web/CSS/text-emphasis)

> emphasis/ˈemfəsɪs/：重点；强调；

该属性为文字添加可自定义的着重号，包含两种属性：

+ [`text-emphasis-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-emphasis-style)：着重号样式
  + 可以指定为两个关键词，分别指定形状与是否实心
  + 直接指定为单个字符，当输入多个字符时只会取第一个字符
+ [`text-emphasis-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-emphasis-color)：着重号颜色

注意：

*该属性具有继承性，子元素可以通过`none`关键字重置其样式*，这点与[`text-decoration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)不同

*该属性可能会影响行高*

---

着重号默认在上方（横版文字）与右侧（竖版文字），可以使用下面的属性自定义位置

+ [`text-emphasis-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-emphasis-position)：可接受两个关键字，指示横竖书写下的着重号位置
+ **注意：**该属性不包含在[`text-emphasis`](https://developer.mozilla.org/en-US/docs/Web/CSS/text-emphasis)内，不能直接使用其关键字赋值

## Css-空白处理

使用[white-space属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)来规定对文本中空白符（换行符与空白符）的处理，为可继承属性

+ normal：合并空白符序列，自动换行
+ pre：原样输出，保留空白符序列，遇到换行符或`<br>`才换行
+ nowrap：合并空白符序列，遇到`<br>`标签才换行
+ pre-wrap：原样输出，保留空白符序列；遇到换行符或`<br>`换行，也会自动换行
+ pre-line：合并空白符序列；遇到换行符或`<br>`换行，也会自动换行
+ inherit：从父元素继承属性

|              | 换行符 | 空格与制表符 | 文字换行 | 行尾空格 |
| ------------ | ------ | ------------ | -------- | -------- |
| normal       | 合并   | 合并         | 换行     | 删除     |
| nowrap       | 合并   | 合并         | 不换行   | 删除     |
| pre          | 保留   | 保留         | 不换行   | 保留     |
| pre-wrap     | 保留   | 保留         | 换行     | 挂起     |
| pre-line     | 合并   | 合并         | 换行     | 删除     |
| break-spaces | 保留   | 保留         | 换行     | 换行     |

## Css-滤镜

> filter/ˈfɪltə(r)/：n. 滤镜；过滤器

Css属性[filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)，可以为元素添加模糊、颜色偏移与反相等图片处理效果

+ blur()：运用高斯模糊
+ opacity()：调节不透明度
+ invert()：调节反相程度
+ brightness()：调节明度
+ contrast()：调节对比度
+ grayscale()：调节灰度
+ hue-rotate()：色相旋转
+ saturate()：调节饱和度
+ sepia()：调节深褐色程度

## Css-鼠标光标

> cursor/ˈkɜːsə(r)/：n.光标 

Css属性[cursor](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)，用以改变鼠标光标的样式

以下是常用的几种样式：

+ auto：默认，交由浏览器判断
+ none：不渲染鼠标光标（消失）
+ pointer：手指样式
+ progress：程序后台繁忙，但用户当前可以交互
+ wait：程序繁忙，用户当前不可交互
+ text/vertical-text：文字、竖版文字选择样式
+ not-allowed：禁用
+ move：指示该组件可以移动
+ zoom-in/zoom-out：放大镜加减样式

## Css-文字背景图

使文字作为缝隙透出后面的背景图，实现多彩的文字

### 叠加方式

> mix/mɪks/：v.（使）混合
>
> blend/blend/：v.（使）混合

> [mix-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mix-blend-mode)：描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合，类似ps中的图层混合模式

这种方法需要两层：下层为图片，上层为完全覆盖图片的文字层

文字层设置为**黑字白底**，再设置`mix-blend-mode:lighten`，此时白色部分不变，黑色部分以底层颜色为准，达成效果

```css
.pic {
    position: relative;
    width: 100%;
    height: 100%;
    background: url('xxx.jpg');
    background-repeat: no-repeat;
    background-size: cover;
}
 
.text {
    position: absolute;
    width:100%;
    height:100%;
    color: #000;
    mix-blend-mode: lighten;
    background-color: #fff;
}
```

### 裁切方式

> [background-clip:text](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)：设置背景图片的裁切方式，`text`为沿着文字裁切，保留文字内的背景

这种方法只需要图片层，文字可以直接写在上面

将文字颜色设置为透明，再设置`background-clip:text`属性，此时除文字外的部分都被裁切掉，达成效果

*可能兼容性较差*

```css
.pic2{
    width: 500px;
    height: 500px;
    background: url("xxx.jpg") no-repeat center center;
    background-size: cover;
    font-size: 120px;
    line-height: 500px;
    /* 以下为重点 */
	background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
}
```

## Css-文字描边

[-webkit-text-stroke](https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-text-stroke)：该属性可以用以定义文字的描边，可以传入**宽值**与**颜色**

## Css-不规则元素环绕

思路：使用 [clip-path](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path) 创建不规则图形，使用 [shape-outside](https://developer.mozilla.org/zh-CN/docs/Web/CSS/shape-outside) 属性实现围绕其的文本

+ clip-path 属性：
  + [`<basic-shape>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/basic-shape)图形类型
    + inset()：矩形
    + circle()：圆形
    + ellipse()：椭圆
    + polygon()：多边形
    + path()：使用SVG定义形状

> inset*/*ˈɪnset*/*	*n.*嵌入物
>
> circle*/*ˈsɜːk(ə)l*/*	*n.*圆
>
> ellipse*/*ɪˈlɪps*/*	*n.*椭圆
>
> polygon*/*ˈpɒlɪɡən*/*	*n.*多边形

+ shape-outside 属性：
  + `<shape-box>`：盒模型的边缘，如：margin-box、border-box、padding-box、content-box
  + `<basic-shape>`图形类型，同上

## Css-方向简写

+ `-inline`：`-left`与`-right`的简写
+ `-block`：`-top`与`-bottom`的简写
+ `inset`：`top`、`bottom`、`left`与`right`的简写

举例：

```css
.div {
    margin-inline: 12px;
    /* 相当于：
     margin-left: 12px;
     margin-right: 12px;
     */
}
.div2 {
    position: absolute;
    inset: 0;
    /* 相当于：
     top: 0; 
     bottom: 0;
     left: 0; 
     right: 0;
     */
}
/* 另 */
.div3 {
    position: absolute;
    inset-block: 0;
    /* 相当于：
     top: 0; 
     bottom: 0;
     */
}
```

# JavaScript

## JavaScript-数组乱序

Fisher–Yates 算法：倒序遍历数组，将每一位与剩下的数组随机一位进行置换

```js
function shuffle(a) {
    for (let i = a.length-1; i<0 ; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
    }
    return a;
}
```

## JavaScript-生成顺序数字数组

```js
[...new Array(num).keys()]	//[0,num-1]的顺序数组
[...new Array(num).keys()].map((i)=>{return i+1})
Array.from({length: num}, (item, index) => index+1)	//[1,num]的顺序数组
```

## JavaScript-数组去重

> 参考：https://github.com/mqyqingfeng/Blog/issues/27

```js
Array.from(new Set(array));
[...new Set(array)]
```

```js
// 旧版本
function unique2(arr){
    for(let i=0; i<arr.length; i++){
        for(let j=i+1; j<arr.length; j++){
            if(arr[i] === arr[j]){
                arr.splice(j, 1);
                j--;
            }
        }
    }
    return arr;
}
```

## JavaScript-拷贝

> 参考：https://github.com/mqyqingfeng/Blog/issues/32

### 浅拷贝

拷贝后的对象，其中的引用对象（对象或者数组）实际指向同一个目标

```js
let arr = [ [1] ]
let arr_copy = arr
arr_copy[0].push(2)
console.log(arr, arr_copy)	// [ [ 1, 2 ] ] [ [ 1, 2 ] ]
```

浅拷贝的方法：

```js
var new_arr = arr.concat();	// concat()用于连接多个数组，返回值为新数组
var new_arr = arr.slice();	// slice()用于返回数组中的片段
```

### 深拷贝

拷贝后的对象，其中的引用对象（对象或者数组）指向分别指向两个目标，只不过内容相同

如下例所示，arr并没有被添加第二个元素

```js
let arr = [ [1] ]
let arr_copy = JSON.parse( JSON.stringify(arr) )
arr_copy[0].push(2)
console.log(arr, arr_copy)	// [ [ 1 ] ] [ [ 1, 2 ] ]
```

深拷贝的方法：

```js
var new_arr = JSON.parse( JSON.stringify(arr) );	// 不能拷贝函数对象
```

**注意**，该种方法存在弊端：

> 参见：[关于 JSON.parse(JSON.stringify(obj)) 实现深拷贝的一些坑](https://segmentfault.com/a/1190000020297508)

1. `Date`对象（非时间戳） => 字符串
2. `RegExp`与`Error`对象  => 空对象
3. `Function`与`undefined` => 属性丢失
4. `NaN`与`±Infinity` => `null`
5. 构造函数生成的函数  =>  丢失`constructor`
6. 对象存在循环引用（比如引用自身），会导致深拷贝失败

```js
let obj = { age: 18 };
obj.obj = obj; // 循环引用
let objCopy = JSON.parse(JSON.stringify(obj));
console.log('obj', obj);
console.log('objCopy', objCopy); // Uncaught TypeError: cyclic object value
```

## JavaScript-节流和防抖

+ 节流：规定时间多次触发，计时器不被更新，触发第一次操作
+ 防抖：规定时间多次触发，计时器被更新，重新开始计时，触发最后一次操作

```js
//防抖函数-定时器
const debounce = (fn, delay=1000)=>{
    let timer = null;
    return (...args)=>{
        clearTimeout(timer);
        timer = setTimeout(()=>{
            fn.apply(this, args);
        }, delay)
    }
};
//节流函数-定时器
const throttle = (fn, delay=1000) =>{
    let flag = false;
    return (...args)=>{
        if(flag) return;
        flag = true;
        setTimeout(()=>{
            fn.apply(this, args);
            flag = false;
        }, delay)
    }
};
//节流函数-时间戳
const throttle2 = (fn, delay=1000)=>{
    let previous = 0;
    return (...args)=>{
        let now = Date.now();
        if(now - previous <= delay) return;
        fn.apply(this, args);
        previous = now;
    }
}

function fnn(){
    console.log("")
}
// 添加事件监听
document.addEventListener("scroll", throttle(fnn,1000));
document.addEventListener("scroll", debounce(fnn,1000));
```

## JavaScript-`...`

> rest/rest/：*n.* 剩余部分
>
> spread/spred/：*v*. 展开

`...`为ES6中的**新特性**，在不同场景中有着不同的含义：

### rest 参数

在函数中作为形参，为rest参数，当传入的参数大于定义的形参数量，多余参数都放入数组作为rest参数

注意：rest参数只能作为最后一个形参加入

```js
function push(array, ...items) { return } 	// 将多余的参数保存在items数组中
```
### spread 运算符

在平时的计算中，表示就地展开该对象或数组，将一个参数变为同级的多个参数

```js
[1,...[2,3],4] 						//	将里面的数组拆开，得到[1,2,3,4]
clonedObj = { ...obj1 }				//	使用该特性快速复制对象
mergedObj = { ...obj1, ...obj2 }	//	也可以快速生成合并对象
```

## JavaScript-计算属性值

> 参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Object_initializer#%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7%E5%90%8D

ES6中的**新特性**，在对象初始化语法中，支持在`[]`中**加入表达式**，得到计算的属性值

```javascript
var param = 'size';
var config = {
  [param]: 12,
  ["mobile" + param.charAt(0).toUpperCase() + param.slice(1)]: 4,
  [`mobile2${param.charAt(0).toUpperCase() + param.slice(1)}`]: 8	//	使用模板字符串也可以
};
// {size: 12, mobileSize: 4, mobile2Size: 8}
```

其实就是方括号取值的功能，也可以在对象初始化赋值时使用了

```js
config[param]	//	12
```

## JavaScript-数字数组和字符数组互换

```js
arr = [1,2,3]
arr.map(String); 	//['1','2','3']
arr.map(Number); 	//[1,2,3]
```

## JavaScript-JSONP跨域

+ 背景：Ajax无法跨域，但**Web页面上调用js文件时可以跨域**

+ 解决：
  + 请求端：
    + 定义返回数据后的回调函数
    + 动态生成请求URL（包括回调函数名、查询参数）
    + 新建`script`标签元素并赋值`src`为上一步生成的URL，添加该标签至`<head>`标签，此时开始调用
  + 服务端：
    + 根据传来的`callback`参数作为函数名，用函数名包裹应该返回的数据
    + 返回，请求端执行该函数，获得数据

## JavaScript-`var、let、const`之间的区别

+ `var`声明变量可以重复声明，而`let`不可以重复声明 （在同一块作用域中）
+ `var`是不受限于块级的，而`let`是受限于块级 
+ `var`会与`window`相映射（会挂一个属性），而`let`不与`window`相映射 
+ `var`可以在声明的上面访问变量，而`let`有暂存死区，在声明的上面访问变量会报错 
+ `const`声明之后必须赋值，否则会报错
+ `const`定义不可变的量，改变了就会报错 
+ `const`和`let`一样不会与`window`相映射、支持块级作用域、在声明的上面访问变量会报错

## JavaScript-数组操作

### 添加

+ [arr.push(e1, e2...)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
  + 添加（单个或多个）元素至末尾，返回修改后*数组的长度*
  + *改变原数组*
+ [arr.unshift(e1, e2...)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)
  + 添加（单个或多个）元素至第一个元素前，返回修改后*数组的长度*
  + 多个参数会被作为一个整体直接加入，而非依次加入
  + *改变原数组*

### 删除

+ [arr.shift()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
  + 删除第一个元素，返回*被删除的元素*
  + *改变原数组*

+ [arr.pop()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
  + 删除最后一个元素，返回*被删除的元素*
  + *改变原数组*

### 定位&包含

+ [arr.indexOf(ele, start)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
  + 定位并返回元素在数组**首次**出现中的*索引或`-1`*
  + `start`参数指定从该索引（含）开始查找
+ [arr.includes(item, start)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
  + 返回是否包含元素的*布尔值*
  + `start`参数指定从该索引（含）开始查找

### 筛选

+ [arr.find(callback)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
  + 返回满足回调函数的**首个**元素本身
  + `callback`接受三个参数，自动传入：
    + `element`：当前元素
    + `index`：当前索引
    + `array`：数组本身
+ [arr.findIndex(callback)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)
  + 返回满足回调函数的**首个**元素索引

### 反转

+ [arr.reverse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)：反转数组并返回，改变原数组

### 遍历

见[此处](.\JavaScript-四种循环.md)

## JavaScript-`slice()`、`splice()`和`split()`

> slice/slaɪs/：vt. 切下；把…分成部分；将…切成薄片
>
> splice/splaɪs/ ：v. 拼接，接合
>
> split/splɪt/：vt. 分离；使分离；劈开；离开；分解

#### `slice()`

+ 可操纵 String 和 Array
+ 复制**[begin, end)**范围的内容作为返回值
  + 空缺end则默认到尾部
  + 空缺两项则默认为从头到尾
+ 用法
  + `array.slice(begin, end)`
  + `array.slice(begin)`
  + `array.slice()`
+ **不改变**原数组，返回**被复制的部分**

#### `splice()`

+ 操纵Array
+ 从数组中删除项目，返回值为被删除项
  + `start`删除起始值
  + `deleteCount`删除偏移值，为 0 则不删除
  + `item…`在删除起始处插入新项目
+ 用法
  + `array.splice(start)`
  + `array.splice(start, deleteCount)`
  + `array.splice(start, deleteCount, item1, item2, ...)`
+ **改变**原数组，返回**被删除的部分**

#### `split()`

+ 操纵String
+ 以`separator`（可以是字符串或正则）为界分割字符串成为数组，并根据`howmany`返回该数组的前几位，之后部分丢弃，不指定则返回全部
+ 用法
  + `stringObject.split(separator)`
  + `stringObject.split(separator, howmany)`
+ **不改变**原字符串，返回一个**分割生成的数组**

> `array.join([separator])`：该函数反过来将数组中的元素，以分隔符（可以为空）为界连接成一个新的字符串并返回，原数组无变化
>
> 如果数组为空，或为`undefined`或`null`，会生成空字符串，不会报错

## JavaScript-对象数组操作

### 删除指定元素

① 已知该对象元素的下标

直接改变原数组 arr，index 为下标

```js
arr.splice(index, 1)
```

② 已知该对象元素中的唯一值

返回一个新数组，removeId 为该对象元素包含的值

```js
let newArr = arr.filter(item => return item.id !== removeId)
```

## JavaScript-弹出框

三种弹出框分别为**警告框、确认框、提示框**

```js
// 弹出警告信息
window.alert("sometext");
// 用户选择“确定”返回true, 选择“取消”返回false
window.confirm("sometext");
// 返回用户输入的值，可以指定默认值
window.prompt("sometext","defaultText"); 
```

注：都可以去掉`window`前缀使用

## JavaScript-鼠标事件

> 参考：https://zh.javascript.info/mouse-events-basics

### 监听事件

+ `mousedown/mouseup`：在元素上点击/释放鼠标按钮。
+ `mouseover/mouseout`：鼠标指针从一个元素上移入/移出。
+ `mousemove`：鼠标在元素上的每个移动都会触发此事件。
+ `click`：如果使用的是鼠标左键，则在同一个元素上的 `mousedown` 及 `mouseup` 相继触发后，触发该事件。
+ `dblclick`：短时间内双击元素。

### 鼠标键值

使用`event.button`属性获取被点击的键，键值为：

+ 左键/主键：0
+ 中键：1
+ 右键/辅助键：2
+ 后退键：3
+ 前进键：4

*在默认配置下，左键为主键，存在右键为主键的配置；*

⚠ 过时的用法：`event.which`：

+ 左键：1
+ 中键：2
+ 右键：3

### 组合键

`event`还有如下属性，检测组合键是否被按下：

- `shiftKey`：<span style="border:1px solid #e8e6e5;"> Shift </span>
- `altKey`：<span style="border:1px solid #e8e6e5;"> Alt </span> 或Mac: <span style="border:1px solid #e8e6e5;"> Opt </span>
- `ctrlKey`：<span style="border:1px solid #e8e6e5;"> Ctrl </span>
- `metaKey`：<span style="border:1px solid #e8e6e5;"> Win </span> 或Mac: <span style="border:1px solid #e8e6e5;"> Cmd </span>

### 坐标

有两种属性：

1. 相对于窗口的坐标：`clientX` 和 `clientY`。
2. 相对于文档的坐标：`pageX` 和 `pageY`。

## JavaScript-键盘事件

### 监听事件

触发顺序自上而下，keydown与keypress事件过程，可以阻止默认事件：

+ [keydown](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/keydown_event)：键盘被按下时
+ [keypress](https://developer.mozilla.org/en-US/docs/Web/API/Document/keypress_event)：字符串被输入时
+ [keyup](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/keyup_event)：键盘被松开时

keydown 与 keypress 的不同：按下按键后，如果没有产生字符，则 keypress 事件不被调用

### 属性

> 参考：https://www.zhangxinxu.com/wordpress/2021/01/js-keycode-deprecated/

+ [event.keyCode](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/keyCode)：返回按键的`keyCode`值，<span style="color:red">被弃用</span>
+ [event.key](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/key)：返回具体输入的字符，非打印字符亦会返回其名称；可以区分大小字母、修饰键修饰下的次字符
+ [event.code](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/code)：返回键盘的物理键值，可以区分左右<span style="border:1px solid #e8e6e5;"> Shift </span>；不因按键位置的更改或修饰键的存在而改变

> 修饰键：Alt, Shift, Ctrl 等；次字符，例如`Shift+1`键产生的`!`字符

## JavaScript-userAgent 详解

> agent/ˈeɪdʒənt/ n.代理人

> 参考：https://www.yukapril.com/2018/10/13/useragent.html

```js
// 取值
var sUsrAg = (window.)navigator.userAgent
```

通用格式

```
Mozilla/[version] ([system and browser information]) [platform] ([platform details]) [extensions]
```

- **Mozilla 前缀**：这个已经是兼容后的产物了，没什么意义了。即 `Mozilla/5.0`。
- **系统和浏览器信息**：即 `(Windows NT 10.0; Win64; x64)`。这个字段需要用括号括起来。
- **浏览器渲染引擎**：即 `AppleWebKit/537.36`。
- **浏览器渲染其他补充信息**：我认为各个浏览器为了兼容，这个字段已经没有了实际意义，即 `(KHTML, like Gecko)`。这个字段同样需要用括号括起来。
- **扩展字段**：这个字段内容最为丰富，主要描述了浏览器信息，以及各个浏览器自己添加的自定义字段等。即 `Chrome/70.0.3538.5 Safari/537.36`。

## JavaScript-精确小数到几位

### `num.toFixed(n)`

+ 优点：快速精确到n位，并**五舍六入**
+ 缺点：小数位不足时会补充0，如`2=>2.00`

```js
0.123.toFixed(2)	// 0.12
0.125.toFixed(2)	// 0.13
0.1.toFixed(2)		// 0.10
```

### `Math.round(num* 10**n)/10**n`

`round()`函数返回一个更加接近的整数，即不超过0.5的向下取整，超过0.5的向上取整；`10**n`为语法糖，表示10的次方

将小数放大一定位数后，执行`round`取整，再缩小到原先位数

```js
// 保留两位小数，不会四舍五入
Math.round(number * 100) / 100
```

### 使用字符串

```js
//	字符串转化，不四舍五入
Number( num_str.slice(0,num_str.indexOf('.')+3) );
```

## JavaScript-立即执行函数

[IIFE（立即调用函数表达式）](https://developer.mozilla.org/zh-CN/docs/Glossary/立即执行函数表达式)

```js
(function(){
    // pass;
})();
```

使用括号包裹匿名函数，再立即调用；用来创建私有区域，避免全局变量污染产生

注意：连续使用多个立即执行函数，需要后续`;`，否则后面的函数会被错误作为参数传入上个函数，[参见](https://stackoverflow.com/questions/42036349/uncaught-typeerror-intermediate-value-is-not-a-function)

## JavaScript-addLoadEvent函数

`addLoadEvent()`用于大量函数需要`onload`的时候，确保这些函数执行的顺序

> `onload`标识页面、组件或图像加载完后执行的函数；`window.onload`用以标识页面加载完后执行的函数

```js
function addLoadEvent(func) {
  var oldonload = window.onload;//将现有的事件处理函数的值存入变量中
  if (typeof window.onload != 'function') {
    window.onload = func;//如果这个事件处理函数没有绑定任何函数，就把新函数添加给它
  } else {
    window.onload = function() {
      oldonload();
      func();//如果已经绑定了函数，就把新函数追加到现有指令的末尾
   }
  }
}
```

使用：将需要函数名作为参数调用即可

```js
addLoadEvent(func1);
addLoadEvent(func2);
addLoadEvent(func3);
```

## JavaScript-事件冒泡和捕获

> 参考：https://segmentfault.com/a/1190000005654451

事件流（事件发生顺序）的两种解决方案：事件**冒泡**和**捕获**

+ 事件冒泡：由微软提出，从当前触发元素向上传递事件，直到document对象
+ 事件捕获：由网景提出，与事件冒泡顺序相反

当前解决方案：**先进入事件捕获，再进入事件冒泡**

### 添加事件监视

[EventTarget.addEventListener()](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)函数，将指定的监听器注册到目标对象上，当该对象触发监听器定义的事件后，调用定义的回调函数

```
addEventListener(type, listener);
addEventListener(type, listener, options);
addEventListener(type, listener, useCapture);
```

该函数的参数：

+ `type`：触发[事件类型](https://developer.mozilla.org/zh-CN/docs/Web/Events)，大小写敏感
+ `listener`：触发事件后执行的回调函数
+ `options`-可选：对象类型，指定`listener`的属性
  + `capture`：布尔值，作用同`useCapture`属性
  + `once`：布尔值，`listener`被添加后，至多只执行一次
  + `passive`：布尔值，设置为 `true` 时，表示 `listener` 永远不会调用 `preventDefault()`
  + `signal`
+ `useCapture`-可选：布尔值，是否在事件捕获阶段调用函数，默认为`false`，故一般在事件冒泡时调用函数

### 事件冒泡和捕获共存时

1. 首先从document向target节点进行**事件捕获**，遇到捕获事件则触发
2. 到达target节点，遵循**先注册绑定的先执行**（无论捕获事件或冒泡事件）
3. 再从target节点到document进行**事件冒泡**，遇到冒泡事件则触发

### 事件代理

借助于事件冒泡或捕获的性质，当很多子元素都需要绑定同一个事件时，可以直接绑定在公共祖先元素上统一处理

+ 在父元素上绑定子元素们需要执行的事件函数
+ 在事件函数中，使用`e.target`获得当前触发事件的元素，使用`nodeName`或其他属性确保特定子元素才触发事件

+ 当绑定事件的元素和触发事件的元素不为同一元素时：
  + `e.target`为触发事件的元素
  + `e.currentTarget`为绑定事件的元素，此时`this`也为该元素

### 阻止事件冒泡和捕获

#### `stopPropagation()`

> propagation	/ˌprɒpəˈɡeɪʃn/	n. 传播；繁殖
>
> prevent	/prɪˈvent/	vt. 预防；阻止
>
> default	/dɪˈfɔːlt; ˈdiːfɔːlt/	n. 系统默认值
>
> immediately	/ɪˈmiːdiətli/	adv. 立即

使用 [event.stopPropagation](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation) 来阻止事件冒泡和捕获，也就是将过程中断于此

+ 阻止当前事件的进一步传播，无论是冒泡还是捕获阶段 
+ 不阻止事件的默认行为，如链接被点击后发生的跳转等行为

相似的名称 [event.stopImmediatePropagation](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopImmediatePropagation) 函数：

+ 使用于单个元素被绑定多个事件时，按照绑定顺序被依次调用
+ 对其中某事件监听器（函数）使用，在其之后的监视器都不会被调用

与之相对的，阻止默认行为的 [event.preventDefault](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 函数

+ 阻止事件的默认行为，如链接的点击跳转、输入框的输入等行为
+ 事件会继续传播

#### `return false`

**在jQuery中**，回调函数返回 false ，可以同时阻止传播行为 和 阻止默认行为

### 手动派发事件

[EventTarget.dispatchEvent(Event)](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/dispatchEven t) 方法会向指定的事件目标，派发一个事件。从而可以调用该目标上的监听回调事件，此过程为同步：在该方法返回前，监听该事件的回调函数会被执行完毕并返回。

调用该方法是触发事件的最后一步。传参为通过 [`Event()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/Event) 构造函数创建的事件对象。

返回值： `event` 中至少有一个回调函数调用了 [`Event.preventDefault()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 方法时，返回`false`，否则为`true`。

### 与on事件的对比

`addEventListener()`是W3C DOM规范中，提供的注册事件监听器

+ 允许为一个事件注册多个监听器
+ 可以选择事件触发阶段：捕获或者冒泡
+ 对任何DOM元素都是有效的，不限于HTML DOM元素
+ 通过[`removeEventListener`](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FEventTarget%2FremoveEventListener)来移除注册的事件；如果挂载匿名函数，则无法移除

`on`事件（如`onclick`）为注册事件监听器的旧方法

+ 可以用于js与html标签上
+ 替换该元素已存在的`on`事件，即为同一事件最多只有一个事件
+ 将该值直接设置为`null`以移除
+ DOM 0规范的内容，兼容性好

**说明：**两种方法都默认传入`event`事件，函数内部的`this`指向元素本身（若为箭头函数则指向父级作用域）

```html
<body>
  <button id="btn1">1</button>
  <button id="btn2">2</button>
  <button onclick="...">3</button>
</body>
<script>
const b1 = document.getElementById('btn1')
const b2 = document.getElementById('btn2')
b1.addEventListener("click", function() {...})
b2.onclick = function() {...}
</script>
```

注：在html的属性中定义`on`事件，可以使用`event`事件，且`document`与`this`里的成员可以被直接使用，原理是通过`with`实现的作用域链扩展

如下例，`document.getElementById`与`this.textContent`都可以被直接读取，`event`的属性则需要前缀

```html
<button onclick="getElementById('xx');textContent;event.type;">btn</button>
```

## JavaScript-各种高度

| 属性               | 中文名       | 说明                                               |
| ------------------ | ------------ | -------------------------------------------------- |
| screen.height      | 屏幕高度     | 整个屏幕的高度                                     |
| screen.availHeight | 屏幕可用高度 | 不包含任务栏高度的屏幕高度                         |
| window.outerHeight | 浏览器高度   | 浏览器窗口高度，最大化时占满屏幕时，浏览器可用高度 |
| window.innerHeight | 页面可用高度 | 不包含浏览器工具栏的浏览器可见高度                 |
| body.clientHeight  | body展示高度 | 不包含滚动条高度的页面可用高度                     |
| body.offsetHeight  | body总高度   | body的实际高度，包括body展示高度和未显示的高度     |

![](https://i.loli.net/2021/04/08/Fra4M36YXhIu7jH.png)

## JavaScript-变量提升

### 定义

在JavaScript代码中，变量或函数可以在声明前使用，而不会出现引用错误，这是因为**变量提升**的存在

+ 字面意思：代码在运行时，变量或函数的声明会被提升至代码头部，以避免出现引用错误

+ 实质：代码在编译阶段，运行之前，会检测所有的变量和函数声明，然后添加至运行内存中，便于运行时调用

### 提升范围

不同的变量和函数提升的范围不一样：

+ `function`类型变量，**会被创建、初始化和赋值**；可以调用运行
+ `var`类型变量，**会被创建和初始化为`undefined`，但不会被赋值**；可以调用，但直到赋值语句之前都为`undefined`
+ `let`类型变量与`const`类型变量，**会被创建，不会被初始化和赋值**；在赋值语句前调用时，会出现引用错误
  + 如果`let`变量的声明处还未赋值，则赋值为`undefined`，直到遇到赋值语句
  + 如果`const`变量的声明处还未赋值，抛出一个语法错误`SyntaxError`

```js
console.log(a);
var a = "a";
//	outcome:undefined，a还未被赋值

console.log(b);
let b = "b";
//	ReferenceError: can't access lexical declaration 'b' before initialization
//	引用错误，未初始化之前不能使用变量"b"

fn1()
function fn1() {
    console.log("fn1")
}
//	outcome:fn1，正常运行

fn2()
var fn2 = function() {
  console.log("fn2")
}
//	TypeError: fn2 is not a function，类型错误，目前fn2还未被赋值
```

## JavaScript-布尔值转化

使用`Boolean(value)`函数，将任意值转化为布尔类型

+ 以下值将会被转化为`false`：
  + `undefined`
  + `null`
  + `±0`
  + `NaN`
  + `''`（长度为 0 的字符串，具有长度的多个空字符串为`true`）
+ 除此之外的值都将转化为`true`
  + 所有对象的布尔值都将转化为`true`，**即使为空对象**
  + `new Boolean(false)`将布尔值包装为对象，转化为布尔值亦为`true`；`Boolean(false)`是转化布尔值的操作，取得`false`

```js
//	为否的情况
Boolean(false)		//	false
Boolean(undefined)	//	false
Boolean(null)		//	false
Boolean(0)			//	false
Boolean(-0)			//	false
Boolean(NaN)		//	false
Boolean('')			//	false

//	为是的情况
Boolean([])					//	true
Boolean({})					//	true
Boolean('  ')				//	true
Boolean(new Boolean(false))	//	true
```

## JavaScript-逻辑与&逻辑或操作符

用于赋值操作中的取值

+ 逻辑与（`&&`）：返回第一个可以被转化为`false`的值，或最后一个值
+ 逻辑或（`||`）：返回第一个可以被转化为`true`的值，或最后一个值
+ [`空值合并`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)（`??`）：返回第一个不为`null`或`undefined`的值，或最后一个值

关于布尔值转化，可见[布尔值转化](##JavaScript-布尔值转化)

## JavaScript-可选链操作符

> 此处提到的“引用为空”：[nullish](https://wiki.developer.mozilla.org/en-US/docs/Glossary/nullish)：为`null`或`undefined`

### 用于属性

希望读取对象的对象链上某个属性，但链中的某些*引用可能为空*，使用`?.`代替`.`链接该属性

当引用为空时，返回`undefined`而不是报错

```js
const myNull = null
myNull.length	//	Uncaught TypeError: null has no properties
myNull?.length	//	undefined
```

### 用于函数

也可以用于调用可能不存在的方法，注意操作符的位置

```js
const obj = { fn1(){ console.log('fn1') } }
obj.fn1()	//	fn1
obj.fn2()	//	Uncaught TypeError: obj.fn2 is not a function
obj.fn2?.()	//	undefined
```

### 用于数组&方括号属性

```js
const arr = []
const obj2 = {}
arr?.[78]				//	undefined
obj2?.['prop' + 'Name']	//	undefined
```

### 不能用于赋值

```js
let obj3 = {};
object?.property = 1; // Uncaught SyntaxError: Invalid left-hand side in assignment
```

---

### 空值合并操作符

[`空值合并操作符`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)，即`??`，是一个逻辑操作符，作用是返回第一个不为空（不为`null`或`undefined`）的值

与逻辑或`||`相比，向左取的范围更大

```js
let n1 = '' || '右侧'	//	'右侧'
let n2 = '' ?? '右侧'	//	''
```

## JavaScript-合并

### 数组

#### [concat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

该方法用于合并两个及以上的数组，并返回**新数组**

```js
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

参数可以为数组或单值，数组中元素会被遍历加入，而不会作为整体加入

```js
[1].concat(2,[3,4])	//	output:[1,2,3,4]
```

当没有赋参数时，获得*原数组的一个浅拷贝*

#### [apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

该方法利用了`apply()`函数接收参数形式为数组的特点，遍历后者元素使用`push()`函数，此方法**修改原数组**

```
array.push.apply(array, new_array)
```

#### 扩展字符串

最简单的方法

```
var a = [1,2,3];
var b = [4,5,6];
var newA = [...a,...b]
```

### 对象

#### [assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

该方法用于将数个源对象的键值对加入到目标对象中

```js
Object.assign(target, ...sources)
```

注意点：

+ 原目标对象会被修改，也会返回目标对象
+ 存在同名的键值时，后来的会覆盖已有的键值对

## JavaScript-生命周期

+ DOMContentLoade：解析DOM树构建完毕后
+ loaded：所有的外部资源都被加载成功后调用
+ beforeunload：离开当前页面前触发，给予用户额外的确认

### 使用

使用`addeventListener()`函数监听document触发DOMContentLoaded

```js
document.addEventListener("DOMContentLoaded", readyFunction);
```

使用全局对象window指定loaded时，需要执行的函数：

```js
window.onload = function() { ... }
```

指定用户离开前的提示字符串（有的浏览器不会显示，而是为指定内容）

```js
window.onbeforeunload = function() {
  return "There are unsaved changes. Leave now?";
};
```

### 状态

使用`document.readyState`查看当前加载状态：

+ `loading`：**加载中**，DOM还在加载中
+ `interactive`：**可交互**，DOMContentLoade状态
+ `complete`：**完成**，loaded状态

```js
console.log(document.readyState)
```

通过监听`readystatechange`事件的触发，来得知每次的状态更改

```js
document.addEventListener('readystatechange', () => console.log(document.readyState));
```

## JavaScript-生成器函数

> generator/ˈdʒenəreɪtə(r)/：n. 生成器
>
> iterator/ɪtə'reɪtə/：n. 迭代器
>
> yield/jiːld/：v. 产出

### 生成

使用`function*`的方式来创建一个生成器函数([Generator function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*))；生成器函数在执行时可以暂停，之后可以继续运行

```js
function* fn() {
  console.log("one");
  yield '1';
  console.log("two");
  return '2';
}
```

### 迭代

生成器函数被调用后，返回一个 [`Generator`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator) 对象

+ `next()`函数：获取生成器函数中下一个`yield`语句所返回的值，没有则返回`undefined`，此时迭代结束
+ `return()`函数：直接结束当前迭代，并返回最后的`undefined`
+ 实质上述函数返回一个对象，包括返回值`value`属性与迭代器结束状态`done`属性

```js
let f = fn() // undefined
f.next() // 打印出'one';返回 Object { value: "1", done: false }
f.next() // 打印出'two';返回 Object { value: "2", done: true };此时迭代结束
f.next() // Object { value: undefined, done: true };返回undefined
```

### 传参

带参数的`next()`，可以为迭代器传参；实质参数是替换上一次的`yield`语句，所以对第一个`next`的传参是无效的

```js
function* sum(a) {
  console.log('a:', a);
  let b = yield 1;
  console.log('b:', b);
  let c = yield 2;
  console.log('c:', c);
  let sum = a + b + c;
  console.log('sum:', sum)
  return sum;
}

let s1 = sum(10) // 赋值a为10
s1.next(20) // 没有作用；打印a:10;返回1
s2.next(30) // 赋值b，实质为：let b = yield 1; => let b = 30;打印b:30;返回2
s3.next(40) // 赋值c，打印c:40,sum:80;返回80(10+30+40)
```

## JavaScript-`select`函数

> 参见：[select函数](https://www.jquery123.com/select/)

`select`函数只能用在`<input type="text">` 和`<textarea>`，有两种用法

+ 带参数：绑定一个文本选中时的触发函数，可以携带一个对象传递给该函数，同`.bind('select', handler)`
+ 不带参数：快速将文本内容选中，同 `.trigger('select')`

## JavaScript-`once`函数实现

> 参考：https://juejin.cn/post/6971225536882278413

`once()`函数用以确保函数只被执行一次；但并未被正式支持，需要自己写代码来实现

实现一：

```js
function once(fn) {
    let called = false;
    return function () {
        if (!called) {
            called = true;
            fn.apply(this, arguments);
        }
    };
}

function launchRocket() {
    console.log("执行一次");
}

const launchRocketOnce = once(launchRocket);
launchRocketOnce();	// 打印“执行一次”
launchRocketOnce(); // 无
```

## JavaScript-`compose`函数实现

> 参考：https://juejin.cn/post/6971260867300032525

当需要对某数据进行复杂处理时，可以先拆分为多个步骤的分函数，再实现`compose`函数，整合分步骤。这样可以增加代码阅读性

以下是几种实现方法：

```js
// 使用while循环
function composeCyclic(...fns) {
    return function(value) {
        let list = fns.slice()
        while (list.length) {
            value = list.shift()(value)
        }
        return value
    }
}
```

```js
// 使用嵌套
function composeNest(...fns) {
    const [fn1, fn2, ...rest] = fns
    const composedFn = function(...args) {
        return fn2(fn1(...args))
    }

    if(!rest.length) return composedFn
    return composeNest(composedFn, ...rest)
}
```

调用，因为实际返回的是一个函数，所以还需要调用返回的函数

```js
let fn = compose(fn1, fn2, fn3)
let outcome = fn(value)
```

## JavaScript-修改&移除样式

> 参见：https://cloud.tencent.com/developer/article/1537911

可以使用以下方式来修改元素样式：

### 修改内联样式

```js
const element = document.getElementById('title')

element.style.color = 'initial'								// 直接修改对应属性为默认，带‘-’的属性需要改为驼峰形式
element.style.setProperty('color', 'initial', 'important')	// 通过函数修改对应属性为默认,第三参数可选
element.setAttribute('style', 'color: initial !important')	// 这是直接设置style属性本身
```

这种方法实际上是为元素添加`style='color: initial'`属性，用内联样式**覆盖**掉原有的样式

所以并不能直接移除样式表样式，如下方法是不可行的：

```js
element.style.removeProperty('color')	// 这种只能移除内联样式，无法清除样式表样式
```

---

操作样式表样式的方法：

```js
document.styleSheets[0].rules[0].style.setProperty('color', 'initial')
```

获取第一处样式表的第一处规则，设置或覆盖其中的样式

或者直接添加或插入一整块：

```js
document.styleSheets[0].addRule('.box', 'height: 100px');		// 添加新样式，被弃用
document.styleSheets[0].insertRule('.box {height: 100px}', 0);	// 插入新样式置顶部
```

### 修改类名

定义不同类下的不同样式，通过修改类名来切换样式

```js
// 直接设置class，注意为className
element.className = 'active'	// 修改class
element.className += ' active'	// 追加class，注意空格
// 通过函数设置
element.setAttribute('class', 'active')
```

## JavaScript- in & includes

[`in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in)为运算符，用于检测指定的属性是否存在于指定对象中

语法：`prop in object`

```js
const car = { make: 'Honda', model: 'Accord', year: 1998 }
'make' in car 	// true
'driver' in car	// false
```

注意检测的目标为属性，而当对象为数组时，实际为检测数组索引而非值

```js
const trees = ["redwood", "bay", "cedar", "oak", "maple"]
0 in trees 			// true
6 in trees 			// false
"bay" in trees		// false
"length" in trees	// true，数组含有固有属性length
```

---

需要检测数组是否存在某值，使用数组的[`includes()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)方法，该方法区分大小写

语法：`arr.includes(valueToFind[, fromIndex])`

+ valueToFind：查找的值
+ fromIndex：从该索引开始寻找，可以为负值

```js
const pets = ['cat', 'dog', 'bat']
pets.includes('dog')	// true
pets.includes('god')	// false
```

## JavaScript-JSON.stringify

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

+ 值调用`toJSON()`方法被转化，日期类型值调用`Date.toISOString()`方法
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

## JavaScript-Argument对象

[`arguments`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments) 是一个对应于传递给函数的参数的**类数组**对象：

+ `arguments`对象是在所有（非箭头）函数中都可用的**局部变量**，包含所有传入参数，按传入顺序排列

+ 参数可以被设置：`arguments[1] = 'new value';`

+ 类数组对象：`arguments`对象除了length属性和索引元素外，没有数组对象的其他属性；但可以被转化为真正的数组对象

  ```javascript
  // 旧版本
  var args = Array.prototype.slice.call(arguments);
  var args = [].slice.call(arguments);
  // ES6
  const args = Array.from(arguments);
  const args = [...arguments];
  ```

+ 在ES6中，可以使用[剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Rest_parameters)更好地代替该用法

## JavaScript-前后补零

ES7的新函数：可以使用<u>指定字符串</u>填充<u>目标字符串</u>至指定长度，有需要则会重复填充，返回新字符串

+ [String.prototype.padStart()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padStart#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9%E6%80%A7)：从前面（左边）开始填充
+ [String.prototype.padEnd()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)：从后面（右边）开始填充
  + `targetLength`：当前字符串需要填充到的目标长度。如果小于原字符串，则原样返回
  + `padString`：可选，填充字符串。默认为`U+0020`空格，如果字符串过长，保留左侧部分

## JavaScript-`switch`语句

```js
switch (expression) {
  case value1:
    [break;]
  ...
  case valueN:
    [break;]
  [default:
    [break;]]
}s
```

注意：`break`语句是可选的。如果某个`case`中**无`break`**，那么会**继续执行**接下来的`case`块中的语句（无视条件！），直到遇到`break`：所以有如下操作：

① 多`case`执行统一操作：前四个`case`，都执行前一个`log`；`Dinosaur`和其他都会执行第二个`log`

```js
switch (Animal) {
  case 'Cow':
  case 'Giraffe':
  case 'Dog':
  case 'Pig':
    console.log('This animal will go on Noah's Ark.');
    break;
  case 'Dinosaur':
  default:
    console.log('This animal will not.');
}
```

② 多`case`叠加操作：下例中，输入0~3会形成不同长度的句子，最终在`case 4`中被打印出来并跳出

```js
var foo = 1;
var output = 'Output: ';
switch (foo) {
  case 0:
    output += 'So ';
  case 1:
    output += 'What ';
    output += 'Is ';
  case 2:
    output += 'Your ';
  case 3:
    output += 'Name';
  case 4:
    output += '?';
    console.log(output);
    break;
  case 5:
    output += '!';
    console.log(output);
    break;
  default:
    console.log('Please pick a number from 0 to 5!');
}
```

## JavaScript-页面嵌套

是用iframe嵌套多级页面时，使用这些全局对象获取页面对象：

+ [`self`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/self)：返回一个指向**当前 window 对象的引用**，通常都用于进行比较
+ [`parent`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/parent)：返回当前窗口的**父窗口**的引用
  + 如果一个窗口没有父窗口，则它的 `parent` 属性为自身的引用
  + 如果当前窗口是一个 `<iframe>`, `<object>`, 或者 `<frame>`，则它的父窗口是嵌入它的那个窗口
+ [`top`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/top)：返回窗口层级**最顶层窗口**的引用

+ [`frames`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/frames)：一个类数组对象，列出了当前窗口的所有直接子窗口

例子：

```js
const bool = (window.self != window.top) // 判断当前窗口是否为嵌套页面
```

## JavaScript-`location.href`

[Location: href](https://developer.mozilla.org/zh-CN/docs/Web/API/Location/href) 是一个字符串转化器，可以返回完整URL值，也可以被更新

常见形式：

```js
self.location.href;			// 当前页面打开URL页面
window.location.href;		// 当前页面打开URL页面
this.location.href;			// 当前页面打开URL页面
location.href;				// 当前页面打开URL页面
parent.location.href;		// 在父页面打开新页面
top.location.href;			// 在顶层页面打开新页面
<frameName>.location.href;	// 在自定义frame窗口打开URL页面
```

注：

+ 可以使用`window.location.href=window.location.href`来刷新页面；效果同`window.location.Reload()`，不同之处在于是否有提交数据：
  + 前者向指定的url提交数据；而后者会提示是否提交
+ `window.location.href=`在原窗口打开，而`window.open()`是打开新窗口

## JavaScript-Date

### 新建对象

Date对象的唯一创建方式是使用`new`操作符，如果直接调用则是返回日期字符串

```js
new Date()	// Mon Jun 05 2023 10:58:17 GMT+0800 (中国标准时间)
Date()		// 'Mon Jun 05 2023 10:57:17 GMT+0800 (中国标准时间)'
```

`Date()`的用法：

```js
// 当前时间
new Date();
// 时间戳，整数类型；如果为字符类型则会解析错误
new Date(value);
// 表示日期的字符串值，等同于调用Date.parser()；但不推荐使用
new Date(dateString);
// 分别提供每个成员
new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
```

针对最后一种方法的赋值。前两项为必填，其他项默认值为理论最小值（日期为1，其他为0）：

+ year：0~99会被添加19成为完整年份
+ **monthIndex**：0~11，分别映射为1~12个月
+ date：从1开始，默认为1
+ hours：0~23
+ minutes、seconds：0~60
+ milliseconds：0~999

如果某个值超出正常值，则进位：

```js
// 都表示2021年1月
new Date(2020, 12)
new Date(2021, 0)
```

### 属性

+ Date.prototype：为Date对象增加属性
+ Date.length：7，为函数可接受的参数个数

### 方法

> 毫秒数：指距离1970年起始的毫秒数

原型方法：

+ Date.now()：返回当前时间的毫秒数
+ Dae.parse()：解析字符串，并返回毫秒数
+ Date.UTC()：接受多个时间参数，并返回毫秒数

实例方法：

+ 基础：
  + getDate()、getDay()（返回星期几：0~6）、getMonth()、get<u>Full</u>Year()
  + getHours()、getMinutes()、getSeconds()、getMilliseconds()
  + 以上除了day值外，都有对应的set的函数
+ `getTime()`、`valueOf()`：两者功能相同，以数值形式返回毫秒数
  + 所返回值可以作为`setTime()`、`new Date()`的入参
  + 可以计算两个Date对象距离的毫秒数
+ toString系列：可以用下列函数的返回字符串作为`new Date()`的入参
  + to：toString()、toDateString()、toTimeString()：Date对象的字符串化
  + toLocale：toLocaleString()、toLocaleDateString()、toLocaleTimeString()：更简洁的字符串形式，因不同语言而不同
  + toUTC：toUTCString()：Date对象的字符串化，但是以UTC时区；没有toDate和toTime方法

```js
const date = new Date()
date.toString()			// "Mon Jun 05 2023 14:25:43 GMT+0800 (中国标准时间)" 
date.toLocaleString()	// "2023/6/5 14:25:43" 
date.toUTCString()		// "Mon, 05 Jun 2023 06:25:43 GMT"
```

## JavaScript-navigator

> navigator /ˈnævɪɡeɪtər/ *n. 领航员*

`Navigator` 接口表示用户代理的状态和标识。它允许脚本查询它和注册自己进行一些活动

通过只读属性[`window.navigator`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/navigator)，可以用于获取运行当前代码应用程序的相关信息

一些常用的属性：

+ [`navigator.geolocation`](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/geolocation) ：设备的地理位置
+ [`navigator.userAgent`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgent)：浏览器的用户代理
+ [`navigator.onLine`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine)：浏览器是否联网
  + 当该值发生改变时（网络情况发生变化），将会触发`online`或`offline`事件

## JavaScript-进制

进制包括如下几种，以不同的前缀而区分：

+ 十进制：无前缀
+ 十六进制：前缀为`0x`
+ 二进制：前缀为`0b`（ES6）
+ 八进制：前缀为`0o`（ES6）

注：前缀不区分大小写

注2：**非严格模式**下，前缀是`0`且后续数字只有0~7时，也被视为八进制，否则视为十进制

### 转化

+ [`parseInt(string, radix);`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)：解析字符串返回十进制整数
  + `radix` 是 2-36 之间的整数，表示输入值的进制
  + 输入值<u>不需要</u>前缀
+ `Number()`：将字符串转为数字，<u>需要</u>前缀
+ `+`：可以直接置于字符串前转为数字
+ `Number.prototype.toString(radix)`：将**数字**转换成对应进制的**字符串**
  + `radix` 是 2-36 之间的整数，默认为10

十进制小数转化为二进制时，会发生无限循环，被截取后发生精度丢失；

常见的有：`0.1 + 0.2 = 0.30000000000000004`

# jQuery

## jQuery-选择器

> 参见：https://www.cnblogs.com/alone2015/p/4962687.html

## jQuery-鼠标与键盘事件

### [event.which](https://www.jquery123.com/event.which/)

该属性针对鼠标与键盘事件，获取**按键值**：

+ 鼠标：左键为1，中键为2，右键为3
+ 键盘：相应 Ascll 码值（不区分大小写）

另：使用`event.type`获取事件类型：如`keydown`或`mousedown`

# Other

## Other-重排重绘

> 参见：[重排(reflow)和重绘(repaint)](https://juejin.cn/post/6844904083212468238)

浏览器渲染界面是基于流式布局模型

> 

页面的生成：

1. HTML被解析为DOM树
2. CSS被解析为CSSOM树
3. 结合前两者，生成一颗渲染树（Reander Tree）
4. 生成布局（flow），生成渲染树的所有节点
5. 绘制布局（paint），显示整个页面

---

### **重排**

因为元素的尺寸、结构或属性发生变化，浏览器重新渲染部分或整个文档的过程；

<u>导致重排</u>的操作：

+ 页面初渲染，是为开销最大的一次重排
+ 添加/删除可见DOM元素
+ 改变元素的一些自身尺寸相关属性：
  + 位置
  + 尺寸：包括边距、边框与宽高
  + 内容
  + 字体大小
+ 改变浏览器大小，如发生`resize`事件
+ 激活CSS伪类，如`:hover`
+ 设置`style`属性；查询某些属性；调用某些方法

重排范围：

+ 全局范围重排：某个元素发生变化，导出整个页面需要重排
+ 局部范围重排：某个在DOM中拥有固定尺寸的元素，内部触发重排时，其他元素因为该元素的总尺寸没有改变，不会触发重排

### **重绘**

因为元素的样式等不影响其位置的属性发生变化，重新绘制元素的过程；

<u>导致重绘</u>的操作：改变元素的一些样式类型属性

**注意：**重排必然导致重绘，但重绘不一定会导致重排

