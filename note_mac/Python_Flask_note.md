#### 简单使用

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
	return 'Hello, World!'
```

#### python获取或设置系统环境变量

- 设置系统环境变量
  - `os.environ['环境变量名称'] = '环境变量值'`
  - `os.putenv('环境变量名称'， '环境变量值')`

- 获取系统环境变量
  - `os.environ['环境变量名称']`
  - `os.getenv('环境变量名称')`

####调试模式

在运行服务器之前可以设置:

- `FLASK_ENV=development`
- `FLASK_DEBUG=1`

```shell
$ export FLASK_ENV=development
$ export FLASK_DEBUG=1
$ flask run
```

#### 设置允许外网访问

```shell
# 方式一
# 终端运行时加 `--host=0.0.0.0`
flask run --host=0.0.0.0
# 方式二
# 修改代码 app.run() => app.run(host='0.0.0.0')
app.run(host='0.0.0.0')
```

#### 唯一URLs

Flask 的 URL 规则是基于 Werkzeug 的 routing 模块。该模块背后的思路是基于 Apache 和早期的 HTTP 服务器定下先例确保优雅和唯一的 URL。

以这两个规则为例，在 `/home/shiyanlou/Code/hello.py` 文件中添加如下的代码：

```python
@app.route('/projects/')
def projects():
    return 'The project page'

@app.route('/about')
def about():
    return 'The about page'
```

虽然它们看起来确实相似，但它们结尾斜线的使用在 URL 定义中不同。

第一种情况中，规范的 URL 指向 projects 尾端有一个斜线`/`。这种感觉很像在文件系统中的文件夹。访问一个结尾不带斜线的 URL 会被 Flask 重定向到带斜线的规范 URL 去。当访问 `http://127.0.0.1:5000/projects/` 时，页面会显示 `The project page`。

然而，第二种情况的 URL 结尾不带斜线，类似 UNIX-like 系统下的文件的路径名。此时如果访问结尾带斜线的 URL 会产生一个`404 “Not Found”`错误。当访问 `http://127.0.0.1:5000/about` 时，页面会显示 `The about page`；但是当访问 `http://127.0.0.1:5000/about/` 时，页面就会报错 `Not Found`。

当用户访问页面忘记结尾斜线时，这个行为允许关联的 URL 继续工作，并且与 Apache 和其它的服务器的行为一致，反之则不行，因此在代码的 URL 设置时斜线只可多写不可少写；另外，URL 会保持唯一，有助于避免搜索引擎索引同一个页面两次。

#### 构建URL

去构建一个 URL 来匹配一个特定的函数可以使用 `url_for()` 方法。它接受函数名作为第一个参数，以及一些关键字参数，每一个关键字参数对应于 URL 规则的变量部分。未知变量部分被插入到 URL 中作为查询参数。

```python
$ python3
>>> from flask import Flask, url_for
>>> app = Flask(__name__)
>>> @app.route('/')
... def index():
...     return 'index'
...
>>> @app.route('/login')
... def login():
...     return 'login'
...
>>> @app.route('/user/<username>')
... def profile(username):
...     return '{}\'s profile'.format(username)
...
>>> with app.test_request_context():
...     print(url_for('index'))
...     print(url_for('login'))
...     print(url_for('login', next='/'))
...     print(url_for('profile', username='John Doe'))
...
/
/login
/login?next=%2F   # 对字符串进行了转义，未知变量部分被当做查询参数
/user/John%20Doe
```

`test_request_context()` 方法告诉 Flask 表现得像是在处理一个请求，即使我们正在通过 Python shell 交互。大家可以仔细分析一下该函数的打印结果。



#### HTTP方法

HTTP (也就是 Web 应用协议) 有不同的方法来访问 URLs 。默认情况下，路由只会响应 GET 请求，但是能够通过给 `route()` 装饰器提供 `methods` 参数来改变。这里是一个例子:

```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        do_the_login()   # 如果是 POST 方法就执行登录操作
    else:
        show_the_login_form()   # 如果是 GET 方法就展示登录表单
```

如果使用 `GET` 方法，`HEAD` 方法将会自动添加进来。你不必处理它们。也能确保 `HEAD` 请求会按照 HTTP RFC (文档在 HTTP 协议里面描述) 要求来处理，因此你完全可以忽略这部分 HTTP 规范。同样地，自从 Flask 0.6 后，`OPTIONS` 方法也能自动为你处理。

也许你并不清楚 HTTP 方法是什么？别担心，这里有一个 HTTP 方法的快速入门以及为什么它们重要：

