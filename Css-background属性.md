# Css-Background属性

## 介绍

用以设置元素的背景，是一个简写属性，可以声明以下属性：

+ [`background-clip`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)：设置背景颜色的范围
  + 为`border-box`时，边框为非固实（点状线、虚线）时可以看见效果
  + 可以使用`text`将背景设置为文字的前景色，注意设置`color:transparent`将文字设置为透明
+ [`background-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-color)：设置背景颜色
+ [`background-image`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-image)：设置背景图片
+ [`background-origin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-origin)：设置背景图片的范围
+ [`background-position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-position)：设置背景图片的位置
+ [`background-repeat`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-repeat)：设置背景图片是否重复
+ [`background-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)：设置背景图片的大小
+  [`background-attachment`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)：设置背景图片是否根据滚动条滚动

*这些属性在没有被设定时，都会有一个默认值*

## 语法

可以设置多层背景图，用逗号分隔

每层设置可以包含以下属性

+ attachment：指定滚动方式
+ bg-image：指定背景图片
+ position
  + 单值：
    + 设置一个方向的关键词（top、bottom、left、right），另一个方向则默认为`center`
    + 设置距离**左侧**的百分比或具体值，上下默认设置为`center`
    + ~~关键词`contain`与`cover`无效！~~
  + 双值：设置两个方向的关键词、百分比或数值
+ bg-size：
  + 单值-带单位：图片设置具体宽度，高度同比放缩
  + 单值-百分比：图片设置为本元素的百分比宽（不为原图尺寸的百分比），高度同比放缩
  + 双值：分别设置宽高度
+ repeat-style：设置重复方式
+ box：
  + 单值：同时设置两种范围（颜色范围与图片范围）
  + 双值：前者设置背景图片范围，后者设置背景颜色范围
+ bg-color：设置背景颜色，多层背景时只能被包含在最后一层设置中，设置在之前层无效（不会有效果，而不是掩盖之后的背景层）

注意：`position`与`bg-size`属性必须写成**`position/bg-size`的格式**，避免混淆

举例：

```css
background: url("xxx.jpg") no-repeat center center/50% 50% padding-box border-box, url("yyy.jpg") no-repeat center center/70% 70% padding-box border-box, #6c7f97;
```

