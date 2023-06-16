# Other-Yarn

> yarn/jɑːn/：n.纱线

[yarn](http://yarnpkg.top/index.html) 是一个依赖管理工具，类似 npm

## 常用命令

### 新建项目

生成`package.json`文件，期间可选不同配置

```
yarn init
```

### 添加

添加并写入`package.json`相应属性

+ 基本：添加依赖包，并写入`dependencies`属性中
+ `<version/tag>`：指定包版本号或标签名进行添加
+ `dev`：添加，但写入`devDependencies`属性中
+ `path`：从其他途径加载包（如本地）

```
yarn add <package-name>
yarn add <package-name>@<version/tag>
yarn add <package-name> --dev
yarn add <package-name> <path>
```

> 包版本号，一般为`x.y.z`这样的样式，分别代表：
>
> + x-主版本号(major)：**不**向下兼容的较大改动
> + y-次版本号(minor)：向下兼容的功能修改
> + z-修订版本号(patch)：向下兼容的问题修复

### 更新

更新并修改`package.json`

```
yarn upgrade <package-name>
```

### 安装所有

安装`package.json`中所有依赖

+ 直接`yarn`就可以，也可以用`yarn install`
+ 添加`force`属性，强制重新安装所有包，即使已经存在

```
yarn
yarn install
yarn install --force
```

### 查看已安装

+ 查看已安装的所有包列表
+ 查看某个安装包，使用`pattern`属性置于包名前，多个包使用`|`间隔

```
yarn list
yarn list --pattern <package-name>
```

### 移除

移除并修改`package.json`

```
yarn remove <package-name>
```

