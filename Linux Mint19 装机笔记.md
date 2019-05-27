#### 一、安装系统（U盘安装）

##### （1）准备工作

- 分出至少20G的磁盘空间分区
- 系统镜像文件
- U盘（大于4G）
- U盘系统启动盘制作工具（Ubuntu系列可以使用deepin-boot-make）
##### （2）制作U盘启动盘（制作过程U盘会被格式化）
- 插入U盘
- 运行U盘系统盘制作工具
- 选择需要安装的系统的镜像文件
- 选择要制作的盘
- 开始制作等待完成（5~6分钟）
##### （3）准备安装



#### 二、系统优化

##### 开机自动挂载磁盘

- （1）创建挂载路径（就是把磁盘分配一个路径，这个路径可以分到任何目录）
一般都放在`/media`目录下
- （2）获取需要挂鞋的分区表示
打开文件管理器
查看需要挂载磁盘的分区表示（比如本地磁盘后面的标识是`/dev/sda4`)
- （3）获取分区类型
终端输入`sudo blkid`，查看要挂载分区的`type`
- （4）编辑`/etc/fstab`文件
终端输入`sudo gedit /etc/fstab`打开`fstab`文件（如果没有gedit可以安装或者使用其他编辑器，但需要管理员权限）
在末行后加上 分区标识 挂载点 分区type defaults 0 0
例如：`/dev/sda1 /media/C ntfs defaults 0 0`
保存退出
重启后会发现自动挂载所需要的磁盘了



##### github下载速度慢解决方案
`https://www.ipaddress.com/` 使用 IP Lookup 工具获得下面这两个github域名的ip地址，该网站可能需要梯子，输入上述域名后，分别获得github.com和`github.global.ssl.fastly.net`对应的ip，比如192.30.xx.xx和151.101.xx.xx。准备工作做完之后，打开的hosts文件中添加如下格式，IP修改为自己查询到的IP：

`192.30.xx.xx github.com`
`151.101.xx.xx github.global.ssl.fastly.net`

windows:

最后执行`ipconfig /flushdns`命令，刷新 DNS 缓存。修改后的下载速度能达到 200KB/S 以上。







