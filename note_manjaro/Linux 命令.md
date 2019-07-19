bc      命令行式计算器（quit离开）
ls      显示文件夹（-a 显示所有  -l 显示详细的信息）
cd      切换文件夹
pwd     显示当前的文件夹路径
mkdir   创建一个文件夹
rmdir   删除一个空文件夹
rm -f   删除一个文件夹以及里面的内容
cp      复制文件夹（不同的执行者产生的结果有差异）
mv      移动文件夹或者修改名字
touch   创建一个空白文档
cat     查看文档
tac     查看文档（倒序输出）
nl      查看文档，并显示行数
more    以部分显示的方式查看文档（只支持PgDn查看下一页）
less    以显示一个窗口内容查看文档（支持PgUp和PgDn）
head    查看文档前十行
tail    产看文档后十行
file    查看文件信息(例如文本的编码信息)
gcc(选项)(参数)
-o:指定生成的输出文件
-E:仅执行编译预处理
-S:讲C代码转换为汇编代码
-wall:显示警告信息
-c:仅执行编译操作,不进行链接操作

inxi -Fxz   查看系统详细信息
smartctl -A /dev/sda	查看磁盘使用情况

xfce警告声音设置：
关闭：sudo rmmod pcspkr，开启：sudo modprobe pcspkr
compton 解决画面撕裂问题,替换默认的渲染器,使用compton渲染,使用compton -o 0,使阴影边框消失

#### tee

- 用途说明

在执行Linux命令时，我们可以把输出重定向到文件中，比如 ls >a.txt，

这时我们就不能看到输出了，如果我们既想把输出保存到文件中，又想在屏幕上看到输出内容，就可以使用tee命令了。

tee命令读取标准输入，把这些内容同时输出到标准输出和（多个）文件中，tee命令可以重定向标准输出到多个文件。要注意的是：在使用管道线时，前一个命令的标准错误输出不会被tee读取。

- 使用

```shell
tee # 只输出到标准输出，因为没有指定文件
tee file # 输出到标准输出的同时，保存到文件file中。如果不存在，则创建；存在则覆盖
tee -a file # 输出到标准输出的同时，追加到文件file中。如果文件不存在，则创建。
tee - # 输出到标准输出两次
tee file1 file2 - # 输出到标准两次，同时保存到file1和file2中
```

#### cat

以下`<file>`代表文件名

`cat <file>`: 输出文件内容至标准输出流

`cat > <file>`：创建文件，并从标准输入流中读取输入写至文件中

`cat >> <file>`: 追加文件，并从标准输入流中读取输入写至文件中

一般使用

`cat > <file>`和`cat >> <file>`需要在顶行指明结束符标志，一般为`EOF`，也可以为其他符号。

```shell
[smilejack@smilejack-pc linux_practice]$ cat > test.txt << EOF
> test file
> test file2
> EOF
[smilejack@smilejack-pc linux_practice]$ cat test.txt 
test file
test file2

[smilejack@smilejack-pc linux_practice]$ cat >> test.txt << EOF
> hello world
> hello world2
> EOF
[smilejack@smilejack-pc linux_practice]$ cat test.txt 
test file
test file2
hello world
hello world2
```



在非脚本中

`cat > <file>`和`cat >> <file>`可以使用`Ctrl-D`输出EOF结束符进行结束



区别`cat > <file> <<EOF` 和 `cat > <file> <<-EOF`

第一种表示结束符前面不可以有任何字符，否者不会识别为结束符

第二种表示结束符前面可以有制表符（不能为其他）也能正常结束输入，当然没有制表符也能识别。



#### useradd

添加用户

`useradd <username>`

参数：

- `-m`: 自动创建家目录
- `-g`: 指定用户所在的组，否者会建立一个和用户名同名的组



> 新创建的用户默认没有密码，可以用passwd初始化用户密码



> 如果新创建的用户不能使用tab不全

可能的原因： 	`useradd`用户后默认的`shell`是`sh`，而不是`bash shell`。

解决方法：

方法一：

修改用户默认的`shell`

`chsh -s /bin/bash <username>`

方法二：

修改`useradd`默认的`shell`

`sudo vim /etc/default/useradd`

修改`$SHELL`为

```
$SHELL = /bin/bash
```

可能原因：补全功能没有启动

解决方法：编辑`/etc/bash.bashrc`或者用户家目录的`.bahsrc`,把`bash comlpetion`相关字段修改为：

```shell
#enable bash completion in interactive shells
if ! shopt -oq posix; then
     if [-f  /usr/share/bash-completion/bash_completion ]; then
          . /usr/share/bash-completion/bash_completion
      elif [ -f /etc/bash_completion]; then
           . /etc/bash_completion
      fi
fi
```

最后使用`source <file>`激活文件



> 新建用户后，用户信息会保存在`/etc/passwd`文件中



> 如果新用户需要管理员权限，可以将其加入`/etc/sudoers`文件中

假设用户名为`smilejack`, `sudoer`配置文件如下:

```shell
root	ALL=(ALL:ALL)	ALL
smilejack ALL=(ALL:ALL)	ALL
```





#### userdel

