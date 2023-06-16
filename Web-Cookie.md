# Web-Cookie

> 参考：https://g.yuque.com/wuzhao/kb/qlc9vk?language=zh-cn

因为Http的无状态特点，服务器不能判断一个客户端的多个请求，cookie为此而生

### 定义

Cookie是浏览器在访问WEB服务器的某个资源时，由WEB服务器在**HTTP响应头**附带**传送给浏览器**的一个纯文本文件

### 原理

1. 客户端**首次**访问服务端时，服务端响应请求，并将cookie放入响应头中，供客户端保存
2. 客户端**非首次**访问服务端前，将保存的cookie自动放入请求头，服务端借此辨别用户身份，并可以修改cookie再返回

### 属性

cookie都具有属性，设置cookie时可以设置这些属性，否则使用属性的默认值

设置属性时，属性之间用`;`和空格隔开：

```
"key=name; expires=Thu, 25 Feb 2016 04:18:00 GMT; domain=ppsc.sankuai.com; path=/; secure; HttpOnly"
```

#### name & value

cookie的形式为键值对：

name：<u>同域名且同名称</u>的cookie会**覆盖**之前的cookie

value：规定不允许包含分号，逗号，空格，所以数据都应该被编码

#### domain & path

域名与路径，指出cookie的使用范围

domain：匹配域名

+ 默认值为当前的[URL.host](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/host)
+ 如果domain值以`.`开头

可以设置为本域名，也可以设置为父域名

**cookie不可跨域**，如下代码在百度的页面下执行，后者不能设置成功，因为域名不同

```javascript
javascript:document.cookie='myname=laihuamin;path=/;domain=.baidu.com';
javascript:document.cookie='myname=huaminlai;path=/;domain=.google.com';
```

path：匹配路由，默认为`/`

例：`/blog`，可以匹配`/blog`、`/blogabc`等

> 特殊：发送XHR跨域请求时，即使满足域和路由范围，默认情况下cookie也不会添加到请求头部

#### expires & max-age

expires：失效日期，必须是GMT格式时间；默认为`session`，会话结束后失效

在新的http/1.1协议中，`expires`被`max-age`选项替代

---

`max-age`为生效时长，即 <u>有效期 = 创建时间 + max-age</u>

单位为**秒**，可以为以下值：

+ 正数：经过`max-age`秒后被删除
+ 负数：有效期为`session`
+ 0：删除cookie；设置为`0`使其失效，失效后浏览器将其从内存中删除

#### secure

当这个属性设置为true时，此cookie只会在https和ssl等安全协议下传输

提示：这个属性并不能对客户端的cookie进行加密，不能保证绝对的安全性

#### HttpOnly

当这个属性设置为true时，就不能通过js脚本来获取cookie的值，能有效的防止xss攻击

## 设置cookie

### 新增

服务端和客户端都可以设置cookie

#### 服务端

在响应头(response header)中，`set-cookie`用以设置cookie

`set-cookie`可以有多个，每个对应设置一个`cookie`，包含键值与属性；实质为字符串

服务端可以设置 cookie 如下选项：`expires`、`domain`、`path`、`secure`、`HttpOnly`

#### 客户端

在控制台中

```javascript
document.cookie = "name=Jonh; ";
```

JavaScript中

```javascript
// 错误，实际只设置了一个
document.cookie = "name=Jonh; age=12; class=111";
// 正确
document.cookie = "name=Jonh";
document.cookie = "age=12";
document.cookie = "class=111";
```

### 修改

使用相同的`path/domin`和`name`，重新赋值`value`

### 删除

使用相同的`path/domin`和`name`，重新赋值`expires`为过去的时间

## cookie、localStorage和sessionStorage 

> 参考：https://juejin.im/post/5a191c47f265da43111fe859

+ 生命周期：
  + cookie：可设置失效时间，没有设置的话，默认是**关闭浏览器后**失效。
  + localStorage：除非被手动清除，否则将会**永久保存**
  + sessionStorage： 仅在当前网页会话下有效，**关闭页面或浏览器后**就会被清除

+ 存储大小
  + cookie：**4KB**左右
  + localStorage和sessionStorage：可以保存**5MB**的信息

+ Http请求时
  + cookie：**每次都会携带在HTTP头中**，如果使用cookie保存过多数据会带来**性能问题**

  + localStorage和sessionStorage：**仅在客户端（即浏览器）中保存**，不参与和服务器的通信

+ 兼容性
  + localStorage和sessionStorage是html5才应用的新特性，可能有些浏览器并不支持，这里要注意

控制台查看

控制台 > Application标签页 > Storage > Cookies、Local Storage、Session Storage

<img src="https://user-gold-cdn.xitu.io/2017/11/25/15ff2f727028f37b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1"  />

## session、cookie和token

- session存储于服务器，可以理解为一个状态列表，拥有一个唯一识别符号sessionId，通常存放于cookie中。服务器收到cookie后解析出sessionId，再去session列表中查找，才能找到相应session。**cookie是其传递sessionId的一种手段**
- cookie类似一个令牌，装有sessionId，存储在客户端，浏览器通常会自动添加。
- token也类似一个令牌，无状态，用户信息都被加密到token中，服务器收到token后解密就可知道是哪个用户。需要开发者手动添加。更加安全