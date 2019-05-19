#### 创建项目

​		**requires project**: no

​		**command line**: scrapy startproject *myproject_name* *[project_dir]*

#### scrapy.cfg项目配置文件

- /etc/scrapy.cfg
- ~/.config/scrapy.cfg
- scrapy.cfg

按照上述文件scrapy.py进行继承覆盖配置属性

##### genspider 创建爬虫

​		**syntax**: scrapy genspider [-t template <name> <domain>]

​		**requires project**: no		

> 在当前文件路径或者当前项目的spiders文件下创建一个新的spider(爬虫)，<name> 参数为spider's 名字，<domain> 用于生成 `allowed_domains` 和 `start_urls`爬虫属性。

```shell
$ scrapy genspider -l
Available templates:
  basic
  crawl
  csvfeed
  xmlfeed

$ scrapy genspider example example.com
Created spider 'example' using template 'basic'

$ scrapy genspider -t crawl scrapyorg scrapy.org
Created spider 'scrapyorg' using template 'crawl'
```

> 注意：这不是唯一创建爬虫的方式，你也可以自己创建爬虫源码在spiders目录下



#### crawl 开启爬虫抓取

​		**Syntax**: `scrapy crawl <spider>`

​		**requires project**: yes



#### check 运行爬虫项目检查（run contract checks)

​		**Syntax**: `scrapy check [-l]  <spider>`

​		**requires project**: yes

