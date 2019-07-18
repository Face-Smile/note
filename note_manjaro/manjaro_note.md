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