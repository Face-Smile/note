# lxml 模块 

## 1. `etree` -- 网页解析

#### 1.1 使用xpath解析网页

```python
from lxml import etree
# doc = lxml.html.fromstring(html=req.text, parser=lxml.html.HTMLParser())
doc = etree.HTML(req.text)
doc.xpath('//div[contains(@class,"img")]/img/@src')
```

```
['http://imglf4.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0',
 'http://imglf5.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTTE50aTRuVUlGZHBvWm1TOUVBZ3FhVXVTMEwrZktMZEFnPT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0',
 'http://imglf3.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRXpmM0IrNFdmWVJvL2pGVGFrRUxZcElqYnR0QWRNRjh3PT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0',
 'http://imglf3.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTTW9aWUF0SElpNzVzWHJEdTFWbjNpZHlDSzZTaWhxcGFnPT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0',
 'http://imglf6.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTTnVxKzNWUlpJei9uYWo3ajZwSE9vc01nSGJwWjhWRTdnPT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0',
 'http://imglf6.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTTGJrQXJsamg5VXBlbkdpNzF5d3ZzTnVoQkxBQjJUS3N3PT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0']
```













# urllib 模块 

## 1. `parse` -- 解析url

### 1.1 `unquote`,`unquote_plus` -- 解码url

`unquote` 和 `unquote_plus`的区别为 `unquote_plus`将`+`解码为空格` `，`unquote`原样输出`+`

```python
>>> from urllib import parse
>>> parse.unquote('https://translate.google.cn/#view=home&op=translate&sl=en&tl=zh-CN&text=Line%2023%3A%20Char%2016%3A%20invalid%20operation%3A%201%20%3C%3C%20count%20(shift%20count%20type%20int%2C%20must%20be%20unsigned%20integer)%20(solution.go)')
'https://translate.google.cn/#view=home&op=translate&sl=en&tl=zh-CN&text=Line 23: Char 16: invalid operation: 1 << count (shift count type int, must be unsigned integer) (solution.go)'
>>>
```



### 1.2 `quote`, `quote_plus` -- 编码url

两者基本一样，除了`quote_plus`把空格` `和斜线`/`编码为`+`和`%2F`





### 1.3 `urlparse` -- 解析出url的各个部分

```python
parse.urlparse('http://imglf4.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg')
```

```
ParseResult(scheme='http', netloc='imglf4.nosdn0.126.net', path='/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg', params='', query='', fragment='')
```



### 1.4 `urlunparse` -- 组合url的各个部分

```
component = parse.ParseResult(scheme='http', netloc='imglf4.nosdn0.126.net', path='/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg', params='', query='', fragment='')
parse.urlunparse(component)
```

```
'http://imglf4.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg'
```



### 1.5 `parse_qs` 、`parse_qsl`-- 解析url查询参数

```python
result = parse.urlparse('http://imglf4.nosdn0.126.net/img/U2t6YVJaMDRkcW1TWGR3anluTkFTRkxjRnJDWkdZMUoyc0NyNGZKZndZdGo4NHE3OS9tOUZRPT0.jpg?imageView&thumbnail=500x0&quality=96&stripmeta=0&type=jpg%7Cwatermark&type=2&text=wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20=&font=bXN5aA==&gravity=southwest&dissolve=30&fontsize=240&dx=8&dy=10&stripmeta=0')
print(parse.parse_qs(result.query))
print('------------------------')
print(parse.parse_qsl(result.query))
```

```
{'thumbnail': ['500x0'], 'quality': ['96'], 'stripmeta': ['0', '0'], 'type': ['jpg|watermark', '2'], 'text': ['wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20='], 'font': ['bXN5aA=='], 'gravity': ['southwest'], 'dissolve': ['30'], 'fontsize': ['240'], 'dx': ['8'], 'dy': ['10']}
------------------------
[('thumbnail', '500x0'), ('quality', '96'), ('stripmeta', '0'), ('type', 'jpg|watermark'), ('type', '2'), ('text', 'wqkg56ul5Zyw55Oc5ZGx5ZGx5ZGxIC8gdG9uZ2RpZ3VhLmxvZnRlci5jb20='), ('font', 'bXN5aA=='), ('gravity', 'southwest'), ('dissolve', '30'), ('fontsize', '240'), ('dx', '8'), ('dy', '10'), ('stripmeta', '0')]
```



