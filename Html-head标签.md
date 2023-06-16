# Html-head标签

## `<!DOCTYPE html>`

`<!DOCTYPE>`声明：指示web浏览器对页面使用哪个HTML版本进行编写的指令

+ 必须位于html文档的第一行
+ 不属于html标签，也没有结束标签
+ 大小写**不敏感**

HTML5声明只有一种类型，将其添加到文档头部即可；HTML5 不基于 SGML

```html
<!DOCTYPE html>
```

因为HTML 4.01 基于 SGML，需要引用 DTD 来规定标记语言的规则，

> SGML(Standard Generalized Markup Language，即标准通用标记语言）是现时常用的超文本格式的最高层级标准
>
> HTML是 SGML 的一个实例，遵从 DTD 标准；但 HTML5 不属于这个范畴

![](https://images2017.cnblogs.com/blog/1288677/201801/1288677-20180102165648440-35197995.png)

## `<title>`标签

- 定义浏览器工具栏中的标题
- 提供页面被添加到收藏夹时显示的标题
- 显示在搜索引擎结果中的页面标题

## `<base>`标签

规定了页面上所有链接的**默认基础地址**和**默认打开方式**

放在最前面以便接下来的链接都可以使用，可以被其他链接覆盖

+ `href`属性：规定基础地址，之后的链接可以直接写相对地址
+ `target`属性：
  + `_self`：默认，当前页面打开
  + `_blank`：打开新窗口
  + `_parent`：父框架打开
  + `_search`
  + `_top`

```html
<head>
	<base href="http://www.xxx/" />
	<base target="_blank" />
</head>
<body>
    <!-- 相当于http://www.xxx/img/pic.gif-->
    <img src="img/pic.gif" />
</body>

```

## `<link>`标签

定义文档与外部资源之间的关系，最常用于连接样式表

## `<mete>`标签

META标签用来描述一个HTML网页文档的属性，为最基础的元数据

+ name属性（键）+ content属性（值）：为浏览器提供信息
  + keywords：关键词 
  + description ：概述
  + author：作者
+ http-equiv（键）+ content属性（值）：声明功能
  + conten-Type + charset：使用字符集和编码方式

  ```html
  // HTML5之前的写法
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  // HTML5的写法
  <meta charset="character_set">
  ```

  + Refresh + URL：刷新并跳转其他url
  + Set-Cookie：cookie设定

## `<script>`标签

定义客户端脚本，比如 JavaScript

## `<style>`标签

定义文档的样式信息