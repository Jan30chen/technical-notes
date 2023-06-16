# Css-预编译器

> 参考：https://juejin.im/post/6844904169313140749

CSS 作为一种标记语言，不能使用诸如变量、函数或运算等编程语言所具有的便捷功能，会导致代码难以复用

CSS 预编译器的原理：定义一种编程语言，支持 CSS 原生功能的同时扩展一些编程特性的功能。用该语言书写代码，然后转化为CSS文件使用，这样能提高代码的书写效率。从编程语言到原生 CSS 代码的过程就叫**预编译**

## 预编译语言：Less&Sass

预编译语言可以视作 CSS 的超集，在保留原有功能语法的前提下，提供编程语言的便捷功能

介绍两种预编译语言：

+ Less：向后兼容的 CSS 扩展语言，因为只增加少量方便的扩展，学习起来很容易

> 向后兼容（backward）指旧版本的脚本能够被新版本程序识别运行，这里指CSS文档能够被Less识别运行

+ Sass：兼容所有版本的 CSS，且有无数框架使用sass构建，如Compass，Bourbon和Susy

> 最开始的Sass文件后缀为`.sass`；Sass3引入新语法升级为Sass CSS，文件后缀为`.scss`，在继承Sass的同时，语法完全兼容CSS

## 使用

### Less的使用

#### 变量

使用`@varname:value`命名一个变量，可以在用在通过计算符赋值给其他变量，或规则集中使用

```less
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

#### 混合

通过`selector()`将一段属性混入其他规则集中

```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered();
}
```

#### 嵌套

使用嵌套语句，生成多个前缀相同的选择器，使用`&`符号表示**在此之前的所有**父级选择器前缀

```less
#header {
  color: black;
  .navigation {	
    font-size: 12px;
  }
  & #header {
    width: 300px;
  }
}
```

**注意：**`&`实际代表所有之前的父级选择器，所以不用于选择器开始处时，可能会产生歧义

以下代码本意是，选取作为父元素`rank`第一个为`item`的元素，使其下的`item-num`元素设置颜色为红；

但是编译后，`&`实际被转化为`.rank .item`，实际试图寻找作为第一个子元素的`item`中的`rank`子元素中的`item-num`子元素，多出一级

```less
//	编译前
.rank {
  .item {
    &:first-child > &-num {
      color: red;
    }
  }
}
//	编译后
.rank .item:first-child > .rank .item-num {
  color: red;
}
```

#### 运算

算术运算符 `+`、`-`、`*`、`/` 可以对任何**数字、颜色或变量**进行运算

如果可能的话，算术运算符在加、减或比较之前会进行单位换算。计算的结果以**从左到右第一次出现的单位类型**为准。

如果单位换算无效或失去意义，则忽略单位。

#### 函数

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等，详见[Less 函数手册](https://less.bootcss.com/functions/)；使用形式是很常见的`function()`

#### 命名空间和访问符

类似调用类方法一样，调取某命名空间中的一段规则集，混入当前代码

创建一个命名空间

```less
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}
```

访问，会在规则集混入`.button`规则集（不包括button及外花括号）

```less
#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

结果

```css
#header a {
  color: orange;
  display: block;
  border: 1px solid black;
  background-color: grey;
}
#header a:hover {
  background-color: white;
}
```

#### 映射

类似调用 JS 对象属性一样，调取某个规则集的单独一个属性

```less
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

#### 作用域

由本作用域向父作用域不断寻找变量或混合，并且混合与变量的**使用可以在声明前**

#### 注释

行注释与块注释都可以使用。

块注释在编译后依然被原样，

行注释是**静默注释**，编译后不被保留。

```less
/* 这一段注释编译后依然存在 */
// 这一段注释编译后会消失
```

编译后

```css
/* 这一段注释编译后依然存在 */
```

#### 导入

使用`@import`导入其他文件（CSS、less文件），使用其中变量

导入Less文件时，可以省略文件后缀

### Sass的使用

大部分操作与Less类似

#### 变量

使用`$`前缀声明变量

#### 嵌套

除了选择器，属性也可以嵌套

```scss
nav {
  border: {
      style: solid;
      width: 1px;
      color: #ccc;
  }
}
```

结果为

```css
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

#### 混合器

使用`@mixin name`声明一个混合器，再使用`@include name`来使用、

#### 默认变量

`!default`声明默认值，如果**变量**没有被赋值则赋值为默认值，如果被赋值过就用赋值的值

```scss
$fancybox-width: 200px !default;
```

## 运行编译

### 自带编译

#### Less

##### 本地 Node 环境编译

安装NPM包

```cmd
npm install -g less
```

编译

```cmd
lessc styles.less styles.css
```

##### 浏览器环境编译

引入less文件

```html
<link rel="stylesheet/less" type="text/css" href="styles.less" />
```

引入编译用的 JS 文件

```	html
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.11.1/less.min.js"></script>
```

#### Sass

##### 命令行编译

Sass基于Ruby语言，首先下载Ruby

再使用 Ruby 自带软件下载

```
gem install sass
```

编译

```
//单文件转换命令
sass input.scss output.css

//单文件监听命令
sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
sass --watch app/sass:public/stylesheets
```

### 使用编译软件

使用编译软件，如 [Koala](http://koala-app.com/)，支持Less与Sass的编译

### 使用编译插件

[Live Sass Compiler](https://www.sass.hk/skill/sass154.html)：VS Code扩展，可通过实时浏览器重新加载来帮助您实时将SASS / SCSS文件编译/转换为CSS文件

## 注意

### [calc()函数](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc())

`css`原生支持`calc`函数，在`less`中使用该函数会直接执行计算并得到结果，如果某项为变量，将会丢失整个式子的变化性

如使用该函数实现随窗口大小变化而变化的样式，清除页面抖动的功能

## `&`的嵌套陷阱

详见[嵌套](####嵌套)一节的注意事项

