## pip

### pip镜像

经常在使用Python的时候需要安装各种模块，而pip是很强大的模块安装工具，但是由于国外官方pypi经常被墙，导致不可用，所以我们最好是将自己使用的pip源更换一下，这样就能解决被墙导致的装不上库的烦恼。
网上有很多可用的源，例如：

- 阿里: `http://mirrors.aliyun.com/pypi/simple/`
- 豆瓣: `http://pypi.douban.com/simple/`
- 清华: `https://pypi.tuna.tsinghua.edu.cn/simple`

#### 临时使用

可以在使用pip的时候加参数`-i https://pypi.tuna.tsinghua.edu.cn/simple`
例如：`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple gevent`，这样就会从清华这边的镜像去安装gevent库。

#### 永久修改

- Linux下
  修改 `~/.pip/pip.conf` (没有就创建一个)，添加或修改，内容如下【参考
  [阿里help](https://link.jianshu.com/?t=http://mirrors.aliyun.com/help/pypi)】：

```csharp
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

- windows下
  直接在user目录中创建一个pip目录，如：C:\Users\xx\pip，新建文件pip.ini，内容如下

```shell
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

### 导出与安装

```shell
pip freeze > requirements.txt
pip install -r requirements.txt
```