删除用户

`userdel <username>`

参数：

- `-r`: 自动删除家目录





#### passwd

修改或初始用户密码

`passwd [<username>]`

不指定用户名，则修改当前用户密码





#### dirs

显示目录栈

目录栈的栈顶永远存放着当前目录，其中之一改变，另一个跟着改变

- `-p`: 每行显示一条记录
- `-v`: 每行显示一条记录，同时展示该记录在栈中的索引。
- `-c`: 清空目录栈



#### pushd

`pushd`执行最后，默认会执行一个`dirs`命令来显示目录栈的内容。

`pushd`:

将目录栈最顶层的两个目录进行交换，相当于实现了`cd -`功能，返回上次目录改变的地方。



`push <directory>`

切换至该目录，并将该目录置于栈顶。



`push +n`

从栈顶开始，切换到栈中的第n个目录，索引从0开始，然后该元素移至栈顶



`push -n`

从栈底开始，切换到栈中的第n个目录，索引从0开始，然后该元素移至栈顶



#### popd

执行`popd`后，默认会执行`dirs`显示目录栈的内容。

`popd`：

将目录栈顶元素取出，目录切换至新的栈顶元素位置。



`popd +n `:

从栈顶开始，移除栈中的第n个目录，索引从0开始



`popd -n`:

从栈底开始，切换到栈中的第n个目录，索引从0开始，然后该元素移至栈顶。





#### nohup

不挂断地运行命令。

`nohup <command> [<arg>] [&]`

运行<command> 命令 ，<arg>为<command> 的参数，添加`&`表示后台运行

默认将输出到当前目录的`nohup.out`文件中

如何结束`nohup`开启的进程：

`ps -ef | grep <进程名>`:获取进程pid，

`kill -9 <pid>`: 杀死进程



#### netstat

`netstat -nat`：查看端口使用情况



#### update.rc

设置开机脚本



#### sed





#### grep

`grep -v grep`:过滤含有`grep`的行



#### ps

查询进程



#### awk

字符串分割



### ssh

ssh 登入服务器

```shell
ssh user@host
```

防止登入掉线，可以添加`-o ServerAliveInterval=60`,数字可以改

```shell
ssh -o ServerAliveInterval=60 user@host
```

使用代理登入(`-o ProxyCommand="nc -X 5 -x 127.0.0.1:1081 %h %p"`)

```shell
ssh -o ProxyCommand="nc -X 5 -x 127.0.0.1:1081 %h %p" user@host
```

两者都使用

```shell
ssh -o ServerAliveInterval=60 -o ProxyCommand="nc -X 5 -x 127.0.0.1:1081 %h %p" user@host
```



### ssh-keygen

#### 创建一个 SSH key 

```shell
$ ssh-keygen -t rsa -C "your_email@example.com"
```

代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key

```shell
Generating public/private rsa key pair.
# Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```

当然，你也可以不输入文件名，使用默认文件名（推荐），那么就会生成 id_rsa 和 id_rsa.pub 两个秘钥文件。



### ssh-copy-id

ssh-copy-id命令可以把本地主机的公钥复制到远程主机的authorized_keys文件上

ssh-copy-id命令也会给远程主机的用户主目录（home）和~/.ssh, 和~/.ssh/authorized_keys设置合适的权限。  

ssh-copy-id命令可以把本地的ssh公钥文件安装到远程主机对应的账户下。                                                                                                           达到的功能：        

ssh-copy-id - 将你的公共密钥填充到一个远程机器上的authorized_keys文件中。   

使用模式：        `ssh-copy-id [-i [identity_file]] [user@]machine`   

描述：          

​			ssh-copy-id 是一个实用ssh去登陆到远程服务器的脚本（假设使用一个登陆密码，                    因此，密码认证应该被激活直到你已经清理了做了多个身份的使用）。                   

​			它也能够改变远程用户名的权限，~/.ssh和~/.ssh/authorized_keys                    

​			删除群组写的权限（在其它方面，如果远程机上的sshd在它的配置文件中是严格模式的话，这能够阻止你登陆。）。

​			如果这个 “-i”选项已经给出了，然后这个认证文件（默认是~/.ssh/id_rsa.pub）被使用，不管在你的ssh-agent那里是否有任何密钥。

​			另外，命令 “ssh-add -L” 提供任何输出，它使用这个输出优先于身份认证文件。如果给出了参数“-i”选项，或者ssh-add不产生输出，然后它使用身份认证文件的内容。一旦它有一个或者多个指纹， 它使用ssh将这些指纹填充到远程机~/.ssh/authorized_keys文件中。





#### 脚本参数

```
$# 是传给脚本的参数个数
$0 是脚本本身的名字
$1 是传递给该shell脚本的第一个参数
$2 是传递给该shell脚本的第二个参数
$@ 是传给脚本的所有参数的列表
$* 是以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个
$$ 是脚本运行的当前进程ID号
$? 是显示最后命令的退出状态，0表示没有错误，其他表示有错误
$!	Shell最后运行的后台Process的PID
```


### 输入/输出重定向：

大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。

重定向命令列表如下：

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

> 需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

#### 输出重定向

