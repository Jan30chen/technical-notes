# Other-Package

> 参见：[关于前端大管家 package.json，你知道多少？](https://juejin.cn/post/7023539063424548872#heading-13)

项目文件`Package.json`的配置项

## 必须属性

### `name`

项目名称

### `version`

项目版本，格式为 `主版本号.次版本号.修订号(-先行版本.先行版本号)`

+ 主版本：大的功能修改
+ 次版本：新增新功能
+ 修订号：修复bug
+ 先行版本：
  + alpha：内部版本
  + beta：公测版本
  + rc：候选版本

## 依赖配置

配置项为键值对，为模块名称与版本号：

版本号前的各种字符：

+ 固定版本：如`4.0.3`版本
+ 波浪号：`~4.0.3`，表示`4.0.x`中的最新版本
+ 插入号：`^17.0.2`，表示`17.x.x`中的最新版本
+ `latest`：最新版本

### `dependencies`

**生产环境**中所需要的依赖，使用如下命令，安装包并写入

```bash
npm i <package>		# 默认写入
yarn add <package>	# 默认写入
npm i --save/-S <package> # save参数
```

### `devDependencies`

**开发环境**中所需要的依赖，使用如下命令，安装包并写入

```bash
npm install --save-dev/-D <package> # save-dev参数
yarn add --dev <package>			# yarn方法
```

### `peerDependencies`

供依赖指定其所需要的依赖的版本

下列配置表示：安装`chai-as-promised`模块时，必须安装1.x版本的`chai`

```json
"name": "chai-as-promised",
"peerDependencies": {
   "chai": "1.x"
}
```

### `optionalDependencies`

可选配置，当找不到包或者安装包失败时，npm仍然能够继续运行

会覆盖`dependencies`中同名的包

### `bundledDependencies`

配置项为<u>数组</u>，指定一些模块，将在这个包发布时被一起打包

模块必须在`dependencies`或`devDependencies`被定义

### `engines`

对一些旧项目，可能对`npm`或`Node`版本有要求，使用该字段说明具体的版本号要求

```json
"engines": {
	"node": ">=8.10.3 <12.13.0",
 	"npm": ">=6.9.0"
}
```

注意：此处只做说明作用，不会实质影响依赖包的安装

## 脚本配置

### `scripts`

内置的脚本入口，形式为键值对，使用`npm run <key>`来执行命令；使用`pre`与`post`前缀，完成前置与后续操作

```json
"scripts": {
    "predev": "node beforeIndex.js",
	"dev": "node index.js",
  	"postdev": "node afterIndex.js"
}
```

当执行`npm run dev`，从上往下依次执行

### `config`

配置`scripts`运行时的配置参数

```json
"config": {
	"port": 3000
}
```

脚本文件中，使用`npm_package_config_port`获取该字段

使用命令`npm config set foo:port 3001`重写该字段

## 文件&目录

### `main`

指定加载的入口文件，默认为根目录下的`index.js`文件

```json
"main": "./src/index.js"
```

### `browser`

> browser*/*ˈbraʊzə(r)/ n.浏览器

定义npm包在browser环境下的入口文件

```json
"browser": "./src/index.js"
```

### `module`

如果npm包导出的是ESM规范的包，则使用`module`

```json
"module": "./src/index.mjs",
```

注意：`.js`文件是使用commonJS规范的语法（require('xxx')）；`.mjs`是用ESM规范的语法（import 'xxx'）

<u>加载顺序：</u>

+ Web环境：
  + 使用ESM方式加载：browser > module > main
  + 使用require加载CommonJs：main > module > browser

### `bin`

用来指定各个内部命令对应的可执行文件的位置；如下例所示，指定命令文件路径

```json
"bin": {
  "someTool": "./bin/someTool.js"
}
```

之后可以简写命令为：

```json
scripts: {  
  start: './node_modules/bin/someTool.js build'
  // 简写
  start: 'someTool build'
}
```

## 第三方

### `eslintConfig`

`eslint`的配置，可以写在此处，也可以写在单独的配置文件`.eslintrc.json`文件中

### `babel`

指定Babel的编译配置