# Other-npm

## 命令

### 源

```bash
npm install <pack> --registry=<url>		# 临时更换下载源
npm install -g cnpm --registry=<url>	# 新增下载源（cnpm）
npm config set registry <url>			# 更换原有下载源
npm config get registry 				# 查看当前下载源
```

> 默认下载源：https://registry.npmjs.org/
>
> 淘宝镜像源：https://registry.npm.taobao.org

### 安装

```bash
npm install/i <pack-name>	# 安装某个包
npm i -g <pack-name>		# 全局安装
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
npm show <pack-name>						# 包最新线上版本信息
npm view <pack-name>						# 包信息
npm view <pack-name> version/versions		# 查看指定包最新版本/全部版本
npm list/ls (-D=0) (-g)	(<pack-name>)		# 本地全部包信息(指定深度、全局、包名)
npm config get userconfig					# 查看本地npm配置
```

### 安装并写入

```bash
npm install --dependencies		# 安装生产版本项目的依赖
npm install --devDependencies	# 安装开发版本项目的依赖
```

```bash
# 下载依赖包，并把该包名称添加到package.json文件dependencies键下（默认）
npm install <pack-name> -–save
npm install <pack-name> -S
# 下载依赖包，添加到package.json文件devDependencies键下
npm install <pack-name> -–save-dev
npm install <pack-name> -D
```

### 移除

```bash
npm uninstall <pack-name>
```

## 问题

### **npm 安装失败**

删除`node_modules`文件夹

方法1：直接于资源管理器中删除

方法2：使用命令

```bash
rm node_modules -r	# 该命令于win平台时，于git bash中调用
```

清除缓存

```
npm cache clean --force
```

重新安装

```
npm install
```

以上步骤执行完仍报错，删除整个文件夹，重新建一个文件夹执行npm install

---

报错：`Unsupported platform for fsevents@2.3.2`

`fsevents`依赖只能在macOS下安装，其他平台会跳过，如果没有跳过导致报错；使用`-f`强制安装

```bash
npm i -f
```

### Unable to resolve dependency tree

> 参见：https://bbs.huaweicloud.com/blogs/349716

这是个npm v7版本及以上才会出现的问题

```bash
npm i --legacy-peer-deps
```

> legacy/ˈleɡəsi/:	adj.（软件或硬件）已过时但因使用范围广而难以替代的
>
> peer-deps:	peerDependencies属性（对等依赖关系）

## 包

### npx

npm 5.2版本之后都自带 npx，有如下三种用法：

#### **临时执行**

普通情况下，非全局安装的包，直接本地执行会报错；npx 可以解决这个问题，当我们键入如下命令时：

```bash
$ npx <command-name>
```

npx 依次尝试：

1. 在`node_modules/.bin`路径下寻找该命令，即本地路径
2. 在环境变量`$PATH`中寻找该命令，即全局路径
3. 自动安装该命令的依赖，然后执行，执行完成后再删除该依赖，不污染全局依赖项

可附加的参数：

+ `-p`：指定安装的依赖
+ `--no-install`：强制使用本地依赖，不存在则报错
+  `--ignore-existing`：强制忽略本地依赖，直接远程安装并执行

#### 指定特定版本执行

npx 还可以指定某个特定版本，来运行某脚本

```bash
$ npx node@0.12.8 -v
```

下载 0.12 版本的 npm，然后调用`npm -v`来获取版本号

#### 远程源码执行

```bash
# 执行 Gist 代码
$ npx https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32
# 执行仓库代码
$ npx github:piuccio/cowsay hellos
```

### nvm

用于切换不同版本的 npm，使用前请先把<u>已全局安装的 node 卸载</u>

```bash
$ nvm ls available	# 列出可用node版本
$ nvm install 4.2.2	# 安装node，指定版本4.2.2
$ nvm install 4.2	# 安装4.2.x范围内的最新版本
$ nvm install lts	# 安装最新版本

$ nvm ls		# 列出已安装版本
$ nvm use 4.2.2	# 切换版本
```

### nrm

用于切换npm下载源

```bash
$ nrm ls					# 列出已有的源
$ nrm use <registry>		# 切换到指定源
$ nrm add <registry> <url>	# 添加新源
$ nrm del <registry>		# 删除指定源
$ nrm test <registry>		# 测试指定源的速度
```

### 其他

+ [http-serve](https://www.npmjs.com/package/http-server)：本地模拟服务器环境，可见[http-server 使用详解](https://www.ifrontend.net/2022/07/http-server/)

  