# Webpack3

## 介绍

webpack是将项目中各种各样的静态资源文件视为模板,然后打包到最后的输出文件中

<img src="http://stepimagewm.how2j.cn/7949.png" style="zoom: 100%;" />

## 过程

### 安装

```CMD
npm install -g webpack@1.13.2 #全局安装并规定版本
```

### 运行

```
webpack a.js bundle.js
```

### 运用

```html
<html>
    <head>
        <script src="bundle.js"></script>
    </head>
</html>
```

### 命令行运行

> webpack.config.js 配置文件

```javascript
module.exports = {
  entry: './a.js', #输入文件名
  output: {
    filename: 'bundle.js' #输出文件名
  }
};
```

> CMD

```cmd
webpack #不加参数直接运行(在当前目录下)
```

### webpack-dev-server

一个**微型服务器**，用以测试在服务器上的网页

#### 安装

```
npm install -g webpack-dev-server@1.15.0
```

#### 运行

```
webpack-dev-server --open
```

#### 修改启动端口

修改 webpack.config.js

```javascript
module.exports = {
    entry: './a.js',
    output: {
        filename: 'bundle.js'
    },
    devServer: {
        port:8088 #端口号
    }
}
```

重新输入启动命令即可

#### 热更新

> webpack-dev-server 支持热更新。 所谓的热更新，即在 webpack.config.js 中的 entry 文件 ( a.js ) 发生了改变之后，会自动运行 webpack, 并且自动刷新页面，立即看到修改之后的效果。

为了做到这一点，需要**修改 webpack.config.js 文件**。

```javascript
var webpack = require('webpack') // 导入webpack模板

module.exports = {
    entry: './a.js',
    output: {
        filename: 'bundle.js'
    },
   plugins:[
        new webpack.HotModuleReplacementPlugin()
    ],
   devServer: {
        port:8088,
        inline:true,
        hot:true
    }
}
```

因为 webpack 模块是全局的，在某些情况下，通过这种方式导入不能够被识别，需要进行一次链接：

```
npm link webpack
```

之后重新启动就可以了

#### html和css监视更新

暂缺

### npm启动

*大多数项目采取这种方式*

运行如下命令在本地目录生成 package.json 配置文件， `-y`的意思是全部采用初始设置

``` nase
npm init -y
```

修改 package.json，增加scripts项目:

```json
 "dev": "webpack-dev-server --open"
```

修改后的package.json:

```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "main": "a.js",
  "dependencies": {
    "webpack": "^1.13.2"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

执行， `dev`即是添加属性的键

```base
npm run dev
```

### 多入口(entry)文件

有时候，会不止一个 js 文件，会有多个。 本知识点就是讲解这种情况下如何进行配置处理

修改webpack.config.js

```javascript
module.exports = {
    entry: {
        bundle1: './a.js',
        bundle2: './b.js'
    },
    output: {
        filename: '[name].js'
    },
    devServer: {
        port:8088
    }
}
```

修改 index.html 为如下代码, 分别引用两个打包的文件

```html
<html>
    <head>
        <script src="bundle1.js"></script>
        <script src="bundle2.js"></script> 
    </head>
</html>
```

以npm方式运行测试

### 使用 BABEL-Loader(ES6到ES5适配器)

因为 ES6 标准推出时间还不够久，所以并不是所有的浏览器都支持 ES6 的运行。 因此，需要把 ES6 的 javascript 代码，转换为 ES5 标准的代码，以期能够在当下浏览器上兼容运行。

转换工具有很多种， **babel** 就是其中的一种

#### 下载

```
npm install --save-dev babel-Loader@6.2.7 babel-core@6.18.0 babel-preset-latest@6.24.1
```

#### 使用ES6的 a.js

```
const name = 'ES6'
document.write(`hello ${name}`)
```

#### 修改webpack.config.js

```javascript
module.exports = {
  entry: {
      bundle1: './a.js'
  },
  output: {
      filename: 'bundle.js'
  },
  devServer: {
      port:8088
  },
  module:{
      loaders:[
          {
              test:/\.js$/, //正则仅匹配js文件
              Loader: 'babel', //所使用Loader
              query:{
                  presets: ['latest'] //表示用最新的语法规则进行
              }
          }
      ]
  }
}
```

#### 修改回index.html

```html
<html>
    <head>
        <script src="bundle.js"></script>
    </head>
</html>
```

启动检测

###  CSS-Loader和Style-Loader

在 webpack 的概念里，所有文件都是模块

用 **css-Loader 和 style-Loader** 把 css 文件作为一个模块打包到 bundle.js 文件里

#### 安装

```
npm install style-Loader@0.13.1 css-Loader@0.25.0 --save-dev
```

#### 准备css样式文件

style.css

```css
body {
    background-color: blue;
}
```

#### 修改webpack.config.js

增加 style-Loader 和 css-Loader

```javascript
module.exports = {
  entry: {
      bundle1: './a.js'
  },
  output: {
      filename: 'bundle.js'
  },
  devServer: {
      port:8088
  },
  module:{
      loaders:[
          {
            test:/\.js$/, //正则仅匹配js文件
            loader: 'babel', //所使用loader
            query:{
                presets: ['latest'] //表示用最新的语法规则进行
            }
          },
          {
            test:/\.css$/,
            loader: 'style-loader!css-loader'
          }
      ]
  }
}
```

启动检测

### 其他Loader

+ HtmlWebpackPlugin
+ ExtractTextPlugin
+ Image-loader 

## Loader

> 参见：https://juejin.cn/post/6847902222873788430

### 介绍

语法方面，loader为普通的Node.js模块，但需要作为函数导出

功能方面，loader作用于特定格式的资源文件，再返回指定格式的输出；如 less-loader 将 less 文件转化为 css 文件

特点：

+ 单一职责：一个loader只负责一种文件
+ 支持链式调用，对一种文件可以前后执行多个loader

### 配置

#### VUE2

配置于`webpack.conf`的`module.rules`属性下，为一个**对象数组**，每个对象元素对应一个规则（一种文件）

规则的执行顺序为栈（**后进先出**）

---

`rules`数组中元素属性：

+ `test`：指定适用范围，可以为字符串、正则表达式、函数、数组
  + 字符串：为**绝对路径**，或使用`node`的`path.resolve()`方法转化相对路径
  + 函数：参数为资源的绝对路径，返回`true`表示该资源适用规则
  + 数组：定义多个规则，每条都可以为字符串、正则表达式、函数，互相为**或**关系
+ `include`：扩展适用范围，用法同
+ `exclude`：缩小适用范围，用法同
+ `resource`：自定义范围，定义此项后，`test`、`include`、`exclude`选项失效；子选项有：
  + `test`选项，同
  + `exclude`选项，同
  + `include`选项，同
  + `not`选项，数组：符合*任意一项*则**排除**
  + `and`选项，数组：符合*每一项*则**添加**
  + `or`选项，数组：符合*任意一项*则**添加**
+ `use`：指定所使用的 loader，可以为多个，执行顺序为栈（**后进先出**）
  + 基础：`{ loader:'css-loader' }`
  + 简写：`css-loader`
  + 附加选项：`{ loader:'css-loader', options: {} }`
+ `loader`：`loader: 'css-loader'`为`use: { loader:'css-loader' }`的简写
+ `enforce`：改变默认后进先出的执行顺序
  + `pre`：优先执行
  + `post`：最后执行
  + `normal`：默认

#### VUE Cli3

使用 [webpack-chain](https://github.com/Yatoo2018/webpack-chain/tree/zh-cmn-Hans) 包来对`vue.config.js`文件进行修改