`HTTP`方法（通常也称为“谓词”）告诉服务器客户端想要对请求的页面做什么。

下面这些方法是比较常见的：

- GET：浏览器通知服务器只获取页面上的信息并且发送回来。这可能是最常用的方法。
- HEAD：浏览器告诉服务器获取信息，但是只对头信息感兴趣，不需要整个页面的内容。应用应该处理起来像接收到一个 GET 请求但是不传递实际内容。在 Flask 中你完全不需要处理它，底层的 Werkzeug 库会为你处理的。
- POST：浏览器通知服务器它要在 URL 上提交一些信息，服务器必须保证数据被存储且只存储一次。这是 HTML 表单通常发送数据到服务器的方法。
- PUT：同 POST 类似，但是服务器可能触发了多次存储过程，多次覆盖掉旧值。现在你就会问这有什么用，有许多理由需要如此去做。考虑下在传输过程中连接丢失：在这种情况下浏览器和服务器之间的系统可能安全地第二次接收请求，而不破坏其它东西。该过程操作 POST 方法是不可能实现的，因为它只会被触发一次。
- DELETE：移除给定位置的信息。
- OPTIONS：给客户端提供一个快速的途径来指出这个 URL 支持哪些 HTTP 方法。从 Flask 0.6 开始，自动实现了该功能。

现在在 HTML4 和 XHTML1 中，表单只能以 GET 和 POST 方法来提交到服务器。在 JavaScript 和以后的 HTML 标准中也能使用其它的方法。同时，HTTP 最近变得十分流行，浏览器不再是唯一使用 HTTP 的客户端。比如许多版本控制系统使用 HTTP。

#### Jinja2 模板使用

##### 注释

- 使用`{# #}`进行注释

```
{# {{ name }} #}
```

##### 变量代码块

- `{{}}`来表示变量名

```
{{ post.title }}
```

Jinja2 模板中的变量代码块可以是任意Python类型或者对象，只要它能够被Python的str()方法转换为一个字符串就可以。

###### 变量名属性获取方式

- 可以使用`.`来访问变量属性
- 也可以使用下标语法（`[]`）来获取属性

以下语句效果是一样的

```python
{{ foo.bar }}
{{ foo['bar'] }}
```

如果变量或属性不存在，则会返回一个未定义值。你可以对这类值做什么取决于应用的配 置，默认的行为是它如果被打印，其求值为一个空字符串，并且你可以迭代它，但其它 操作会失败。

