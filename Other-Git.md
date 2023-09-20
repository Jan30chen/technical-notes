# Other-Git

## 关系图

![关系图](https://cdn.ancii.com/article/image/v1/Qj/zb/RO/ORzjbQVFJasX5aR2DaTg2_Zx5KSH75KdTtVIpx5BAib1lvfYUed4isUGp2pPuUSwC1Oeyb4uYYVr0dil2FldjGj67oep5ktR2qxO4mzH0qk.jpg)

## 常用命令

```bash
# 基础
git init				# 初始化某目录为仓库
git add <file>			# 添加工作区内容到暂存区，可多次使用添加多个文件；使用'.'符号来添加全部已更改

# 文件
git commit -m <message>	# 暂存区对版本库进行一次提交，-m后添加此次提交说明
git status				# 查看当前工作区状态
git diff <file>			# 比较文件在工作区与暂存区的不同

# 查看
git log (-num)				# 查看提交记录（显示最近的num条），按Q退出log加载
git log --abbrev-commit     # 查看，缩写校验值
git log --pretty=oneline	# 查看，每条记录缩略为一行
git log --oneline			# 查看，效果为上两条之和
git log --graph				# 以图表形式查看提交树
git reflog					# 查看命令记录

# 差异（在fetch拉取最新远端后）
git diff <branch-name> <remote-name>/<branch-name> (--stat)	# 比较指定分支，本地与远端间所有文件内容的不同，stat参数只展示文件名
git diff HEAD FETCH_HEAD (--stat)							# 比较当前分支的本地与远端
git log <branch-name1>..<branch-name2>						# 比较分支2比分支1多出的提交差异

# 回退				
git reset --hard					# 强制放弃本地所有【已追踪】文件的修改
git reset HEAD^/HEAD~10/<commit-id>	# 将工作区内容更新至（回退或快进）某个提交版本（分别为上个版本、向上十个版本与具体提交id的版本）
git revert 							# 新建提交，以旧内容覆盖最新的提交，一般用于使远端回退版本（但原先的新提交依然存在）
git rm <file>						# 删除某文件，并放弃对其的追踪
git checkout -- <file>				# 放弃所有工作区修改，回退到暂存区版本（如果有）或版本库最新版本；不能去掉‘--’，否则会变成切换分支
git restore <file>					# 新版本用法，效果同 git checkout --
git restore --staged/-S <file>		# 新版本用法，效果同 git reset HEAD
git clean -df						# 递归当前目录及所有子文件夹，删除本地所有【未追踪】的文件

# 远程库
git remote										# 列出已链接的远程库（别名）
git remote -v 									# 列出远程库与远程地址
git remote add <remote-name> <branch-name>		# 添加远程库，指定名称和地址
git remote rm <remote-name> 					# 移除某个远程库
git remote show <remote-name>					# 查看远端地址、分支，和本地的对应关系
git remote prune <remote-name>					# 本地清除已经失效的远端分支记录

# 分支
git branch												# 查看本地分支
git branch -r											# 查看远程分支
git branch -a											# 查看全部分支
git branch <branch-name>								# 创建分支
git switch <branch-name>								# 转到分支
git switch -c <branch-name>								# 创建并转到分支
git switch -c <branch-name> <remote-name>/<branch-name>	# 创建并转到本地分支，并与远程分支建立映射（远程分支存在的情况下）
git branch -d/-D <branch-name>							# 删除分支，-D为强制删除

# 追踪
git branch -vv								# 查看本地分支与远端分支的追踪情况
git branch -u <origin-branch>				# 使当前分支与指定远端分支建立追踪
git branch --unset-upstream	<branch-name>	# 断开指定分支追踪的远端分支，默认为当前分支

# 合并
git merge <branch-name>			# 将指定分支合并到当前分支
git merge <branch-name> --no-ff	# 合并分支，且禁用Fast forward模式，此法生成新的commit记录
git rebase						# 整理提交记录为变基形式
git rebase <branch-name> 		# 以某分支为基础变基
git rebase --continue			# 上一个命令中，出现合并冲突后，解决冲突并git add后（无需commit），继续变基
git rebase --abort				# 中止变基
git cherry-pick <commit-id>		# 单独应用（其他分支上的）某个提交到当前分支，生成新的提交（原、新提交的commit-id不同，合并时也不视为同一提交）

# 拉取
git clone <remote-address>							# 克隆远程库到本地，--depth=1：只下载单个commit；--single-branch：只下载单个branch
git fetch (<remote-name> <remote-branch>)			# 拉取远程分支到本地。默认取回所有的分支，取回的分支使用<remote-name>/<branch-name>获得
git fetch --prune/-p								# 拉取远程分支前执行git remote prune
git pull 											# 拉取所有远程分支，且合并当前分支
git pull <remote-name> <remote-branch>:<branch>		# 拉取并合并指定远程分支到指定本地分支
git pull --rebase									# 拉取远程分支，并使用变基方式合并本地分支
git diff <branch-name>								# 比较当前分支与指定分支的不同

# 推送
git push									# simple方式，推送当前分支到默认远程库
git push <remote-name> <branch>				# 推送指定分支到指定远程库的对应分支
git push -u	<remote-name> <branch>			# 创建远端分支，并推送当前分支建立联系
git push --all <remote-name>				# 推送全部分支
git push <remote-name> --tags				# 推送当前分支与标签
git push <remote-name> <empty>:<branch>		# 推送空分支到指定远程库分支，实质为删除远端分支
git push <remote-name> --delete <branch> 	# 删除远端分支
git branch --unset-upstream <branch>		# 断开本地分支与已追踪的远端分支的联系
								
# 标签
git tag										# 查看标签列表
git tag <name> (<commit-id>)				# 为某次提交打一个标签（默认为最新提交）
git show <tag-name>							# 查看标签详细信息
git tag -d <tag-name>						# 删除某个标签（不删除提交）
git push <remote-name> <tagname>			# 推送单个标签到远程库
git push --tags								# 推送全部标签到远程库
git push origin --delete <tagname>			# 于远程库删除标签-方法1
git push <remote-name> :refs/tags/<tagname>	# 于远程库删除标签-方法2
git checkout <tag-name>						# 检出标签，会导致仓库进入“分离头指针”状态
git switch -								# 撤销检出标签，返回分支
git checkout -b <new-branch> <tag-name>		# 检出标签为新分支

# 储藏
git stash (push)			# 储存为储藏
git stash save 'message'	# 储存并添加说明信息
git stash -u				# 存储为储藏，包括未跟踪文件
git stash -a				# 存储为储藏，包括全部文件（未跟踪和忽略）
git stash list				# 查看储藏栈
git stash apply (<name>)	# 应用储藏，默认为最新
git stash drop (<name>)		# 删除储藏
git stash pop (<name>)		# 应用并删除储藏
git stash clear 			# 清空所有储藏

# 配置
git config (--global) user.name	<new-name>		# <查看/修改> 当前（全局）用户名
git config (--global) user.email <new-email>	# <查看/修改> 当前（全局）邮箱账户
git config --global http.proxy "127.0.0.1:xxx"	# 更换git代理1
git config --global https.proxy "127.0.0.1:xxx"	# 更换git代理2
git config --global --unset http.proxy			# 恢复默认代理1
git config --global --unset https.proxy			# 恢复默认代理2

# 优化
git gc  # 清理不必要的文件并优化本地存储库
```

## 详细操作

### 远端交互

①**本地有分支**，远端无分支

创建远端分支，并与当前本地分支建立追踪

```bash
git push -u <remote> <remote-branch>
```

②**远端有分支**，本地无分支

创建并转到新分支，再拉取远端分支建立联系

```bash
git switch -c <branch> <remote>/<remote-branch>
```

③本地、远端**都没有分支**

创建新的本地分支，再执行①

```bash
git switch -c <branch>
git push -u <remote> <remote-branch>
```

④本地、远端**都有分支**，但没有追踪

将当前分支与指定远端分支建立追踪关系

```bash
git branch -u <remote>/<remote-branch>
```

### 回退

①**未add时：**工作区回退到和暂存区一致，若暂存区为空则回退至最新版本库

```bash
git checkout <file-name>	# 旧版本
git restore <file-name>		# 新版本
```

②**已add，未commit时：**暂存区回退到和最新版本库一致，暂存区的修改会被放回工作区；如有需要可以再执行上一步回退工作区

```bash
git reset HEAD <file-name>			# 旧版本
git restore --staged/-S <file-name> # 新版本
```

③**已commit时：**回退当前版本到未修改的版本（比如上一个版本），再执行第一步回退工作区

方法1：当前提交id变更，版本回退，之间的版本丢失

+ 使用`reset`回退到指定版本
  + 添加`--hard`，强制回退
  + 添加`--soft`，回退，但保留修改内容
+ 强制推送到远端，因为此时本地分支比远端落后

```bash
git reset <commit-id>
git push -f
```

方法2：只恢复某个指定提交的修改，再生成新的提交id（生成一个新提交，完全抵消某个提交中所涉及修改）

+ 使用`revert`命令撤销某次提交
+ 如果出现冲突，手动处理
+ 提交，生成新的提交记录，推送远端

```bash
# 两者选一，根据提交id或提交标题
git revert -n <commit-id>
git revert -m <commit-name>
```

方法3：当前提交id不变，直接恢复旧版本中的记录（一般用于针对某个文件）

```bash
git checkout <commit-id> <file-name>
```

### 删除

①删除文件

```bash
git rm <file-name>
```

### 比较

①比较分支间的**内容**差异

```bash
git diff <branch1> <branch2>				# 比较内容差异，具体到文件内容
git diff <branch1> <branch2> --stat			# 比较内容差异，只展示有差异的文件名称
git diff <branch1> <branch2> <file-path>	# 比较指定路径文件的内容差异
```

②明确两个分支的提交先后

查看**前者比后者多出的**提交

```bash
git log <branch1> ^<branch2>
```

查看**后者比前者多出的**提交

```bash
git log <branch1>..<branch2>
```

注：可以用于查看本地与远端的提交差异

③未知两个分支的提交先后，单纯比较两者

```bash
git log <branch1>...<branch2>
git log --left-right <branch1>...<branch2>	# 比较两个分支，将分支1的提交显示在左边
```

## 状态

| 状态符 | 状态                                   |
| :----: | -------------------------------------- |
|   A    | 新增的文件                             |
|   C    | 新增的拷贝文件                         |
|   D    | 被删除的文件                           |
|   M    | 内容被修改或mode被修改的文件           |
|   R    | 重命名的文件                           |
|   T    | 类型被重新指定的文件                   |
|   U    | 没有被合并的文件（提交前必须进行合并） |
|   X    | 未知状态的文件（大概率是Git出BUG）     |

## 多SSH

> 参见：https://support.huaweicloud.com/codehub_faq/codehub_faq_0002.html

### 配置

配置`.ssh\config`文件

+ Host：自定义主机名称

+ HostName：网站地址

+ IdentityFile：需要使用的私钥相对路径

+ User：在该网站使用的用户名

```bash
# 设置github项目登录
Host git1
HostName github.com
IdentityFile ~/.ssh/id_ed12345
User user@123.com
```

### 使用

使用时替换为如下格式：

```
git clone git@<host>:<url>
```

例子：将网址上复制的`ssh`地址替换网址部分（github.com）为自定义主机名

```bash
# 原
git@github.com:vuejs/vue-next.git
# 现
git@git1:vuejs/vue-next.git
```

## 设置短命令

> 参考：https://juejin.cn/post/7071780876501123085#heading-29

## 规范提交

> 参考：[约定式提交 1.0.0-beta.4](https://www.conventionalcommits.org/zh-hans/v1.0.0-beta.4/)、[git commit 提交规范](https://zhuanlan.zhihu.com/p/90281637)

提交结构，以空行分隔：

+ 标题（必填）：
  + type：提交类型，可以后续`!`，表示注意`Breaking Changes`说明
  + scope：作用域，进一步描述提交的效果
  + subject：详细说明
+ 主体-body：主体内容
+ 页脚-footer：放`Breaking Changes`或`Closed Issues #<issue num>`

> `Breaking Changes`：表示引入了破坏性API变更，对应语义化版本的`MAJOR`

```
<type>(<scope>): <subject>

<body>

<footer>
```

### type

commit 的类型：

+ **feat**：新功能，对应语义化版本的`MINOR`
+ **fix**：缺陷修复，对应语义化版本的`PATCH`
+ perf：优化代码性能
+ refactor：重构
+ docs：文档修改
+ style：代码格式修改
+ test：新增或修改测试用例
+ build：项目构建项或依赖项修改
+ revert：恢复上一次提交
+ ci：持续集成相关文件修改
+ chore：其他修改
+ release：发布新版本
+ workflow：工作流相关文件修改

## 常见问题

### would clobber existing tag

不能同步更新：`would clobber existing tag`

```bash
git fetch --tags -f	# 强制刷新本地标签
```

---

error: You have not concluded your merge (MERGE_HEAD exists).

https://www.cnblogs.com/whowhere/p/9334793.html

### cannot lock ref 'refs/heads/a/b'

推送并建立远端分支失败：cannot lock ref 'refs/heads/a/b': 'refs/heads/a' exists; cannot create 'refs/heads/a/b'

在远端已经存在`a`分支的情况下，不能建立名为`a/b`形式的分支

### OpenSSL SSL_read: Connection was reset, errno 10054

> 参见：[Git 错误：OpenSSL SSL_read Connection was reset, errno 10054](https://zhuanlan.zhihu.com/p/499986340)

解除SSL认证

```text
git config --global http.sslVerify "false"
```

### stash指定文件

> 参见：[git stash 用法总结和注意点](https://www.cnblogs.com/zndxall/p/9586088.html)

1. `add`不需要`stash`的文件进入`stage`区：`git add a.file`
2. 运行`git stash [-k|--keep-index]`，只会`stash`工作区的文件
3. 使用`git reset`将`stage`区还原至工作区