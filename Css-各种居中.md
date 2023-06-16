# CSS的各种居中

## 0  元素类型

| display-值   | 描述                      |
| ------------ | ------------------------- |
| none         | 不渲染                    |
| block        | 块级元素                  |
| inline       | 默认-内联元素（行内元素） |
| inline-block | 行内块元素                |

### 0.1 元素类型

不为块元素的可见元素都属于行内元素

#### 0.1.1 块元素的特点:

1. 总是在新行上开始
2. 宽度、高度和行高以及内外边距都可控制
3. 宽度缺省时，它占据父元素的100%
4. 它可以容纳内联元素和其他块元素

#### 0.1.2 行内（内联）元素的特点：

1. 和其他元素都在一行上
2. 宽度、高度及**上下内外边距**不可改变
   1. 设置宽度width 无效；
   2. 设置高度height 无效，可以通过line-height来设置；
   3. 设置margin只有左右margin有效，上下无效；
   4. 设置padding只有左右padding有效，上下则无效，注意元素范围是增大了，但是对元素周围的内容是没影响的。
3. 宽度就是它的文字或图片的宽度，不可改变；高度通过行高来设置（撑开）
4. 内联元素只能容纳文本或者其他内联元素

#### 0.1.3 行内块元素的特点：

1. 和其他元素都在一行上（行内元素特点） 
2.  能够设置宽高，上下内外边距（块级元素特点） 

### 0.2 各种类型标签

#### 0.2.1 常见行内元素

+ ```<a>```：锚点
+ ```<b>```、```<i>```：粗体、斜体
+ ```<br>```：换行
+ ```<img>```：图像
+ ```<input>```：输入框
+ ```<label>```：表格标签
+ ```<select>```：下拉选择框
+ ```<span>```：常见内联容器
+ ```<textarea>```：多行文本输入框

#### 0.2.2 常见块级元素

+ ```<div>```：常见块级容器
+ `<p>`：段落标签
+ ```<form>```：交互表单
+ ```<h>```：各级标题
+ ```<hr>```：分隔线
+ ```<ol>```、```<ul>```、`<li>`：有序、无序列表以及列表项
+ ```<table>```：表格

#### 0.2.3 空元素（没有内容的自闭合标签）？

## 1  水平居中

### 1.1 行内元素水平居中

给**父元素**设置```text-align:center```即可实现 

```html
.center{
	text-align:center;
}
<div class="center">水平居中</div>
```

### 1.2 块级元素水平居中

#### 1.2.1  定宽块级元素水平居中 

**需要居中的块级元素**加```margin:0 auto```即可，适用于有**宽度的块元素**

```html
.center{
    width:200px;
    margin:0 auto;
    border:1px solid red;
}
<div class="center">水平居中</div>
```

#### 1.2.2  **不定宽块级元素水平居中** 

##### 1.2.2.1 设置table

 **需要居中的元素**，设置```display:table```，然后设置```margin:0 auto```来实现

 ```html
  .center{
      display:table;
      margin:0 auto;
      border:1px solid red;
  }
  <div class="center">水平居中</div>
 ```

##### 1.2.2.2   **设置inline-block**（多个块状元素） 

```html
  .center{
      text-align:center;
  }
  .inlineblock-div{
      display:inline-block;
  }
  <div class="center">
      <div class="inlineblock-div">1</div>
      <div class="inlineblock-div">2</div>
  </div>
```

##### 1.2.2.3   **设置flex布局** 

```html
  .center{
      display:flex;
      justify-content:center;
  }
  <div class="center">
      <div class="flex-div">1</div>
      <div class="flex-div">2</div>
  </div>

```

##### 1.2.2.4   position + 负margin

##### 1.2.2.5   position + margin：auto

