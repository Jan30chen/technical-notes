# Css-transform属性

[`transform属性`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform) 可以使给定元素形变，包括但不限于缩放、旋转、倾斜与平移

<span style="background-color:#faec7c;">只能转换由盒模型定位的元素。根据经验，如果元素具有`display: block`，即为盒模型定位元素</span>

该属性可以设置为：

+ `<transform-function>`：要应用的一个或多个形变函数
+ `none`：清空形变

**注意：**设置多个形变时，请于单个`transform`属性中，用空格隔开多个形变函数；而非设置多个`transform`属性 ，否则后者将覆盖前者

## 形变函数

### [`rotate`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate())

使元素按变换原点**Z轴旋转**，该点由[`transform-origin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)属性指定，默认为元素的中心点

用法：`rotate(a)`

+ a为旋转幅度，取值为正，则元素按顺时针旋转，否则按逆时针旋转
+ a 的取值单位包括：
  + `deg`：角度，360度 = 〇
  + `turn`：圈数，一圈 = 〇
  + `grad`：梯度，400梯度 = 〇
  + `rad`：弧度，2π弧度 = 〇

其他用法：

+ [`rotateX()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateX())：作X轴旋转
+ [`rotateY()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateY())：作Y轴旋转
