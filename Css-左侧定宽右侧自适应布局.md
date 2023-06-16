# Css-左侧定宽右侧自适应布局

> 参考：https://www.jianshu.com/p/01747a08bcbf

## 左侧浮动

左侧设置左浮动，右侧宽度设置100%

```css
.left{
    float:left;
}
.right{
     width:100%;
}
```

左边浮动，右边加上一个margin-left值

```css
.left{
    float:left;
}
.right{
    margin-left: 200px; /*等于左边栏宽度*/
}
```

## Flex布局

```css
body{
    display: flex;
}
.right{
    flex:1;
}
```

## 负margin

调换左右的位置

```jsx
<div class="container">
    <section class="right">Right</section>
</div>
<aside class="left">Left</aside>
```

```css
.left{
    float:left;
    margin-left: -100%;
}
.right{
    margin-left: 200px;
}
.container{
    float:left;
    width:100%
}
```

## 两侧绝对定位

```css
.left{
    position: absolute;
    left:0;
}
.right{
    position: absolute;
    left:200px;
    width:100%
}
```

## Table布局

```css
body{
    display: table;
    width:100%;
}
.left{
    display: table-cell;
}
.right{
    display: table-cell;
}
```