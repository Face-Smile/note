## 字符编码

### unicode编码

`\u`开头的编码代表unicode 编码，是一个字符

`0x`开头的代表十六进制，实际是一个整数

`\x`对应的是UTF-8编码的数据，通过转化规则可以转换为unicode编码，就能得到对应的汉子，转换规则很简单，先将`\x`去掉，转换为数字

#### 十六进制数字字符串转unicode字符

```python
'''chr(int('十六进制字符串数字', 16))'''
def HexStr2Unicode(Hex_Str):
	Unicde_Str = ""
	for i in range(0,len(Hex_Str)//4):
	chr(int(Hex_Str[i*4:i*4+4], 16))
	Unicde_Str += chr(int(Hex_Str[i*4:i*4+4], 16) 
	return Unicde_Str

print(HexStr2Unicode(str_uhex))
```

#### unicode字符串转换成十六进制字符串

```python
def Unicode2HexStr(Unicde_Str):
    Hex_Str = ""
    for i in range(0, len(Unicde_Str)):
        Hex_Str += (hex(ord(Unicde_Str[i])).replace('0x','').zfill(4))
    return Hex_Str
print(Unicode2HexStr(U_Str))
```



## set

set的去重原理： 通过两个函数`__hash__` 和`__eq__`结合实现的

- 1. 当连个变量的哈希值不相同时，就认为这两个变量是不同的。
- 2. 当两个变量的哈希值一样时，调用`__eq__`方法，当返回值为True时，认为这两个变量是同一个，应该去除一个，返回False不去重。



## class

### 新式类

python3 中的类都是新式类，不管他是否显示的继承object。所有的类都继承自object

python2 ，python3 任何从object或者其它内置类型派生的类都会自动视为新式类

- 类和类型合并

类就是类型，并且类型就是类，并且所有类（以及由此所有的类型）继承自object。类是类型，并且一个实例的类型是该实例的类

- 继承的搜索顺序

多继承的钻石模式有一种略微不同的搜索顺序，总体而言，他们可能先横向搜索再纵向搜索，并且先广度搜索优先搜索，再深度优先搜索。找到后不在继续搜索。

- 针对内置函数的属性获取

\_\_getattr__ 和 \_\_getattribute__ 方法不在针对内置运算的隐式属性获取而运行。这意味着，它们不在针对\_\_X__ 运算符重载方法名而调用，这样的名称搜索从类开始，而不是从实例开始。

- 新的高级工具

新式类有一组新的类工具，包括slot、特性、描述符和\_\_getattribute__ 方法



### 新式类的扩展

slots实例将字符串属性名称顺序赋值给特殊的\_\_slots__ 类属性，新式类就有可能限制类的实例将有的合法属性集，又能够优化内存和速度性能。

使用： 这个属性一般在class语句顶层内将字符串名称顺序赋值给变量\_\_slots__ 而设置： 只有\_\_ slots__ 列表内的这些变量名可赋值为实例属性。实例属性必须在引用前赋值，即使列在\_\_slots__ 中也是这样。

```python
>>> class limiter(object):
...		__slots__ = ['age', 'name', 'job']
>>> x = limiter()
>>> x.age = 40
>>> x.age
40
>>> x.ape = 10000
AttributeError: 'limiter' object has no attribute 'ape'
>>> x.__dict__
AttributeError: 'limiter' object has no attribute '__dict__'
>>> getattr(x, 'age')
40
>>> setattr(x, 'age', 20)
>>> x.age
20
>>> 'age' in dir(x)
True
```

```python
>>> class D:
...		__slot__ = ['a', 'b', '__dict__' ]
...		c = 3
...		def __init__(self): self.d = 4
...
>>> x = D()
>>> x.d
4
>>> x.__dict__
{'d': 4}
```

```python
>>> class D:
...		__slot__ = ['a', 'b']
...		c = 3
...		def __init__(self): self.d = 4
...
>>> x = D()
AttributeError: 'D' object has no attribute 'd'
```

### 运算符重载

#### `__call__`

当调用实例时，使用`__call__`方法

````python
>>> class Callee:
...		def __call__(self, *args, **krags):
...			print('Called:', args, kargs)
>>> C = Callee()
>>> C(1, 2, 3)
Called: (1, 2, 3) {}
>>> C(1, 2, 3, x=4, y =5)
Called: (1, 2, 3) {'y': 5, 'x': 4}
````



#### `__repr__` 和 `__str__` 的区别

`__repr__` 用于交互模式下的终端提示

`__str__` 用于对象的打印，相当于Java的toString 方法

当没有定义`__repr__` 但定义`__str__` 时，`__repr__` 调用`__str__`进行交互提示

### 类的伪私有属性

对于类中的 `__X`这样的变量名会自动变为 `_类名__X`, 必须调用后者才能访问到该属性。

这种方法叫`变量名压缩`，只发生在class语句内，可以防止和同一层次中其他类所创建的类似变量名相冲突。

