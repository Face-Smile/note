#### Ubuntu 忽略软件包更新

```shell
apt-mark hold <package-name>
```

或者：

```shell
echo "package-name hold" | dpkg --set-selections
```

或者：

```shell
aptitude unhold package_name
```





查看软件包状态：

```shell
dpkg --get-selections
```





#### Ubuntu-server-18.04 设置开机启动脚本

##### 编辑/lib/systemd/system/rc-local.service

一般正常的启动文件主要分成三部分

```
[Unit]段：启动顺序与依赖关系
[Service]段：启动行为，如何启动，启动类型
[Install]段：定义如何安装这个配置文件，即怎样做到开机启动
```

可以看出，/etc/rc.local 的启动顺序是在网络后面，但是显然它少了 Install 段，也就没有定义如何做到开机启动，所以显然这样配置是无效的。 因此我们就需要在后面帮他加上 [Install] 段:

```shell
[Install]  
WantedBy=multi-user.target  
Alias=rc-local.service
```

##### 创建或者编辑/etc/rc.local 文件

rc.local脚本是一个ubuntu开机后会自动执行的脚本，我们可以在该脚本内添加命令行指令。该脚本位于/etc/路径下，需要root权限才能修改。

脚本格式：

```shell
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
  
exit 0
```

**注意:** 一定要将命令添加在 exit 0之前； 如果该文件没有可执行权限，使用`chmod +x /etc/rc.local `添加可执行权限