> **实现**
>
> 为方便起见，Jinja2 中 `foo.bar` 在 Python 层中做下面的事情:
>
> - 检查 foo 上是否有一个名为 bar 的属性。
> - 如果没有，检查 foo 中是否有一个 `'bar'` 项 。
> - 如果没有，返回一个未定义对象。
>
> `foo['bar']` 的方式相反，只在顺序上有细小差异:
>
> - 检查在 foo 中是否有一个 `'bar'` 项。
> - 如果没有，检查 foo 上是否有一个名为 bar 的属性。
> - 如果没有，返回一个未定义对象。
>
> 如果一个对象有同名的项和属性，这很重要。此外，有一个 [`attr()`](http://docs.jinkan.org/docs/jinja2/templates.html#attr) 过滤 器，它只查找属性。





##### 控制代码块

- 用 `{% %}`定义的**控制代码块**，可以实现一些语言层次的功能，比如循环或者if语句

```
{% if user %}
	{{ user}}
{% else %}
	hello!
{ % endif % }

{% for index in indexs %}
	{{ index }}
{% endfor %}


{% if user %}
	{{ user }}
{% endif %}
```



##### 过滤器

过滤器本质就是函数。有时候我们不仅仅需要变量的值，还需要修改变量的修改变量的显示，甚至格式化，运算等等，而在模板中是不能直接调用Python的某些方法，那么就要用到过滤器。

###### 简单使用

- 过滤器使用方式：`变量名|过滤器`

```
{{variable | filter_name(*args)}}
```

- 如果没有任何参数给过滤器，则可以把括号省略。

```
{{variable | filter_name}}
```

###### 链式调用

```
{{ "hello world" | reverse | upper}}
```

###### 常见内建过滤器

###### 字符串操作

- `lower`: 把值转换为小写
- `upper`:把值转换为大写
- 



#### 消息闪现

> 使用消息闪现必须设置`secret_key`,以对消息进行加密

一个好的应用和用户界面都需要良好的反馈。如果用户得不到足够的反馈，那么应用 最终会被用户唾弃。 Flask 的闪现系统提供了一个良好的反馈方式。闪现系统的基 本工作方式是：在且只在下一个请求中访问上一个请求结束时记录的消息。一般我们 结合布局模板来使用闪现系统。注意，浏览器会限制 cookie 的大小，有时候网络服 务器也会。这样如果消息比会话 cookie 大的话，那么会导致消息闪现静默失败。

```python
from flask import Flask, flash, redirect, render_template, \
     request, url_for

app = Flask(__name__)
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != 'admin' or \
           request.form['password'] != 'secret':
            error = 'Invalid credentials'
        else:
            flash('You were successfully logged in')
            return redirect(url_for('index'))
    return render_template('login.html', error=error)
```

```html
<!doctype html>
<title>My Application</title>
{% with messages = get_flashed_messages() %}
  {% if messages %}
    <ul class=flashes>
    {% for message in messages %}
      <li>{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
{% block body %}{% endblock %}
```

##### 消息闪现的类别

闪现消息还可以指定类别，如果没有指定，那么缺省的类别为 `'message'` 。不同的 类别可以给用户提供更好的反馈。例如错误消息可以使用红色背景。

使用flash指定消息类别

`flash('flash_message', 'error')`

模板中的 [`get_flashed_messages()`](https://dormousehole.readthedocs.io/en/latest/api.html#flask.get_flashed_messages) 函数也应当返回类别，显示消息的循环 也要略作改变：

```html
{% with messages = get_flashed_messages(with_categories=true) %}
  {% if messages %}
    <ul class=flashes>
    {% for category, message in messages %}
      <li class="{{ category }}">{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
```

##### 过滤闪现消息

以下例子只输出消息类别为`error`的消息

```html
{% with errors = get_flashed_messages(category_filter=["error"]) %}
{% if errors %}
<div class="alert-message block-message error">
  <a class="close" href="#">×</a>
  <ul>
    {%- for msg in errors %}
    <li>{{ msg }}</li>
    {% endfor -%}
  </ul>
</div>
{% endif %}
{% endwith %}
```





### 模板代码复用



#### 宏

##### 一.什么是宏

```
宏是Jinja2中的函数,调用后,直接返回一个模板,或者字符串
当模板中出现大量重复功能代码的时候,可以使用宏来进行封装
```

##### 二.定义宏,使用宏

- 定义宏,可以在当前文件,也可以是其他文件

```html
{% macro input(name,value='',type='text') %}
    <input type="{{type}}" name="{{name}}" value="{{value}}">
{% endmacro %}
```

- 使用当前文件宏

```html
{{ input('name' value='zs')}}
```

- 使用其他文件宏

```html
    {%import 'filename.html' as 别名%}
    {%别名.函数名(参数)%}
```

##### 三.代码展示

- 使用当前文件宏,其他文件宏
- 当前文件test.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    {# 定义宏 #}
    {% macro input(name,password) %}
        <label>{{ name }}:</label><input type="text" name="username"><br>
        <label>{{ password }}:</label><input type="password" name="username"><br>
    {% endmacro %}

    {# 使用当前文件宏 #}
    {{ input("账号","密码") }}


    {# 使用其他文件宏 #}
    {% import 'other_macro.html' as other_macro %}
    {{ other_macro.input2('用户名',"密码") }}
    {{ other_macro.input3() }}

</body>
</html>
```

- 其他文件other_macro.html

```html
{% macro input(name,password) %}
    <label>{{ name }}:</label><input type="text" name="username"><br>
    <label>{{ password }}:</label><input type="password" name="username"><br>
{% endmacro %}
```



#### 模板继承,包含

##### 一.什么是继承

```
将公共的内容抽取到父类模板,共子类使用的形式称为继承.
一般Web开发中，继承主要使用在网站的顶部菜单、底部。这些内容可以定义在父模板中，子模板直接继承，而不需要重复书写。
```

##### 二.继承的格式
##### 父模板
- 父模板中使用多个block组成,格式: 
```python
{% block top %}
  顶部菜单
{% endblock top %}

{% block content %}
  正文内容
{% endblock content %}

{% block bottom %}
  底部
{% endblock bottom %}
```

###### 子模板
- 子模板使用格式: 
```python
{% extends 'base.html' %}
{% block content %}
 需要填充的内容
{% endblock content %}
```
- 继承后,子类完全拥有父类内容,并且子类可以进行重写,如果写保留父类内容使用: super()



- 模板继承使用时注意点：
  - 不支持多继承
  - 为了便于阅读，在子模板中使用extends时，尽量写在模板的第一行。
  - 不能在一个模板文件中定义多个相同名字的block标签。
  - 定义block模板的时候,一定要加上endblock结束标记

#### 包含

##### 一.什么是包含

Jinja2模板中，除了宏和继承，还支持一种代码重用的功能，叫包含(Include)。它的功能是将另一个模板整个加载到当前模板中，并直接渲染。

##### 二.包含的使用

格式:

```python
{% include 'hello.html' %}
或者
{% include 'hello.html' ignore missing %}
```

- 提示: ignore missing 加上后如果文件不存在,不会报错

##### 三.代码展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {% include '其他模板文件' ignore missing %}
</body>
</html>
```

#### 小结

- 宏(Macro)、继承(Block)、包含(include)均能实现代码的复用。
- 继承(Block)的本质是代码替换，一般用来实现多个页面中重复不变的区域。
- 宏(Macro)的功能类似函数，可以传入参数，需要定义、调用。
- 包含(include)是直接将目标模板文件整个渲染出来。







### Flask-WTF

> 使用之前安装： pip install flask-wtf

Flask-wtf 是对wtforms 的继承。

#### 渲染属性

> 详见： [wtforms document](https://wtforms.readthedocs.io/en/latest/crash_course.html#rendering-fields)

渲染属性就好比一个字符串：

```python
>>> from wtforms import Form, StringField
>>> class SimpleForm(Form):
...   content = StringField('content')
...
>>> form = SimpleForm(content='foobar')
>>> str(form.content)
'<input id="content" name="content" type="text" value="foobar" />'
>>> unicode(form.content)
u'<input id="content" name="content" type="text" value="foobar" />'
```

真正渲染属性是通过`__call__	`方法来完成的

```python
>>> form.content(style="width: 200px;", class_="bar")
u'<input class="bar" id="content" name="content" style="width: 200px;" type="text" value="foobar" />'
```

简单使用(利用jinja 模板实现)：

先建一个表单类

```python
class LoginForm(Form):
    username = StringField('Username')
    password = PasswordField('Password')

form = LoginForm()
```

模板代码

```html
<form method="POST" action="/login">
    <div>{{ form.username.label }}: {{ form.username(class="css_class") }}</div>
    <div>{{ form.password.label }}: {{ form.password() }}</div>
</form>
```

#### 显示表单错误

模板代码

```html
<form method="POST" action="/login">
    <div>{{ form.username.label }}: {{ form.username(class="css_class") }}</div>
    {% if form.username.errors %}
        <ul class="errors">{% for error in form.username.errors %}<li>{{ error }}</li>{% endfor %}</ul>
    {% endif %}

    <div>{{ form.password.label }}: {{ form.password() }}</div>
    {% if form.password.errors %}
        <ul class="errors">{% for error in form.password.errors %}<li>{{ error }}</li>{% endfor %}</ul>
    {% endif %}
</form>
```



### 采坑记

#### 1. url_for

> `url_for()`函数适用于构建指定函数的URL

`url_for`操作的对象是函数，而不是route路径。

如果route和函数名不一样而导致使用url_for()错误，千万不要去route找错误。 
例如下面的代码：
```python
from flask import Flask, url_for
app = Flask(\__name__)

@app.route('/')
def index():
    pass

@app.route('/login')
def LOGIN():
    pass

with app.test_request_context():
    print(url_for('index'))
    print(url_for('login'))
```

print(url_for('index'))没有报错，就是一个反斜杠；print(url_for('login'))报错，抛出BuildError异常：

```python
/
Traceback (most recent call last): 
File “<\pyshell#12>”, line 3, in 
print(url_for(‘login’)) 
File “C:\Python35\lib\site-packages\flask\helpers.py”, line 332, in url_for 
return appctx.app.handle_url_build_error(error, endpoint, values) 
File “C:\Python35\lib\site-packages\flask\app.py”, line 1811, in handle_url_build_error 
reraise(exc_type, exc_value, tb) 
File “C:\Python35\lib\site-packages\flask_compat.py”, line 33, in reraise 
raise value 
File “C:\Python35\lib\site-packages\flask\helpers.py”, line 322, in url_for 
force_external=external) 
File “C:\Python35\lib\site-packages\werkzeug\routing.py”, line 1758, in build 
raise BuildError(endpoint, values, method, self) 
werkzeug.routing.BuildError: Could not build url for endpoint ‘login’. Did you mean ‘index’ instead?
```
把login修改为LOGIN：
```python
with app.test_request_context():
    print(url_for('index'))
    print(url_for('LOGIN'))
```
打印正常：
```python
/ 
/login
```
参数

> url_for()也可以附带一些参数，比如想要完整的URL，可以设置_external为Ture：
> url_for('.static',_external=True,filename='pic/test.png')
这样返回的url是http://localhost/static/pic/test.png

- endpoint 
  URL的端点（即函数的名字）
  values 
  URL的变量参数
- _external 
  如果设置为True，则生成一个绝对路径URL
- _scheme 
  一个字符串指定所需的URL方案。_external参数必须设置为True，不然会抛出ValueError。
- _anchor 
  如果设置了这个则给URL添加一个mao
- _method 
  如果设置这个则显示地调用这个HTTP方法











### 防范攻击

#### XSS：跨站脚本（Cross-site scripting）攻击

是注入攻击的一种，比如用户上传的评论中含有可执行的JavaScript语句，或者HTML语句加入一些事件(比如点击事件)，如果服务器不进行转义，可能造成意想不到的后果。

往往大型的 XSS 攻击（包括前段时间新浪微博的 XSS 注入）都是由于疏漏。我个人建议在使用模版引擎的 Web 项目中，开启（或不要关闭）类似 Django Template、Jinja2 中“默认转义”（Auto Escape）的功能。在不需要转义的场合，我们可以用类似 的方式取消转义。这种白名单式的做法，有助于降低我们由于疏漏留下 XSS 漏洞的风险。

#### CSRF：跨站请求伪造（Cross-site request forgery）

CSRF 顾名思义，是伪造请求，冒充用户在站内的正常操作。

我们知道，绝大多数网站是通过 cookie 等方式辨识用户身份（包括使用服务器端 Session 的网站，因为 Session ID 也是大多保存在 cookie 里面的），再予以授权的。所以要伪造用户的正常操作，最好的方法是通过 XSS 或链接欺骗等途径，让用户在本机（即拥有身份 cookie 的浏览器端）发起用户所不知道的请求。







## 其它问题

### python 连接sqlite3

引言: SQLite是基于文件系统的mini数据库，我们用以存放简便的数据，本文将描述在代码中碰到的并发问题。

1. 问题的提出

　最近在跑python代码过程中，使用sqlite的时候，碰到了如下错误信息：　
```python
Mon, 17 Apr 2017 16:00:00 base.py[line:131] ERROR Job "save_data (trigger: cron[day_of_week='mon-fri', hour='16'], next run at: 2017-04-18 16:00:00 CST)" raised an exception
Traceback (most recent call last):
  File "C:\Program Files\Python\lib\site-packages\apscheduler\executors\base.py", line 125, in run_job
    retval = job.func(*job.args, **job.kwargs)
  File "indexdata.py", line 238, in save_data
    sqlite_obj.insert_data("index50", datetime.datetime.now().strftime("%Y-%m-%d"), final_data['value'])
  File "C:\workspace\stock-crawler\stockindex\util\SQLiteUtil.py", line 27, in insert_data
    self.cx.execute(self.insert_sql_str, [name, date, valdata, datetime.datetime.now()])
sqlite3.ProgrammingError: SQLite objects created in a thread can only be used in thread xxxx
```
代码为多个线程进行数据库的读写操作。这里简要列出关键的数据库操作，主要集中在insert/update操作。
2. 问题分析

　从错误信息来分析，问题是sqlite本身应对多个线程并发访问过程中的冲突问题，由一个线程创建并访问的sqlite的数据库，无法允许另外一个线程进行访问。这里的解决办法可能有２个：

　通过队列规避多线程的访问
　通过设置或者多个访问管道，规避线程之间的冲突问题
　　从技术上评估，２个方法都可以解决问题，哪种更具优势？

3. 问题解决

　　通过网上搜索发现，其实可以通过设置sqlite文件的读取方式，可以简单规避掉线程之间的冲突问题：
```python
def initDB(self, file_path):
        self.file_path = file_path
        self.cx = sqlite3.connect(file_path, check_same_thread=False)
        self.cx.execute(self.create_table_str)
        self.cx.execute(self.create_detail_table_str)
        print("init the table strucutre successfully")
```
  check_same_thread这个设置为False，即可允许sqlite被多个线程同时访问
4. 总结

　对于sqlite而言，所有的读取或者打开操作，都是有check_same_thread的设置，与语言无关。
