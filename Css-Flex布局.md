# Css-Flex布局

> 参考：https://zhuanlan.zhihu.com/p/25303493

## 轴

Flex容器中默认存在两条轴：

+ 主轴(main axis)：默认为向右的水平轴
  + 主轴的起始位置是main start，默认是容器的左边
  + 结束位置是main end，默认是容器的右边
+ 交叉轴 (cross axis)：由主轴顺时针90°得来，所以默认为向下的竖直轴
  + 主轴的起始位置是cross start，默认是容器的上边界
  + 结束位置是cross end，默认是容器的下边界

容器中的单元被称为flex item

+ 其占据的主轴长度为main size
+ 占据的交叉轴长度为cross size

## Flex容器

首先指定一个容器为`flex`容器，容器本身默认为`block`元素，也可以设置为`inline`元素

```css
.container{
	display:flex;			/* 块级弹性布局 */
	display:inline-flex;	/* 行内弹性布局 */
}
```

设置为弹性布局后，子元素的`float、clear、vertical-align`属性失效

## Flex容器属性

共六种（第一位是默认值）：

+ `flex-direction`：主轴方向
  + row：从左至右
  + row-reverse：从右至左
  + column：从上到下
  + column-reverrse：从下到上
+ `flex-wrap`：决定容器内项目的换行
  + nowrap：不换行，自动调节元素尺寸
  + wrap：换行
  + wrap-reverse：换行，并且从下往上
+ `flex-flow`：`flex-direction`和`flex-wrap`的简写形式
+ `justify-content`：项目在主轴上的对齐
  + flex-start：对齐主轴开端
  + flex-end：对齐主轴末端
  + center：居中
  + space-between：均匀排列，但是对齐主轴两端
  + space-around：均匀排列，两端间隔为子元素间间隔的一半
  + space-evenly：均匀排列，两端间隔与子元素间隔相同
+ `align-items`：项目在交叉轴上的对齐
  + stretch：如果项目未设置高度，则拉伸同容器，如果有高度则对齐交叉轴开端
  + flex-start：对齐交叉轴开端
  + flex-end：对齐交叉轴末端
  + center：居中对齐
  + baseline：根据第一行文字底部对齐 
+ `align-content`：多根轴线的对齐方式，如果容器设置为不换行，则只有一根轴线不起作用
  + stretch：
    + 从交叉轴开端到末端，如果未设置项目高度，则N等分容器
    + 如果设置项目高度，则各自对齐交叉轴开端
  + flex-start：轴线沿交叉轴排列，第一行和开端相切
  + flex-end：轴线沿交叉轴排列，但最后一行和末端相切
  + center：轴线全部居中
  + space-between：轴线两边对齐，中间均匀分布
  + space-around：轴线均匀分布

## Flex项目属性

首先设置各个项目的flex-basis，计算出剩余空间

此时若有空间剩余，则根据各组件的flex-grow按比例放大

若空间不足，则各组件根据flex-shrink按比例缩小

+ `order`：定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0
+ `flex-basis`：定义了在分配多余空间之前，项目占据的主轴空间
  + 浏览器根据这个属性，计算主轴是否有多余空间
  + 默认值auto，取决于本身的宽高
  + **flex-basis 需要跟 flex-grow 和 flex-shrink 配合使用才能发挥效果**
  + 项目的宽高会失效
+ `flex-grow`：定义项目的放大比例
  + 默认值为0，不放大
  + 有剩余空间时根据所有项目的比例放大
+ `flex-shrink`：定义项目的缩小比例
  + 默认值为1
  + 0为不缩小
  + 空间不足时根据所有项目的比例缩小
+ `flex`：`flex-grow`，`flex-shrink`和`flex-basis`的简写
  + **建议使用这种方式**，方便统一管理
  + initial（0 1 auto）--默认值，一般用以还原默认值
  + auto (1 1 auto) 
  + none (0 0 auto)
  + 单值：
    + 有单位：`1 1 Npx/N%`
    + 无单位：`N 1 0%`
  + 双值：
    + 无单位 + 有单位：`N 1 Mpx/M%`
    + 无单位 + 无单位：`N M 0%`
+ `align-self`：允许单个项目有与其他项目不一样的交叉轴对齐方式
  + 默认值为auto，继承父元素的 align-items 属性；如果没有父元素，为stretch
  + 其他属性都和 align-items 一样

### `flex` 单值的设置场景

> by [zhangxinxu](https://www.zhangxinxu.com/) from https://www.zhangxinxu.com/wordpress/?p=9541

`flex`属性接受三个参数，存在值缺省时，**并不都是被设置为默认值**。详见上述`flex`的属性说明

#### `flex:initial`

等同于设置`flex:0 1 auto`，为默认值

`initial`使用场景：

+ 多个小组件的排版，自动根据内容调整大小
+ 一部分固定大小，另一部分根据内容自动调节大小，并适时换行

#### `flex:0`与`flex:none`

+ 前者表示`0 1 0%`，由于建议宽度为0%，且不能扩展，所以表现为最小宽度（1~2字宽度），*使用场景较少*

+ 后者表示`0 0 auto`，元素被内容撑起大小，且不具有弹性，可能会导致溢出；`none`使用场景：需要保持元素不换行，不会被改变大小的时候（如按钮中的字不换行）

#### `flex:1`与`flex:auto`

分别为`1 1 0% `与`1 1 auto`，都是具有弹性的元素，即可以放大也可以缩小

不同之处：

+ `flex:1`优先收缩自己的尺寸来适配，多个不同基础大小的元素，会被放缩至相同大小
+ `flex:auto`优先考虑自己的尺寸，多个不同基础大小的元素，会根据相对大小进行相应比例的放缩

`flex:1`的使用场景：希望元素充分利用空间，生成**等分元素**，如需要N等分的布局

`flex:auto`的使用场景：同样希望元素充分利用空间，并动态分配适宜比例，如每个项目的文字数不同的导航栏

