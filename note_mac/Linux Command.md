### Linux Command

#### `curl`

`curl <url>`

用于多种协议的下载和上传

参数：

`-b <str=value>`:使用cookie，

`-C [number|-]`： 断点续传,从number bytes开始下载，需下载的网址服务器支持断点续传，`-`表示根据保存的文件位置自动设置续传点从而开始下载。

`-r <[start]-[end]>` : 分段下载

额外技巧：

`curl ifconig.me/ip`:获取IP

`curl wttr.in/<city-name>`: 获取天气