# Other-命令

端口

```bash
netstat -aon|findstr "8080" # 查看占用端口的进程号
tasklist|findstr "2448"		# 根据进程号查看进程名
taskkill /t /f /im "2448"	# 根据进程号结束进程
```

## NPM

### 源

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org	# 新增下载源（cnpm）
npm config set registry https://registry.npm.taobao.org 		# 更换原有下载源
npm config get registry 										# 查看当前下载源
```

> 默认下载源：https://registry.npmjs.org/

### 安装

```bash
npm install <pack-name>	# 安装某个包
npm i <pack-name>		# 上条的简写
npm i -g <pack-name>	# 全局安装
```

全局安装包位置：`C:\Users\<user-name>\AppData\Roaming\npm\node_modules`

### 更新

需要先安装`npm-upgrade`

```bash
npm i -g npm-upgrade		# 安装npm-upgrade
npm upgrade <packageName>	# 更新
```

### 查看

```bash
npm show <pack-name>	# 包最新线上版本信息

npm list --depth=0		# 本地全部包信息
npm list <pack-name>	# 本地单个包信息
npm ls <pack-name>		# 上条的简写
```

### 安装、写入依赖

```bash
npm install --dependencies		# 安装生产版本项目的依赖
npm install --devDependencies	# 安装开发版本项目的依赖
```

```bash
# 下载依赖包，并把该包名称添加到package.json文件dependencies键下（默认）
npm install <pack-name> –save
npm install <pack-name> -S
# 下载依赖包，添加到package.json文件devDependencies键下
npm install <pack-name> –save-dev
npm install <pack-name> -D
```

### 其他

#### npm 安装模板失败解决

1. 删除`node_modules`文件夹，使用命令或直接于资源管理器中删除

```bash
rm node_modules -r	# 该命令于win平台时，于git bash中调用
```

2. 清除缓存

```
npm cache clean --force
```
3. 重新安装

```
npm install
```

4. 以上步骤执行完仍报错，删除整个文件夹，重新建一个文件夹执行npm install

## Vue

### Vue Cli3 

#### 新建项目

``` 
vue create [project-name]
```

#### 启动

```
npm run serve
```

#### 图形界面

```
vue ui
```

## ESLint 忽略检查

忽略文件

```js
/* eslint-disable */
alert('foo');
```

忽略部分代码

```js
/* eslint-disable */
alert('foo');
/* eslint-enable */
```

忽略行，有两种写法

```js
alert('foo'); // eslint-disable-line
 
// eslint-disable-next-line
alert('foo');
```

忽略部分代码，限定规则

```js
/* eslint-disable no-alert, no-console */
alert('foo');
console.log('bar');
/* eslint-enable no-alert, no-console */
```

## 其他

### Window设置NODE_ENV(局部)

```
$env:NODE_ENV="production"
```

### 使得网页可编辑

```bash
document.body.contentEditable='true'
```

### 更换 JupterNote 主题

```
jt -t oceans16 -f inputmono -fs 14 -cellw 90% -ofs 12 -dfs 11 -T
```
