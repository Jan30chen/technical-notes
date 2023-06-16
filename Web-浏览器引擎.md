# Web-浏览器引擎

## 引擎内核

### WebKit内核

苹果公司自主研发的内核

Safari、Google Chrome、360极速浏览器以及搜狗高速浏览器高速模式

### Gecko内核（Firefox内核）

Netscape6开始采用的内核，后来的Mozilla FireFox(火狐浏览器) 也采用了该内核，有时也会被称为Firefox内核

### Trident内核（IE内核）

微软在Mosaic代码的基础之上修改而来的

IE4沿用到IE11、2345浏览器

### Presto内核（Opera内核）

Presto是一个由Opera Software开发的浏览器排版引擎

供Opera 7.0及以上使用

## CSS中的私有前缀

> CSS3 规范从启动到成为W3C 的推荐标准，一般要经历数年。
>
> 在W3C 开发标准的过程中，浏览器通常会提前实现这些特性。
>
> 浏览器厂商通常都是在属性名称前添加厂商的私有前缀，来测试这些尚未成为标准的特性。因此，可以借助私有前缀，来解决浏览器对CSS3的兼容性问题。

| 私有前缀 | 浏览器内核  |   代表浏览器   |
| :------: | :---------: | :------------: |
|  -moz-   |  Gecko内核  |    FireFox     |
|   -ms-   | Trident内核 |       IE       |
| -webkit- | WebKit内核  | Safari、Chrome |
|   -o-    |  Opera内核  |     Opera      |