重定向一般通过在命令间插入特定的符号来实现。特别的，这些符号的语法如下所示:

```
command1 > file1
```

上面这个命令执行command1然后将输出的内容存入file1。

注意任何file1内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符。

##### 实例

执行下面的 who 命令，它将命令的完整的输出重定向在用户文件中(users):

```
$ who > users
```

执行后，并没有在终端输出信息，这是因为输出已被从默认的标准输出设备（终端）重定向到指定的文件。

你可以使用 cat 命令查看文件内容：

```
$ cat users
_mbsetupuser console  Oct 31 17:35 
tianqixin    console  Oct 31 17:35 
tianqixin    ttys000  Dec  1 11:33 
```

输出重定向会覆盖文件内容，请看下面的例子：

```
$ echo "菜鸟教程：www.runoob.com" > users
$ cat users
菜鸟教程：www.runoob.com
$
```

如果不希望文件内容被覆盖，可以使用 >> 追加到文件末尾，例如：

```
$ echo "菜鸟教程：www.runoob.com" >> users
$ cat users
菜鸟教程：www.runoob.com
菜鸟教程：www.runoob.com
$
```

------

#### 输入重定向

和输出重定向一样，Unix 命令也可以从文件获取输入，语法为：

```
command1 < file1
```

这样，本来需要从键盘获取输入的命令会转移到文件读取内容。

注意：输出重定向是大于号(>)，输入重定向是小于号(<)。

##### 实例

接着以上实例，我们需要统计 users 文件的行数,执行以下命令：

```
$ wc -l users
       2 users
```

也可以将输入重定向到 users 文件：

```
$  wc -l < users
       2 
```

注意：上面两个例子的结果不同：第一个例子，会输出文件名；第二个不会，因为它仅仅知道从标准输入读取内容。

```
command1 < infile > outfile
```

同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

#### 重定向深入讲解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```
$ command 2 > file
```

如果希望 stderr 追加到 file 文件末尾，可以这样写：

```
$ command 2 >> file
```

**2** 表示标准错误文件(stderr)。

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

```
$ command > file 2>&1

或者

$ command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向，可以这样写：

```
$ command < file1 >file2
```

command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。

------

#### Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

它的基本的形式如下：

```
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

> 注意：
>
> - 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
> - 开始的delimiter前后的空格会被忽略掉。

##### 实例

在命令行中通过 wc -l 命令计算 Here Document 的行数：

```
$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
$
```

我们也可以将 Here Document 用在脚本中，例如：

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

cat << EOF
欢迎来到
菜鸟教程
www.runoob.com
EOF
```

执行以上脚本，输出结果：

```
欢迎来到
菜鸟教程
www.runoob.com
```

------

#### /dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

```
$ command > /dev/null
```

/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 stdout 和 stderr，可以这样写：

```
$ command > /dev/null 2>&1
```

> **注意：**0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。



#### `=~`

判断值是否符合正则表达式,注意后面的正则表达式没有引号

```shell
name=124
if [[ $name =~ ^[0-9]+$]]; then
	echo '$name is a number'
```





### 踩坑记

#### 查询进程pid没有过滤进程本身

```shell
ps -ef | grep '进程特征' | awk '{print $2}'
```

上面会查到的进程包括了自己，实际应该这样

```shell
ps -ef | grep '进程名特征' | grep -v grep | awk '{print $2}'
```

#### `#### , ` ${}`,  `$()`,  <code>&#96; &#96;</code>的区别





#### `'${}'`与`"${}"`的区别

```shell
var=test

echo '$(var)'
$(var)

echo "$(var)"
test
```



#### Shell中的${ }、#、##、%、%%使用范例

**范例**

假设定义了一个变量为，【代码如下】:

file=/dir1/dir2/dir3/my.file.txt

可以用${ }分别替换得到不同的值：

${file#*/}：删掉第一个 / 及其左边的字符串：dir1/dir2/dir3/my.file.txt

${file##*/}：删掉最后一个 /  及其左边的字符串：my.file.txt

${file#*.}：删掉第一个 .  及其左边的字符串：file.txt

${file##*.}：删掉最后一个 .  及其左边的字符串：txt

${file%/*}：删掉最后一个  /  及其右边的字符串：/dir1/dir2/dir3

${file%%/*}：删掉第一个 /  及其右边的字符串：(空值)

${file%.*}：删掉最后一个  .  及其右边的字符串：/dir1/dir2/dir3/my.file

${file%%.*}：删掉第一个  .   及其右边的字符串：/dir1/dir2/dir3/my



【记忆的方法为】：

\# 是 去掉左边（键盘上#在 $ 的左边）

%是去掉右边（键盘上% 在$ 的右边）

单一符号是最小匹配；两个符号是最大匹配

${file:0:5}：提取最左边的 5 个字节：/dir1

${file:5:5}：提取第 5 个字节右边的连续5个字节：/dir2

也可以对变量值里的字符串作替换：

${file/dir/path}：将第一个dir 替换为path：/path1/dir2/dir3/my.file.txt

${file//dir/path}：将全部dir 替换为 path：/path1/path2/path3/my.file.txt