[参见水平垂直居中](###3.1：绝对定位+margin: auto)

##### 1.2.2.6   position + transform

 **注：**这里方法4、5、6同下面垂直居中一样的道理，只不过需要把top/bottom改为left/right 

## 2 垂直居中

### 2.1 单行文本垂直居中

- 设置`padding-top=padding-bottom`；
- 设置`line-height=height`；

### 2.2 多行文本垂直居中

 通过设置父元素```table```，该元素```table-cell```、```vertical-align```和`text-align:center`

```vertical-align:middle```的意思是把元素放在父元素的中部 

```
.center{
    width: 200px;
    height: 200px;
    display: table;
    border: 1px #000 solid;
}
.table-div{
    display: table-cell;
    text-align:center;
    vertical-align: middle;
}
```



```html
<style type="text/css">
    .center{
        width: 200px;
        height: 200px;
        display: table;
		border: 1px #000 solid;
    }
    .table-div{
        display: table-cell;
        vertical-align: middle;
    }
</style>
<div class="center">
    <div class="table-div">这是一个例子：<br>多行文本<br>垂直居中</div>
</div>
```

### 2.3 块级元素垂直居中

#### 2.3.1 Flex布局

元素的父元素，设置```display:flex```和```align-items:center```

要求：父元素必须设置height值

```html
<style type="text/css">
	.center{
		width: 200px;
		height: 300px;
		
		display: flex;
		align-items: center;
	}
</style>
<div class="center">
	<div>flex布局垂直居中</div>
</div>
```

#### 2.3.2 利用position和top和负margin

元素设置：

+ relative/fixed
+ margin为 负的该元素高度的一半值
+ top设置为50%

```html
<style type="text/css">
	.parent{
		position: relative;
		/* position: fixed; */
		width: 200px;
		height: 300px;
	}
	.child{
		position: absolute;
		
		height: 100px;
		width: 150px;
		top: 50%;
		margin-top: -50px;
	}
</style>
<div class="parent">
	<div class="child">已知高度垂直居中</div>
</div>
```

#### 2.3.3 利用position和top/bottom和margin: auto

[参见水平垂直居中](###3.1：绝对定位+margin: auto)

#### 2.3.4 利用position和top和transform

无需子元素的尺寸

```css
#parent{
    position: relative;
    width: 800px;
    height: 800px;
    background-color: #fffae8;
}
#child{
    position: absolute;
    display: inline;
    top: 50%;
    transform: translate(0,-50%);
    background-color: #5d7a03;
}
```

## 3 水平垂直定位

### 3.1：绝对定位+margin: auto

居中元素需要有宽高，但不需要知道值

```css
div{
    width: 200px;
    height: 200px;
    background: green;
    
    position:absolute;
    margin: auto;
    /*	设置水平居中	*/
    left:0;
    right: 0;
    /*	设置垂直居中	*/
    top: 0;
    bottom: 0;
}
```

### 3.2：绝对定位+负margin

position: absolute或者fixed

需要知道居中元素的宽高，并将`margin`取为一半的相反数

```CSS
div{
    width:200px;
    height: 200px;
    background:green;

    position: absolute;
    /*	设置水平居中	*/
    left:50%;
    margin-left:-100px;
    /*	设置垂直居中	*/
    top:50%;
    margin-top:-100px;
}
```

### 3.3：绝对定位+transform

不需要知道居中元素的宽高

```css
div{
    background: green;
    position: absolute;
    /* 定位父级的50% */
    left:50%;    
    top:50%;
    /* 沿着左上移动各自己的50% */
    transform: translate(-50%,-50%); 
}
```

### 3.4：flex布局

```css
/* 父元素 */
.box{
    height:600px;  
    display:flex;
    justify-content:center;  /* 子元素水平居中 */
    align-items:center;      /* 子元素垂直居中 */
}
.box>div{
    background: green;
}
```

### 3.5：table-cell实现居中

```css
.table{
   	display: table;
    width: 500px;
    height: 500px;
    border:1px solid yellowgreen;
}
.table>.center{
    display:table-cell;
    text-align:center;
    vertical-align: middle;
    background-color:salmon;
}
```

### 3.6：歪门邪道之背景图片

```css
background: url("xxx.png") center center no-repeat;
```

