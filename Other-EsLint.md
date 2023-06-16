# Other-EsLint

> 参考：https://juejin.cn/post/6844903901292920846

EsLint 是代码检查工具的一种，作用包括：避免低级语法错误、清除无用代码部分和统一代码风格

## 使用

### 初始化

初始化配置，在此之前确保已安装`EsLint`和初始化`package.json`

```bash
# 安装 ESLint
$ npm install eslint --save-dev

# 初始化 package.json
$ npm init

# 初始化 ESLint 配置，并生成 .eslintrc.js 文件
$ eslint --init (-yes)
```

注意：

+ 不推荐全局安装`EsLint`，因为这会导致配置共享
+ 因为`EsLint`很消耗性能，最好只使用于开发环境中

### 调用

之后就可以对任何文件调用`EsLint`了，可以使用`npx`与`yarn run`等方式：

```bash
# 对某个文件调用
$ npx eslint somefile.js 

# 对某个目录下所有文件调用
$ npx run eslint somepath/**

# 对所有文件调用
$ npx run eslint .
```

### 参数

```bash
# 自动修复（部分）
$ npx eslint somefile.js --fix

# 只显示错误
$ npx eslint somefile.js --quiet
```

## 规则配置

配置规则有如下方法：

### 注释插入

> 参考：https://lbbdegitbook.gitbooks.io/airbnb/content/zhu-shi-jin-yong-gui-ze.html

一般这种方法用来临时禁止某些严格的 lint 规则所产生的警告

```js
/* eslint-disable */
alert('禁用注释间范围的部分');
/* eslint-enable */

/* eslint-disable-next-line */
alert('当前行（注释的下一行）禁用')

alert('当前行禁用') // eslint-disable-line

/* eslint-disable no-alert, no-console */
alert('禁用某些规则')
/* eslint-enable no-alert, no-console */
```

将`eslint-disable`置于文件顶部，对该文件都禁用

```js
/* eslint-disable */
alert('foo');
```

### 配置文件

使用配置文件，从当前目录开始向上寻找配置文件，直至根目录或含有`"root": true`配置项的文件

越靠近当前目录的文件，优先级更高

### 字段

配置文件为实际为`json`类型对象，`.eslintrc.js`文件实际作用为导出该对象

该对象可以含有以下字段：

+ root：设置为`true`时，eslint不再向上级目录查找`.eslintrc.js`规则

+ parse：配置器类型
+ parserOptions：配置器参数
+ globals：引用全局变量，为键值对的形式，排除全局变量导致的报错；
  + 变量名设为`false`时，意为该变量只读
+ env：环境，引用一套对应的全局变量，避免手动输入；各个环境的预设值见[该文件](https://github.com/eslint/eslint/blob/v6.0.1/conf/environments.js)
+ rules：配置具体规则，可以为字符串、数字（规则等级）或数组（深入配置规则，此时数组第一位为规则等级）
  + off/0：关闭规则检查
  + warn/1：违反规则时，只警告不中断程序
  + error/2：违反规则时，抛出错误
+ extends：扩展，他人的 lint 配置集合
+ plugins：插件，原生 EsLint 只能检查 JavaScript 代码，安装其他插件以定义额外的规则