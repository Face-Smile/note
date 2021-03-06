### vim配置

为了vim更好的支持python写代码,修改tab默认4个空格有两种设置方法：

1. vim /etc/vimrc  

|      |              |
| ---- | ------------ |
| `1`  | `set` `ts=4` |
| `2`  | `set` `sw=4` |

2. vim /etc/vimrc 

|      |                    |
| ---- | ------------------ |
| `1`  | `set` `ts=4`       |
| `2`  | `set` `expandtab`  |
| `3`  | `set` `autoindent` |

推荐使用第二种，按tab键时产生的是4个空格，这种方式具有最好的兼容性。

 

**在 Vim 中设置 Tab**

 

缩进用 tab 制表符还是空格，这不是个问题，就像 python 用四个空格来缩进一样，这是要看个人喜好的。在 Vim 中可以很方便的根据不同的文件类型来设置使用 tab 制表符或者空格，还可以设置长度，非常灵活。

首先来看如何设定 tab 的宽度以及如何确定用 tab 制表符还是空格来表示一个缩进：

```shell
set tabstop=4
set softtabstop=4
set shiftwidth=4
set noexpandtab / expandtab
```



**说明：**

 

其中 `tabstop` 表示一个 tab 显示出来是多少个空格的长度，默认 8。

`softtabstop` 表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用 `expandtab` 时特别有用。

`shiftwidth` 表示每一级缩进的长度，一般设置成跟 `softtabstop` 一样。

当设置成 `expandtab` 时，缩进用空格来表示，`noexpandtab` 则是用制表符表示一个缩进。



#### [关于在vim中的查找和替换](https://www.cnblogs.com/huxinga/p/7942194.html)



**1，查找**

在normal模式下按下`/`即可进入查找模式，输入要查找的字符串并按下回车。 Vim会跳转到第一个匹配。按下`n`查找下一个，按下`N`查找上一个。

Vim查找支持正则表达式，例如`/vim$`匹配行尾的`"vim"`。 需要查找特殊字符需要转义，例如`/vim\$`匹配`"vim$"`。

**2，大小写敏感查找**

在查找模式中加入`\c`表示大小写不敏感查找，`\C`表示大小写敏感查找。例如：

```
/foo\c
```

将会查找所有的`"foo"`,`"FOO"`,`"Foo"`等字符串。

**3，大小写敏感配置**

Vim 默认采用大小写敏感的查找，为了方便我们常常将其配置为大小写不敏感：

```
" 设置默认进行大小写不敏感查找
set ignorecase
" 如果有一个大写字母，则切换到大小写敏感查找
set smartcase 
```

> 将上述设置粘贴到你的`~/.vimrc`，重新打开Vim即可生效

**4，查找当前单词**

在normal模式下按下`*`即可查找光标所在单词（word）， 要求每次出现的前后为空白字符或标点符号。例如当前为`foo`， 可以匹配`foo bar`中的`foo`，但不可匹配`foobar`中的`foo`。 这在查找函数名、变量名时非常有用。

按下`g*`即可查找光标所在单词的字符序列，每次出现前后字符无要求。 即`foo bar`和`foobar`中的`foo`均可被匹配到。

**5，查找与替换**

`:s`（substitute）命令用来查找和替换字符串。语法如下：

```
:{作用范围}s/{目标}/{替换}/{替换标志}
```

例如`:%s/foo/bar/g`会在全局范围(`%`)查找`foo`并替换为`bar`，所有出现都会被替换（`g`）

**6，作用范围**

作用范围分为当前行、全文、选区等等。

当前行：

```
:s/foo/bar/g
```

全文：

```
:%s/foo/bar/g
```

选区，在Visual模式下选择区域后输入`:`，Vim即可自动补全为 `:'<,'>`。

```
:'<,'>s/foo/bar/g
```

2-11行：

```
:5,12s/foo/bar/g
```

当前行`.`与接下来两行`+2`：

```
:.,+2s/foo/bar/g
```

###### 替换标志

上文中命令结尾的`g`即是替换标志之一，表示全局`global`替换（即替换目标的所有出现）。 还有很多其他有用的替换标志：

空替换标志表示只替换从光标位置开始，目标的第一次出现：

```
:%s/foo/bar
```

`i`表示大小写不敏感查找，`I`表示大小写敏感：

```
:%s/foo/bar/i
# 等效于模式中的\c（不敏感）或\C（敏感）
:%s/foo\c/bar
```

`c`表示需要确认，例如全局查找`"foo"`替换为`"bar"`并且需要确认：

```
:%s/foo/bar/gc
```

回车后Vim会将光标移动到每一次`"foo"`出现的位置，并提示

```
replace with bar (y/n/a/q/l/^E/^Y)?
```

按下`y`表示替换，`n`表示不替换，`a`表示替换所有，`q`表示退出查找模式， `l`表示替换当前位置并退出。`^E`与`^Y`是光标移动快捷键，参考： [Vim中如何快速进行光标移](http://harttle.com/2015/11/07/vim-cursor.html)



######  大小写敏感查找

在查找模式中加入`\c`表示大小写不敏感查找，`\C`表示大小写敏感查找。例如：

```
/foo\c
```

将会查找所有的`"foo"`,`"FOO"`,`"Foo"`等字符串。



### Vim 跳转

#### 跳转到指定行

1. ##### `ngg`或者`nG`

2. ##### `:n`

3. ##### `vim +n <filename>`

> 其中n代表要跳转的行数

区别: `ngg/nG`输入以后无需回车,`:n`需要回车才能跳转,而`vim +n <filename>`是在打开文件的时候跳转.

实例:(跳转到12行)

```
12gg
12G
:12
vim +12 filename
```

#### 跳转到文件的开始

`gg`

#### 跳转到文件的结束

`G`

#### 跳转到行首

`^`

#### 跳转到第一个字符

`0`   # 数字

#### 跳转到行尾

`$`

### 行号显示与关闭

#### 显示行号

`set number`或者`set nu`

#### 关闭显示行号

`set nonumber`





## vim 补全

在编辑模式下使用 `Ctrl + N` 键, 即可使用







## vim 多行插入

方式一:

按`v`键进入可视模式, 方向键选择你要插入的行

按下`:`, 输入`normal i 你要插入的字符串`即可多行插入

方式二:

按`ctrl` + `v`进入可视块模式, 按大写的`i`进入插入模式

输入要插入的字符串,完成后按`esc`即可







## vim 多行缩进

按`v`进入可视模式,选择你要缩进的行

按`<`向左缩进,按`>`向右缩进.