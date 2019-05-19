## 工作流程

`![img](http://www.runoob.com/wp-content/uploads/2015/02/git-process.png)`


![img](/home/smilejack/Documents/note/git-process.png)

## 基本概念

### 工作区

本地电脑能查看的目录

### 暂存区

英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。

### 版本库

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

原图地址：`http://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg`

![img](/home/smilejack/Documents/note/1352126739_7909.jpg)

图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage, index），标记为 "master" 的是 master 分支所代表的目录树。

图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。

图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。

当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。

当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。

当执行 "git reset HEAD" 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。

当执行 "git rm --cached <file>" 命令时，会直接从暂存区删除文件，工作区则不做出改变。

当执行 "git checkout ." 或者 "git checkout -- <file>" 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。

当执行 "git checkout HEAD ." 或者 "git checkout HEAD <file>" 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。



## 基本操作

### 环境配置

#### 用户信息配置

##### 用户信息显示

`git config user.name`: 显示用户名

`git config user.email`: 显示用户邮箱
##### 用户信息设置

###### 全局信息配置

`git config --global user.name "YOUR_NAME"`： 设置用户名
`git config --global user.email "YOUT_EMAIL"`： 设置用户邮箱



### 创建项目

`git init`

在文件所在目录中创建新的git 仓库或者重新初始化一个已存在的仓库。 你可以在任何时候、任何目录创建，完全本地化。

```shell
$ mkdir warehouse
$ cd warehouse
$ git init
Initialized empty Git repository in ~/warehouse/.git/
```

或者

```shell
git init warehouse
```



### 拷贝项目

使用`git clone`拷贝一个Git仓库到本地，让自己能够查看该项目，或者进行修改。

```shell
# 克隆仓库到当前目录
git clone <repo>
# 克隆仓库到指定目录
git clone <repo> <directory>
```



```shell
# 几种效果等价的git clone写法：
git clone http://github.com/CosmosHua/locate new
git clone http://github.com/CosmosHua/locate.git new
git clone git://github.com/CosmosHua/locate new
git clone git://github.com/CosmosHua/locate.git new
```

```shell
# git clone 时，可以所用不同的协议，包括 ssh, git, https 等，其中最常用的是 ssh，因为速度较快，还可以配置公钥免输入密码。各种写法如下：
git clone git@github.com:fsliurujie/test.git         --SSH协议
git clone git://github.com/fsliurujie/test.git          --GIT协议
git clone https://github.com/fsliurujie/test.git      --HTTPS协议
```



### 跟踪新文件（添加文件到缓存区）

`git add`

`git add` 命令将文件添加到缓存区

```SHELL
$ touch README
$ touch hello.php
$ git status -s
?? REDAME
?? hello.php
$ git add README hello.php
$ git status -s
A README
A hello.php
```



### 忽略文件

创建一个`.gitignore`文件，写入不需要跟踪的文件

文件位置： 仓库根目录下

example:

```
*.[oa]		# 忽略所有以 .o 或 .a 结尾的文件。
*~			#忽略所有以波浪符（~）结尾的文件
```

文件 `.gitignore` 的格式规范如下：

- 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（`*`）匹配零个或多个任意字符；`[abc]`匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）。 使用两个星号（`*`) 表示匹配任意中间目录，比如 `a/**/z` 可以匹配 `a/z` , `a/b/z` 或 `a/b/c/z` 等。



### 检查当前文件状态

`git status`

`-s`参数：获得简短的结果输出

新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。 你可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 M 表示该文件被修改了并放入了暂存区。



### 查看已写入缓存区与已修改但未写入缓存区的改动区别

`git diff`

执行`git diff`来查看执行`git status`的结果的详细信息

- 尚未缓存的改动： `git diff`
- 查看已缓存的改动：`git diff --cached` （Git 1.6.1 及更高版本还允许使用 `git diff --staged`，效果是相同的，但更好记些。）
- 查看已缓存的与未缓存的所有改动：`git diff HEAD`
- 显示摘要而非整个`diff`： `git diff --stat`



### 将暂存区（缓存区）内容添加到仓库（对象库）

`git commit`：添加所有缓存区文件至仓库

`git commit <file>`: 添加指定文件至仓库

执行 git commit 将缓存区内容添加到仓库中

> 使用此命令之前需要完善用户信息（github用户名和邮箱）

参数：

`-m  <comment>`: 添加提交日志

`-a`: 跳过`git add` 提交流程



### 取消已缓存的内容

`git reset HEAD <file>`：取消所有或者指定的文件

`My understanding`：将版本库中所有或指定文件覆盖缓存区的指定或所有文件





## git 仓库克隆

`git clone <url>`

参数：

`--depth=<number>`: 用于指定克隆深度，为1表示只克隆最近一次的commit。


