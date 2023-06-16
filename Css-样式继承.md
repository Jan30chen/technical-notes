# Css-样式继承

> 参考：
>
> https://developer.mozilla.org/zh-CN/docs/Web/CSS/inheritance
>
> https://juejin.im/post/6861772550972801031#heading-7

## 继承属性与非继承属性

对于一个 CSS 属性，其概述都会定义其**继承性(Inherited) **，由此决定在元素未指定值的情况下，是否从父元素继承同属性的值。

> **注意：**如果父元素也未设置值，不再向祖先元素寻找，而是和父元素一样取为初始默认值

因此 CSS 属性也就据此分为两大类：

+ 继承属性：当继承属性未指定值时，取父元素的同属性的值；如果为文档根元素，则取该属性的初始值
+ 非继承属性：当非继承属性未指定值时，直接取该属性的初始值

> 属性的初始值(initial value)：CSS 属性概述中，会定义一个默认的初始值

## 常见属性的继承性

### 继承属性

#### 所有元素可继承属性

+ `cursor`：光标样式
+ `visibility`：元素可见性

#### 内联元素可继承属性

+ 字体属性
  + `font-family`：使用字体
  + `font-weight`：字体加粗量
  + `font-size`：字体大小
+ 文本属性
  + `color`：文本颜色
  + `line-height`：文本行高
  + `text-shadow`：文本阴影
  + `white-space`：空白符处理
  + `word-spacing`：单词间距
  + `letter-spacing`：字母间距
  + `text-transform`：文本大小写
  + `word-break`：单词如何换行
  + `overflow-wrap`：是否允许单词换行

#### 块元素可继承属性

+ 文本属性
  + `text-indent`：（块元素）首行文本前的缩进量
  + `text-align`：控制块元素内的行内元素如何对齐
+ `list-style`：列表样式

### 非继承属性

+ `display`：元素渲染类型
+ 定位属性
  + `float`：控制浮动
  + `clear`：清除浮动
  + `position`：定位属性
  + `left、right、top、bottom`：间距
+ 盒模型属性
  + `width`、`height`：宽、高
  + `margin`：外边距
  + `padding`：内边距
  + `border`：边框
  + `box-sizing`：盒模型类型
+ `background`：背景属性
+ `z-index`：层级属性
+ `content`：元素内容

## 修改继承性

在任何一个属性都可以取为以下值，可以修改其继承性：

+ `inherit`：开启继承性，从父元素继承该属性值
+ `initial`：关闭继承性，设置元素属性为初始值
+ `unset`：恢复属性的默认继承性

## 特例

1. `<a>`标签的字体颜色默认不继承父元素
2. `<h>`标签的字体属性大部分不继承父元素