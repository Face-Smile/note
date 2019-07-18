Adb 工具



adb 

adb shell 进入安卓shell

adb shell am start 包名/MainActivity  启动APP

adb shell ps | grep <app包名> 查看app是否运行

adb shell am force-stop <包名> 杀死进程，强制停止APP进程，不会清除APP进程在系统产生的数据

adb shell pm clear <package name> 杀死进程，并清除APP进程所产生的所有数据

adb shell pm

adb devices 查看连接设备

adb start-server 启动服务器

adb kill-server 	停止服务器

adb -s 指定设备进行操作

adb connect  <device name> 连接设备

adb push LOCAL… REMOTE	将本地文件复制到设备

adb pull REMOTE…LOCAL	将设备文件复制到本地

adb install <file.apk> 安装APP

adb uninstall  package 卸载APP

adb shell pm list package 查看手机所有应用

adb shell pm list package -s 查看手机上的系统应用

adb shell pm list package -3 查看手机第三方的应用

adb shell pm list package -3 -f 查看手机上的包的路径

adb shell pm install/uninstall 安装/卸载存在设备上的包名



adb shell input text <text> 输入文本

adb shell input keyevnet <KEYCODE> 模拟按键操作

adb shell input tap x y	模拟点击

adb shell input swipe x0 y0 x1 y1 time 模拟滑动屏幕，time表示完成滑动需要多少时间





adb reboot 重启手机

adb shell reboot -p 关机

MuMu 浏览器无法连接android问题

adb kill-server &&  adb server







检查android os 是否开机，进入桌面

adb shell getprop init.svc.bootanim

​	返回running：表示正在初始化

​	返回stopped：表示初始化完成



检查android OS 是否进入桌面

adb shell getprop sys.boot_completed

​	返回空：表示还未进入

​	返回1：表示已进入桌面



aapt 工具

aapt dump badging <file.apk>

查看安装包的详细信息，主要是查看包名和MainActivity（一般第一行可以找到包名（一个键为name 的键值对值），含有launchable-activity的那一行可以找到MainActivity（一个键为name 的键值对的值）

