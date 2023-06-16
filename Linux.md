# Linux

> 参见：[一份前端够用的 Linux 命令](https://juejin.cn/post/7044099175838908424)

## 用户类型

对于一个文件而言，当前用户有以下几种身份

### Owner

文件所有者；当创建一个用户时，Linux 为其创建一个主目录，路径`/home/<username>`，使用`cd ~`快捷进入

### Group

用户组；组内多人具有同样的权限

创建用户时，自动创建一个同名用户组

### Others

非上述范畴的其他用户

### Root

超级管理员，可以访问所有文件

## 添加用户

```bash
# 添加用户
adduser <user-name>
# 修改某用户的密码
passwd <user-name>
```

## `ls`

```bash
ls		# 列出文件和目录
ls -la 	# -a：显示所有文件与目录，包括隐藏；-l：显示详细列表
```

### 文件类型和权限

列出的第一列，为文件的类型与权限，共10位：

+ 1位：`-`：普通文件；`d`：目录文件
+ 2~4位：Owner权限，r：读权限；w：写权限；x：执行权限，无权限为`-`
+ 5~7位：Group权限，同样为`rwx`
+ 8~10位：Others权限，同样为`rwx`

对于root用户：

+ 创建文件夹：默认为`rwxr-xr-x`
+ 创建文件：默认为`rw-r--r--`，默认无执行权限

## `chown`

更改文件属主、属组

```bash
# -R：递归更改文件属组
chown [-R] <owner> <file> # 更改属主
chown [-R] <owner>:<group> <file> #更改属主与属组
```

## `chmod`

更改文件权限

#### 二进制

```
chmod [-R] xyz <file>
```

x、y与z分别为Owner、Group与Others的权限，取值为二进制转化后的十进制：

如`r-x`=`101`=`5`，而`750`=`111 101 000`=`rwxr-x---`

#### `+/-、=`

使用符号加减、赋值权限

```bash
chmod u+x, g-x, o-x example.html		# 新增与移除权限
chmod u=rwx, g=rx, o=r example.html		# 直接赋值
chmod +x example.html 					# 全身份增加执行权限
```

## `echo`

打印输出、写入追加

```bash
echo "\"test content\""				# 打印输出
echo "test content" > index.html	# 创建或覆盖文件内容
echo "test content" >> index.html	# 追加到文件内容
```

## `cat`

查看文件，写入追加

```bash
cat ~/.ssh/id_rsa.pub						# 查看文件内容
cat /dev/null > index.html					# 清空index文件
cat index.html > second.html				# 将index写入到index2中
cat index.html >> second.html				# 将index追加到index2中
cat index.html second.html >> third.html	# 将index与second追加到third中
```

## `mv`

移动与重命名

```bash
mv index.html index2.html	# 文件改名
mv index.html .index.html	# 文件隐藏，文件名上加上.

mv  /home/www/index.html   /home/static/ 			# 文件移动	
mv /home/www/index.html   /home/static/index2.html	# 移动又重命名
```

## `rm`

删除文件或目录

`-f`表示直接删除

`-r`表示目录下的所有文件删除

```bash
rm file		# 删除某文件
rm -r  * 	# 删除当前目录下的所有文件及目录
rm -rf /*	# 删库跑路
```

## 其他

```bash
su <user>		# 切换用户 
whoami			# 显示当前用户
pwd				# 显示当前目录

cd /home/www/	# 切换到指定工作目录
cd ~			# 切换到自己的主目录
cd ../..		# 切换到上上级

mkdir folder			# 创建目录
mkdir -p one/two/three	# 递归创建多级目录

touch new_file	# 创建文件，如果已存在则修改文件的时间属性

# -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
cp –r website/ static	# 将目录 website/ 下的所有文件复制到新目录 static 下：
```