### 1.6 `urlencode` 构造查询参数

```
data = {'name': 'sj', 'age': 18}
parse.urlencode(data)
```

```
'name=sj&age=18'
```









# time 模块

## 1.1 `time` -- 获取时间戳

```
>>> import time
>>> time.time()
1572942258.5892923
```



## 1.2 `ctime` -- 获取指定时间戳或当前时间戳标准格式时间字符串

```
# 当前时间
>>> time.ctime()
'Tue Nov  5 16:28:51 2019'

# 指定时间戳
>>> time.ctime(1532335526900)
'Mon Oct 13 05:28:20 50527'
```



## 1.3 `gmtime`、`localtime` -- 指定或当前时间戳转时间元组

```
# 当前时间戳
>>> time.gmtime()
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=8, tm_min=39, tm_sec=31, tm_wday=1, tm_yday=309, tm_isdst=0)
>>> time.localtime()
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=16, tm_min=39, tm_sec=34, tm_wday=1, tm_yday=309, tm_isdst=0)


# 指定时间戳
>>> time.gmtime(1572942258.5892923)
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=8, tm_min=24, tm_sec=18, tm_wday=1, tm_yday=309, tm_isdst=0)

>>> time.localtime(1572942258.5892923)
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=16, tm_min=24, tm_sec=18, tm_wday=1, tm_yday=309, tm_isdst=0)
```



## 1.4 `strftime` -- 时间元组或当前时间转指定格式字时间符串

```
strftime(format[, tuple]) -> string
```

```
# 当前时间
>>> time.strftime('%Y-%m-%d %H:%M:%S')
'2019-11-05 16:43:27'

# 指定时间元组
>>> time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
'2019-11-05 16:44:31'
```





## 1.5 `strptime` -- 时间字符串转时间元祖

```
strptime(string, format) -> struct_time
```

```
>>> time.strptime('2019-11-05 16:44:31', '%Y-%m-%d %H:%M:%S')
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=16, tm_min=44, tm_sec=31, tm_wday=1, tm_yday=309, tm_isdst=-1)
```



## 1.6 `mktime` -- 时间元组转时间戳（浮点数）

```
mktime(...)
    mktime(tuple) -> floating point number
    
    Convert a time tuple in local time to seconds since the Epoch.
    Note that mktime(gmtime(0)) will not generally return zero for most
    time zones; instead the returned value will either be equal to that
    of the timezone or altzone attributes on the time module.
```



```
>>> time_tuple = time.strptime('2019-11-05 16:44:31', '%Y-%m-%d %H:%M:%S')
>>> time_tuple
time.struct_time(tm_year=2019, tm_mon=11, tm_mday=5, tm_hour=16, tm_min=44, tm_sec=31, tm_wday=1, tm_yday=309, tm_isdst=-1)
>>> time.mktime(time_tuple)
1572943471.0
```





# FAQS

## 1. 删除特殊字符（\xa0）

`\xa0`实际上是网页上的 不间断空白符`&nbsp;`

我们通常所用的空格是` \x20 `，是在标准ASCII可见字符 `0x20~0x7e` 范围内。
而` \xa0` 属于` latin1 `（ISO/IEC_8859-1）中的扩展字符集字符，代表空白符nbsp(non-breaking space)。
`latin1` 字符集向下兼容 ASCII （ `0x20~0x7e` ）。通常我们见到的字符多数是 `latin1 `的，比如在 MySQL 数据库中。

### 方法一：

```
# 此方法同时会删除\n,\t和空格
s = 'test\xa0test\n\t'
s = ''.join(s.split())
```

```
#　输出ｓ
'testtest'
```



### 方法二：

```
s = 'test\xa0test\n\t'
move = dict.fromkeys(ord(c) for c in '\xa0')  # 遍历的字符串为要删除的字符
s = s.translate(move)
```

```
#　输出ｓ
'testtest\n\t'
```

