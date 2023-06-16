# JavaScript-Ajax

> 参考：
>
> https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX/Getting_Started
>
> https://blog.csdn.net/tb3039450/article/details/52145583

## 什么是Ajax？

> AJAX是异步的JavaScript和XML（**A**synchronous **J**avaScript **A**nd **X**ML）。简单点说，就是使用 `XMLHttpRequest` 对象与服务器通信。 它可以使用JSON，XML，HTML和text文本等格式发送和接收数据。
>
> AJAX最吸引人的就是它的“异步”特性，也就是说它可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面。

## Ajax的运行原理

Ajax是基于`XMLHttpRequest`（XHR）对象实现的

> XMLHttpRequest：JavaScript向服务器发送一个http请求所必需的一个请求实例，本身包含一些函数功能

### 发送前

首先新建一个XMLHttpRequest实例对象

```js
if (window.XMLHttpRequest) { // Mozilla, Safari, IE7+ ...
    httpRequest = new XMLHttpRequest();
} else if (window.ActiveXObject) { // IE 6 and older
    httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
}
```

指定该实例处理响应的回调函数

```js
// 指定函数名，或直接指定匿名函数
httpRequest.onreadystatechange = nameOfTheFunction;
```

发送GET请求，此时`send()`参数为空

而`open()`的三个参数分别是：

1. 请求类型：常见有GET和POST，请保持全部大写，避免出现错误
2. 请求URL：由于安全原因，默认不能调用第三方URL域名；并请保证书写正确的域名
3. 是否异步：可选，默认为`true`：异步操作；可以让代码不在此被阻塞

```js
httpRequest.open('GET', 'http://www.example.org/some.file', true);
httpRequest.send();
```

或者发送POST请求

**此时`sent()`前需要调用实例对象的`setRequestHeader()`**，用以设置HTTP请求头部（指明发送的数据类型、字符集等）

```js
httpRequest.open('POST', 'http://www.example.org/some.file', true);
httpRequest.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
httpRequest.send("name=value&anothername="+encodeURIComponent(myVar)+"&so=on");
```

### 发送后

回调函数中，首先获取请求状态是否正常（是否为XMLHttpRequest.DONE，也就是4）

```js
if (httpRequest.readyState === XMLHttpRequest.DONE) {
    // Everything is good, the response was received.
} else {
    // Not ready yet.
}
```

再获取Http状态码，如果是200状态码，该Ajax请求成功

```js
if (httpRequest.status === 200) {
    // Perfect!
} else {
    // There was a problem with the request.
    // For example, the response may have a 404 (Not Found)
    // or 500 (Internal Server Error) response code.
}
```

请求成功后，使用请求实例的两个方法获取返回数据：

- `httpRequest.responseText` – 服务器以文本字符的形式返回
- `httpRequest.responseXML` – 以 XMLDocument 对象方式返回，之后就可以使用JavaScript来处理

```js
// 直接使用返回文本
if (httpRequest.readyState === XMLHttpRequest.DONE) {
    if (httpRequest.status === 200) {
        alert(httpRequest.responseText);
    } else {
        alert('There was a problem with the request.');
    }
}
```

处理XML格式

```js
var xmldoc = httpRequest.responseXML;
var root_node = xmldoc.getElementsByTagName('root').item(0);
alert(root_node.firstChild.data);
```

## jQuery-Ajax系列函数

### Get函数

```js
$.get(URL,data,function(data,status,xhr),dataType)
```

参数说明：

+ URL：请求url
+ data：数据
+ function：成功回调函数，失败回调函数可以后接`.fail(function(){})`（jQuery1.5以上可以使用）
+ dataType：希望返回的数据类型

### Post函数

```js
$.post(URL,data,function(data,status,xhr),dataType)
```

### getJSON函数

```js
$.getJSON(URL,data,function(data,status,xhr))
```

限定返回数据，可以是 JavaScript 对象，或以 JSON 结构定义的数组，并使用`$.parseJSON()`方法进行解析

[关于JSON](JavaScript-JSON.MD)

### Ajax函数

使用键值对对象，设置请求的详细参数

```js
$.ajax({name:value, name:value, ... })
```

实例

```js
$.ajax({
    type: 'POST',	
    url: 'XXX',
    data:{ },	//给后台的数据
    dataType: "text",	//预期返回内容
    async: true,	//是否异步
    beforeSend: function(){ },  //发送前执行  
    success: function(obj){ },	//请求成功执行
    error: function(data){ }	//请求失败执行
});
```

