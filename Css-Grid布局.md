# Css-Grid布局

## 启用

```css
display: grid | inline-grid | subgrid;
```

## 容器属性

### 划分网格

```css
grid-template-columns: (<[line-name]>) <track-size> ...;	/* 竖向分割 */
grid-template-rows: (<[line-name]>) <track-size> ...;		/* 横向分割 */
```

+ line-name：非必选，定义网格分割线的名称，必须要加在中括号中；默认值为从1开始的数字
+ track-size：定义网格单位的大小，值可以为：
  + 长度单位
  + 百分比
  + 份数（fr）：对剩余长度的等分（剩余长度：减去长度和百分比的固定长度后，剩余长度）
  + auto：自动填充剩余长度

`repeat()`方法，快速生成重复项

```css
/* 两者相同 */
grid-template-columns: repeat(3, 20px [col-start]) 5%;
grid-template-columns: 20px [col-start] 20px [col-start] 20px [col-start] 5%;
```

### 网格命名

```css
grid-area: <area-name>;	/* 在子元素中 */
grid-area-name: none | "<area-name> | .";	/* 在父容器中 */
```

+ grid-area：将子元素命名为一块区域
+ grid-area-name：将父元素每块grid被划分给不同的区域，注意需要使同名区域内的grid连接为一个矩形
  + none
  + `<string>+`：一行使用一个字符串，多行则使用多个
    + `<area-name>`：划分到指定区域
    + `.`：不划分给任何区域

### 网格间隙

```css
grid-column-gap: <line-size>+;
grid-row-gap: <line-size>+;
grid-gap: <grid-column-gap> <grid-row-gap>;
```

定义网格子项间的间隙，不作用在容器边缘

### 网格排列

> 此属性与flex布局中同名属性类似，取值稍有不同

当**所有网格长度和**小于**父元素长度**时，设置网格如何**在父元素中排列**

```css
justify-content: <content-value>; 	/* 横向上的排列 */
align-content: <content-value>;		/* 竖向上的排列 */
```

`<content-value>`取值：

+ 基本对齐：
  + start：对齐起始（上/左）
  + end：对齐末尾（下/右）
  + center：居中
+ 分布式对齐：
  + stretch：拉伸至填满
  + space-between：等间距放置，但两边无间距
  + space-around：等距离放置，但两边间距只有一半
  + space-evenly：等距离放置

### 网格内排列

当**网格内容长度**小于**网格本身长度**时，定义网格内容**在网格中的排列**

```css
justify-items: <items-value>; 	/* 与列轴的对齐 */
align-items: <items-value>;		/* 与行轴的对齐 */
```

`items-value`取值：

- `start`：内容和网格区域的顶部对齐
- `end`：内容和网格区域的底部对齐
- `center`：内容和网格区域的中间对齐
- `stretch`：默认，填充整个网格区域的高度

### 隐式网格

当定义网格位置超出网格范围时，将自动创建**隐式网格**

定义隐式网格的宽高：

```css
grid-auto-columns: <length>;
grid-auto-rows: <length>;
```

定义隐式网格的排列：

```css
grid-auto-flow: row | column | row dense | column dense;
```

+ row：逐行自动填充
+ column：逐列自动填充
+ dense：“稠密”堆积，后面更小的元素会试图去填充之前的留下的空白，因此元素次序可能发生变化

---

`grid`属性：父容器一些属性的简写形式

```css
grid: none | <grid-template-rows> / <grid-template-columns> | <grid-auto-flow> [<grid-auto-rows> [ / <grid-auto-columns>] ];
```

## 网格属性

### 网格位置

```CSS
/* 中间以|隔开，不是或关系 */
grid-column: <start-value> | (<end-value>);
grid-row: <start-value> | (<end-value>);	
```

`<start-value> `与` (<end-value>)`的取值：

+ number / name：用于指定起始与终止网格线，形成区域；可以为负数，从反方向开始计算
+ span number：以起始线开始，扩展数个网格线
+ auto：自动放置，默认为1

注：`grid-area`不但可以用来命名网格，还可以作为位置属性的缩写：

```css
grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
```

### 网格内排列-单个

可以单独改变某个元素内的自己的排列：

```css
justify-self: <item-value>;	/* 重写justify-items */
align-self: <items-value>;	/* 重写align-items */
```

### 网格顺序

```css
order: <number>;
```

改变某个元素出现的排序，默认都为`0`，数值越大元素排在越后面