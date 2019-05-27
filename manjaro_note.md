##### 更新国内源

`sudo pacman-mirrors -i -c China -m rank`

`sudo pacman -Syy`

##### 刷新字体缓存

在字体所在目录，终端输入`fc-cache -fv`

##### 查询域名IP地址

访问网址：`ipaddress.com`查询

##### 刷新dns缓存

Windows: 
`ipconfig /flushdns`

Debian/Ubuntu: 
`sudo /etc/rc.d/init.d/nscd restart`

Linux with systemd: 
`sudo systemctl restart network.service`

Fedora Linux: 
`sudo systemctl restart NetworkManager.service`

Arch Linux/Manjaro with Network Manager: 
`sudo systemctl restart NetworkManager.service`

Arch Linux/Manjaro with Wicd: 
`sudo systemctl restart wicd.service`

Mac OS X: 
`sudo dscacheutil -flushcache;sudo killall -HUP mDNSResponder`



##### 控制系统风扇的转速

- 机型： ThinkPad x200(其他有不同，未测试)

- 安装要求：安装thinkfan（github地址：https://github.com/vmatare/thinkfan）

`yaourt thinkfan`

- 配置(https://wiki.archlinux.org/index.php/Fan_speed_control#Installation_3):

The embedded controller (EC) regulates fan speed. However, in order to take control over it, add `fan_control=1` to your [kernel parameters](https://wiki.archlinux.org/index.php/Kernel_parameters).

Current fan control daemons available in the [AUR](https://wiki.archlinux.org/index.php/AUR) are [simpfand-git](https://aur.archlinux.org/packages/simpfand-git/)AUR and [thinkfan](https://aur.archlinux.org/packages/thinkfan/)AUR (recommended).

- Installation

Install [thinkfan](https://aur.archlinux.org/packages/thinkfan/)AUR or [thinkfan-git](https://aur.archlinux.org/packages/thinkfan-git/)AUR. Optionally, but recommended, install [lm_sensors](https://www.archlinux.org/packages/?name=lm_sensors). Then have a look at the files:

```
# pacman -Ql thinkfan
```

Note that the thinkfan package installs `/usr/lib/modprobe.d/thinkpad_acpi.conf`, which contains

```
options thinkpad_acpi fan_control=1
```

So fan control is enabled by default. Alternatively, you can enable fan control as follows:

```
$ echo "options thinkpad_acpi fan_control=1" > /etc/modprobe.d/thinkfan.conf
```

Now, load the module:

```
$ su
# modprobe thinkpad_acpi
# cat /proc/acpi/ibm/fan
```

You should see that the fan level is "auto" by default, but you can echo a level command to the same file to control the fan speed manually. The thinkfan daemon will do this automatically.

Set thinkfan to run at startup by editing `/etc/default/thinkfan` and adding the following line:

```
START=yes
```

Finally, enable the thinkfan systemd service:

```
$ systemctl enable thinkfan
```

To configure the temperature thresholds, you will need to copy one of the example config files (e.g. `/usr/share/doc/thinkfan/examples/thinkfan.conf.simple`) to `/etc/thinkfan.conf`, and modify to taste. This file specifies which sensors to read, and which interface to use to control the fan. Some systems have `/proc/acpi/ibm/fan` available; on others, you will need to specify something like:

```
hwmon /sys/devices/virtual/thermal/thermal_zone0/temp
```

to use generic hwmon sensors instead of thinkpad-specific ones.

- Running

You can test your configuration first by running thinkfan manually (as root):

```
# thinkfan -n
```

and see how it reacts to the load level of whatever other programs you have running.

When you have it configured correctly, [start/enable](https://wiki.archlinux.org/index.php/Start/enable) `thinkfan.service`.

- 配置文件解析

（level, low, high)

level: 风扇转速等级（0-7）

low：风扇等级最低适应温度

high：风扇等级最高适应温度



##### vim配置

为了vim更好的支持python写代码,修改tab默认4个空格有两种设置方法：

1. vim /etc/vimrc  

|	|	|
| ---- | ------------ |
| `1`  | `set` `ts=4` |
| `2`  | `set` `sw=4` |

2. vim /etc/vimrc 
| | |
| ---- | ----------------- |
| `1`  | `set` `ts=4` 	|
| `2`  | `set` `expandtab` |
| `3`  | `set` `autoindent` |

推荐使用第二种，按tab键时产生的是4个空格，这种方式具有最好的兼容性。

 

**在 Vim 中设置 Tab**

 

缩进用 tab 制表符还是空格，这不是个问题，就像 python 用四个空格来缩进一样，这是要看个人喜好的。在 Vim 中可以很方便的根据不同的文件类型来设置使用 tab 制表符或者空格，还可以设置长度，非常灵活。

首先来看如何设定 tab 的宽度以及如何确定用 tab 制表符还是空格来表示一个缩进：

````shell
set tabstop=4
set softtabstop=4
set shiftwidth=4
set noexpandtab / expandtab
````



**说明：**

 

其中 `tabstop` 表示一个 tab 显示出来是多少个空格的长度，默认 8。

`softtabstop` 表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用 `expandtab` 时特别有用。

`shiftwidth` 表示每一级缩进的长度，一般设置成跟 `softtabstop` 一样。

当设置成 `expandtab` 时，缩进用空格来表示，`noexpandtab` 则是用制表符表示一个缩进。



##### [关于在vim中的查找和替换](https://www.cnblogs.com/huxinga/p/7942194.html)



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

 

 

 

###### 大小写敏感查找

在查找模式中加入`\c`表示大小写不敏感查找，`\C`表示大小写敏感查找。例如：

```
/foo\c
```

将会查找所有的`"foo"`,`"FOO"`,`"Foo"`等字符串。



##### 终端无法补全

安装脚本 `bash-completion`

在`.bash_profile`或者`.bashrc`添加一下语句：

```shell
if [-r /usr/share/bash-completion/bash_completion ]; then
	. /usr/share/bash-completion/bash_completion
fi
```

或者

```shell
[ -r /usr/share/bash-completion/bash_completion   ] && . /usr/share/bash-completion/bash_completion
```



##### linux系统root用户可强制踢制其它登录用户

首先可用`w`命令查看登录用户信息，显示信息如下：

```shell
[root@admin ~]# w
 14:04:56 up  4:18,  1 user,  load average: 0.00, 0.01, 0.06
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.72.1     14:04    0.00s  0.00s  0.00s w
```


强制踢人命令格式：`pkill -kill -t tty`

解释：

`pkill -kill -t 　踢人命令`

`tty`:　所踢用户的TTY