### 元类

元类是子类化了`type`对象并且拦截类创建调用的类



### 静态方法和类方法

#### 静态方法

不用创建实例就可以调用的方法

python3 中类的简单方法（简单来说就是没有self参数）,这种方式虽说是静态方法，但是只能由类调用，而不能有实例调用。

要想在实例中也能调用，必须要一个staticmethod 声明,两种方法

- 在方法定义后（类中）添加语句 `method = staticmehod(method)`(假设方法名为`method`)
- 在方法前使用装饰器`@staticmethod`

#### 类方法

方法传入的第一个参数是`类`而不是`实例`



### 装饰器

#### 自定义装饰器









### Python try…except…else…finally

Python 中try和finally中都有return 时, 如果没有错误,最后返回的是finally块中的return,即无论如何finally块的与句都会执行.

```python
# test.py

def test():
  try:
    print(1)
    return 1
  finally:
		print(3)
    return 3
if __name__ == '__main__':
  print(test())
  

----------------
运行结果:
1
3
3
```

Python 中try, else和finally中都有return 时, 如果没有错误,最后返回的是finally块中的return,try语句块中的return会阻止else语句块的执行,但不会影响finally语句块的执行.(另外,else语句块前面必须由except语句块)

```python
# test.py

def test():
  try:
    print(1)
    return 1
  except:
    print(2)
    return 2
 	else:
    print(3)
    return 3
 	finally:
    print(4)
    return 4
if __name__ == '__main__':
  print(test())
  
---------
运行结果:
1
4
4
```





### python 数据库连接池

需要按安装的包:DBUtils

DBUtils提供两种外部接口:

- PersistentDB: 提供线程专用的数据库连接,并自动管理连接
- PooledDB : 提供线程间可共享的数据库连接,并自动管理.

实例(简单的数据库连接池:

```python
import MySQLdb
from DBUtils.PooledDB import PooledDB
pool = PooledDB(MySQLdb,5,host='localhost',user='root',\passwd='pwd',db='myDB',port=3306) #5为连接池里的最少连接数
conn = pool.connection()  #以后每次需要数据库连接就是用connection（）函数获取连接就好了
cur=conn.cursor()
SQL="select * from table1"
r=cur.execute(SQL)
r=cur.fetchall()
cur.close()
conn.close()
```

数据库连接工具包:

```python
import pymysql
import DBUtils
class OPMysql(object):

    __pool = None

    def __init__(self):
        # 构造函数，创建数据库连接、游标
        self.coon = OPMysql.getmysqlconn()
        self.cur = self.coon.cursor(cursor=pymysql.cursors.DictCursor)


    # 数据库连接池连接
    @staticmethod
    def getmysqlconn():
        if OPMysql.__pool is None:
            __pool = PooledDB(creator=pymysql, mincached=1, maxcached=20, host=mysqlInfo['host'], user=mysqlInfo['user'], passwd=mysqlInfo['passwd'], db=mysqlInfo['db'], port=mysqlInfo['port'], charset=mysqlInfo['charset'])
            print(__pool)
        return __pool.connection()

    # 插入\更新\删除sql
    def op_insert(self, sql):
        print('op_insert', sql)
        insert_num = self.cur.execute(sql)
        print('mysql sucess ', insert_num)
        self.coon.commit()
        return insert_num

    # 查询
    def op_select(self, sql):
        print('op_select', sql)
        self.cur.execute(sql)  # 执行sql
        select_res = self.cur.fetchone()  # 返回结果为字典
        print('op_select', select_res)
        return select_res

    #释放资源
    def dispose(self):
        self.coon.close()
        self.cur.close()
```

PooledDB 参数解释:

1. `mincached`:最少的空闲连接数,如果空闲连接数小于这个数,pool会创建一个新的连接.
2. `maxcached`:最大的空连接数吗,如果空闲连接数大于这个数, pool会关闭连接.
3. `maxconnections`: 最大的连接数,进程中最大可创建的线程数.
4.  `blocking`: 当连接数达到最大的连接数.再次请求时,如果这个值时True,请求连接的程序会一直等待,直到当前连接数小于最大连接数;如果值为False,会报错.
5. `maxshared`: 当连接数达到这个数时,请求新的连接会分享已经分配出去的连接.

在uwsgi中，每个http请求都会有一个进程，连接池中配置的连接数都是一个进程为单位的（**即上面的最大连接数，都是在一个进程中创建的线程数**），如果业务中，一个http请求中需要的sql连接数不是很多的话（其实大多数都只需要创建一个连接），配置的连接数配置都不需要太大。

**连接池对性能的提升：**

- 在程序创建连接的时候，可以从一个空闲的连接中获取，不需要重新初始化连接，提升获取连接的速度。
- 关闭连接的时候，把连接放回连接池，而不是真正的关闭，所以可以减少频繁的打开和关闭连接。