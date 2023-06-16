# JavaScript-文件对象

> 参见：https://juejin.cn/post/6844904178725158926

## Blob

### 介绍

Blob（Binary Large Object）表示二进制类型大对象，通常用于储存视频、图片或音频等多媒体文件

+ 数据库中，为大量二进制数据的**单一个体**集合
+ JavaScript中，Blob对象表示**不可变**类文件对象的原始数据

Blob对象有两个属性，都为**只读**：

+ `size`：数据大小，以字节为单位
+ `type`：MIME 类型，并不限于js原生数据类型

> [MIME](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)（Multipurpose Internet Mail Extensions），媒体类型，用来表示文件或字节流的性质和格式。浏览器通常使用该类型来确定如何处理URL，而不是靠后缀名。
>
> 格式为：`type/subtype`，常见的 MIME 类型有：超文本标记语言文本 .html text/html、PNG图像 .png image/png、普通文本 .txt text/plain 等。

### 构造

使用[Blob()](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob/Blob)函数构造

```js
var aBlob = new Blob( array, options );
```

+ `array`：要放入的内容，可以为 ArrayBuffer，ArrayBufferView，Blob，DOMString 等对象
+ `options`：可选，包含两个属性
  + `type`：默认为`''`，指定要放入的内容的类型
  + `endings`：指定包含行结束符 `\n` 的字符串如何被写入
    + `"transparent"`：默认值，保持原样不变
    + `"native"`：随宿主操作系统变化

### 方法

[Blob.slice()](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob/slice)：类似字符串，切割Blob对象而生成新的Blob对象：

+ `start`：可选，`start`下标的字节作为首字节
  + 默认：0
  + 负数：指定倒数`start`下标字节
  + 超过原size：得到一个`size`为0，不包含任何数据的新Blob对象
+ `end`：可选，`end-1`下标字节作为尾字节
  + 默认：`size`，截取剩下的所有字节
  + 负数：指定倒数`end-1`下标字节
  + 超过原size：截取剩下的所有字节
+ `contentType`：可选，重新赋予新类型，默认为`''`
  + 注：也就是说，不指定该项时，新生成的Blob对象不会继承原有媒体类型，而是**为空字符串**

### 用处

#### 转化 URL

[URL.createObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)：接受Blob对象，创建并返回 Blob URL

得到的URL可以用于`<a>`、`<img>`等标签，实现文件的导航或下载

示例：文件下载

```js
var link = document.createElement('a')
// 创建Blob对象，再创建URL
var href = window.URL.createObjectURL(new Blob([file]))
// 设置下载URL和下载名称
link.href = href
link.download = decodeURIComponent('download_file_name')
// 添加该元素，模拟点击后，再移除
document.body.appendChild(link)
link.click()
document.body.removeChild(link)
// 从内部映射中删除引用，从而删除内存中的Blob对象
window.URL.revokeObjectURL(href)
```

#### 转换为 Base64

**Base64** 是一种基于 64 个可打印字符来表示二进制数据的表示方法，用以表示、传输与存储二进制数据；将图片或其他文件的二进制数据编码为 Base64，就可以将其作为字符串直接嵌入网页中，减少网络请求

[FileReader.readAsDataURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader/readAsDataUR L)：读取 Blob 对象，读取完成后，`readyState`变为`DONE`，并触发`load`事件，同时`result`属性将包含 base64 编码字符串，表示文件的内容。

```html
<input type="file" accept="image/*" onchange="loadFile(event)">
<img id="output"/>

<script>
  const loadFile = function(event) {
    // 新建FileReader对象，并添加onload事件监听器
    const reader = new FileReader();
    reader.onload = function(){
      const output = document.querySelector('output');
      output.src = reader.result;
    };
    // 调用函数转换
    reader.readAsDataURL(event.target.files[0]);
  };
</script>
```

## File

原型是Blob对象，继承了Blob的方法和属性，也拥有自身独有的属性和方法。

来源：

+ `<input type="file">`选取文件产生的[`FileList`](https://developer.mozilla.org/zh-CN/docs/Web/API/FileList)对象
+ 拖放操作而产生的[`DataTransfer`](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)对象

## ArrayBuffer

[`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)对象为字节数组，用以管理一片内存，不能被直接操作

可以通过[类型数组对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)`TypedArray`或 `DataView` （数据视图）对象来操作它