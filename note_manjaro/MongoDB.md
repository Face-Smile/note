# 1. MongoDB 配置 

下载: https://www.mongodb.org/dl/linux/x86_64

```bash
tar -xvzf mongodb-linux-x86_64-3.2.10.tgz     //解压
mv mongodb-linux-x86_64-3.2.10 /usr/local/mongodb      //将解压后的文件移动到指定目录并改名
cd /usr/local/mongodb/    //切换到mongodb
```

- 在mongodb目录下创建目录data/db ，以及/log目录

```kotlin
cd /usr/local/mongodb/    //切换到mongodb
mkdir data  //创建data目录
mkdir log    //创建log日志目录
cd data       //切换到data目录
mkdir db     //创建db 目录
```

- 系统profile配置，配置环境，这是每装一个软件的必备步骤，在profile文件最后面添加环境变量

```bash
vi /etc/profile  
  
export MONGODB_HOME=/usr/local/mongodb  
export PATH=$PATH:$MONGODB_HOME/bin  
```

保存后，重启系统配置

```bash
source /etc/profile
```

- 在mongodb目录下创建conf目录，并创建mongodb.conf配置文件

```cpp
vim mongodb.conf

        cd /usr/local/mongodb/    //切换到mongodb
        mkdir conf //创建conf目录
        cd conf  //切换到conf  
        touch mongodb.conf  //创建mongodb.conf配置文件
```

配置一些信息在mongodb.conf 中：

```bash
dbpath = /usr/local/mongodb/data/db #数据文件存放目录  
logpath = /usr/local/mongodb/log/mongodb.log #日志文件存放目录  
port = 27017  #端口  
fork = true  #以守护程序的方式启用，即在后台运行  
```

### 一些准备好，启动服务

```bash
cd /usr/local/mongodb/    //切换到mongodb
./bin/mongod --config ./conf/mongodb.conf  //启动服务
```

连接mongodb

```bash
cd /usr/local/mongodb/bin
./mongo
```



### 配置systemctl 服务

```
cd /lib/systemd/system  
vi mongodb.service 
```



```text
[Unit]  
Description=mongodb 
After=network.target remote-fs.target nss-lookup.target  

[Service]  
Type=forking 
ExecStart=/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/conf/mongodb.conf  
ExecReload=/bin/kill -s HUP $MAINPID  
ExecStop=/usr/local/mongodb/bin/mongod --shutdown --config /usr/local/mongodb/conf/mongodb.conf  
PrivateTmp=true 

[Install]  
WantedBy=multi-user.target  
```

- 设置权限
   chmod 754 mongodb.service
- 启动关闭服务，设置开机启动

```bash
#启动服务  
systemctl start mongodb.service    
#关闭服务    
systemctl stop mongodb.service    
#开机启动    
systemctl enable mongodb.service
```

***注意:conf和service文件中设置路径，注意需要设置为绝对路径。**



# MongoDB 连接

标准 URI 连接语法：

```
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

- **mongodb://** 这是固定的格式，必须要指定。
- **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登陆这个数据库
- **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
- **portX** 可选的指定端口，如果不填，默认为27017
- **/database** 如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认打开 test 数据库。
- **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开

标准的连接格式包含了多个选项(options)，如下所示：

| 选项                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| replicaSet=name     | 验证replica set的名称。 Impliesconnect=replicaSet.           |
| slaveOk=true\|false | true:在connect=direct模式下，驱动会连接第一台机器，即使这台服务器不是主。在connect=replicaSet模式下，驱动会发送所有的写请求到主并且把读取操作分布在其他从服务器。false: 在 connect=direct模式下，驱动会自动找寻主服务器. 在connect=replicaSet 模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器。 |
| safe=true\|false    | true: 在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功。(还要参考 wtimeoutMS).false: 在每次更新之后，驱动不会发送getLastError来确保更新成功。 |
| w=n                 | 驱动添加 { w : n } 到getLastError命令. 应用于safe=true。     |
| wtimeoutMS=ms       | 驱动添加 { wtimeout : ms } 到 getlasterror 命令. 应用于 safe=true. |
| fsync=true\|false   | true: 驱动添加 { fsync : true } 到 getlasterror 命令.应用于 safe=true.false: 驱动不会添加到getLastError命令中。 |
| journal=true\|false | 如果设置为 true, 同步到 journal (在提交到数据库前写入到实体中). 应用于 safe=true |
| connectTimeoutMS=ms | 可以打开连接的时间。                                         |
| socketTimeoutMS=ms  | 发送和接受sockets的时间。                                    |



# MongoDB 数据库

## 1. 创建数据库

```
use DATABASE_NAME
```

创建数据库后,需要插入数据,才会显示在数据库的列表中

```
> db.runoob.insert({"name":"菜鸟教程"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
runoob  0.000GB
```

## 2. 查看数据库

### 1. 查看当前数据库

```
db
```

```
> use runoob
switched to db runoob
> db
runoob
> 
```

### 2. 查看所有数据库

```
show dbs
```



## 3. 删除数据库

```
db.dropDatabase()
```

```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
runoob  0.000GB

> use runoob
switched to db runoob

> db.dropDatabase()
{ "dropped" : "runoob", "ok" : 1 }

> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```



# MongoDB 集合

## 1. 创建集合

MongoDB 中使用 **createCollection()** 方法来创建集合。

语法格式：

```
db.createCollection(name, options)
```

参数说明：

- name: 要创建的集合名称
- options: 可选参数, 指定有关内存大小及索引的选项

options 可以是如下参数：

| 字段        | 类型 | 描述                                                         |
| :---------- | :--- | :----------------------------------------------------------- |
| capped      | 布尔 | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。 **当该值为 true 时，必须指定 size 参数。** |
| autoIndexId | 布尔 | （可选）如为 true，自动在 _id 字段创建索引。默认为 false。   |
| size        | 数值 | （可选）为固定集合指定一个最大值，以千字节计（KB）。 **如果 capped 为 true，也需要指定该字段。** |
| max         | 数值 | （可选）指定固定集合中包含文档的最大数量。                   |

```
> use test
switched to db test
> db.createCollection("runoob")
{ "ok" : 1 }
>
```



创建固定集合 mycol，整个集合空间大小 6142800 KB, 文档最大个数为 10000 个。

```
> db.createCollection("mycol", { capped : true, autoIndexId : true, size : 
   6142800, max : 10000 } )
{ "ok" : 1 }
>
```



在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。

```
> db.mycol2.insert({"name" : "菜鸟教程"})
> show collections
mycol2
...
```



## 2.  查看集合

```
show tables
或者
show collections 
```

```
> show tables
runoob
```



## 3. 删除集合

MongoDB 中使用 drop() 方法来删除集合。

**语法格式：**

```
db.collection.drop()
```



```
> use runoob
switched to db runoob
> db.createCollection("runoob")     # 先创建集合，类似数据库中的表
> show tables
runoob
> db.runoob.drop()
true
> show tables
> 
```





# MongoDB 文档

## 1. 插入文档

MongoDB 使用 insert() 或 save() 方法向集合中插入文档，语法如下：

```
db.COLLECTION_NAME.insert(document)
```

```
db.collection.insertOne():向指定集合中插入一条文档数据
db.collection.insertMany():向指定集合中插入多条文档数据
```



以下文档可以存储在 MongoDB 的 runoob 数据库 的 col 集合中：

```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```

以上实例中 col 是我们的集合名，如果该集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。



我们也可以将数据定义为一个变量，如下所示：



```
> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});
```

执行后显示结果如下：

```
{
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

执行插入操作：

```
> db.col.insert(document)
WriteResult({ "nInserted" : 1 })
> 
```

插入文档你也可以使用 db.col.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。



## 2. 更新文档

### 1. update() 方法

update() 方法用于更新已存在的文档。语法格式如下：

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明：**

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别。

#### 实例

我们在集合 col 中插入如下数据：

```
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```

接着我们通过 update() 方法来更新标题(title):

```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息
> db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```

可以看到标题(title)由原来的 "MongoDB 教程" 更新为了 "MongoDB"。

以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。



### 2. save() 方法

save() 方法通过传入的文档来替换已有文档。语法格式如下：

```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

**参数说明：**

- **document** : 文档数据。
- **writeConcern** :可选，抛出异常的级别。





### 3. 在3.2版本开始，MongoDB提供以下更新集合文档的方法：

- db.collection.updateOne() 向指定集合更新单个文档
- db.collection.updateMany() 向指定集合更新多个文档

首先我们在test集合里插入测试数据

```
use test
db.test_collection.insert( [
{"name":"abc","age":"25","status":"zxc"},
{"name":"dec","age":"19","status":"qwe"},
{"name":"asd","age":"30","status":"nmn"},
] )
```

更新单个文档

```
> db.test_collection.updateOne({"name":"abc"},{$set:{"age":"28"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.test_collection.find()
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "zxc" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "qwe" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "nmn" }
>
```

更新多个文档

```
> db.test_collection.updateMany({"age":{$gt:"10"}},{$set:{"status":"xyz"}})
{ "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }
> db.test_collection.find()
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "xyz" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "xyz" }
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "xyz" }
>
```



### 4. 异常级别

- WriteConcern.NONE:没有异常抛出
- WriteConcern.NORMAL:仅抛出网络错误异常，没有服务器错误异常
- WriteConcern.SAFE:抛出网络错误异常、服务器错误异常；并等待服务器完成写操作。
- WriteConcern.MAJORITY: 抛出网络错误异常、服务器错误异常；并等待一个主服务器完成写操作。
- WriteConcern.FSYNC_SAFE: 抛出网络错误异常、服务器错误异常；写操作等待服务器将数据刷新到磁盘。
- WriteConcern.JOURNAL_SAFE:抛出网络错误异常、服务器错误异常；写操作等待服务器提交到磁盘的日志文件。
- WriteConcern.REPLICAS_SAFE:抛出网络错误异常、服务器错误异常；等待至少2台服务器完成写操作。





## 3. 删除文档

 MongoDB 是 2.6 版本以后的，语法格式如下：

```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明：**

- **query** :（可选）删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- **writeConcern** :（可选）抛出异常的级别。



### 实例

以下文档我们执行两次插入操作：

```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```

使用 find() 函数查询数据：

```
> db.col.find()
{ "_id" : ObjectId("56066169ade2f21f36b03137"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
{ "_id" : ObjectId("5606616dade2f21f36b03138"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
```

接下来我们移除 title 为 'MongoDB 教程' 的文档：

```
>db.col.remove({'title':'MongoDB 教程'})
WriteResult({ "nRemoved" : 2 })           # 删除了两条数据
>db.col.find()
……                                        # 没有数据
```

------

如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：

```
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```

如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：

```
>db.col.remove({})
>db.col.find()
>
```



## 4. 查询文档	

MongoDB 查询文档使用 find() 方法。

find() 方法以非结构化的方式来显示所有文档。

### 语法

MongoDB 查询数据的语法格式如下：

```
db.collection.find(query, projection)
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```
>db.col.find().pretty()
```

pretty() 方法以格式化的方式来显示所有文档。

### 实例

以下实例我们查询了集合 col 中的数据：

```
> db.col.find().pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```

除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。

### MongoDB 与 RDBMS Where 语句比较

如果你熟悉常规的 SQL 数据，通过下表可以更好的理解 MongoDB 的条件语句查询：

| 操作       | 格式         | 范例                                        | RDBMS中的类似语句       |
| :--------- | :----------- | :------------------------------------------ | :---------------------- |
| 等于       | `{:`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
| 小于       | `{:{$lt:}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
| 小于或等于 | `{:{$lte:}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
| 大于       | `{:{$gt:}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
| 大于或等于 | `{:{$gte:}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
| 不等于     | `{:{$ne:}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

------

### MongoDB AND 条件

MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，即常规 SQL 的 AND 条件。

语法格式如下：

```
>db.col.find({key1:value1, key2:value2}).pretty()
```



# MongoDB 条件操作符

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte





# MongoDB $type 操作符

$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。

MongoDB 中可以使用的类型如下表所示：

| **类型**                | **数字** | **备注**         |
| :---------------------- | :------- | :--------------- |
| Double                  | 1        |                  |
| String                  | 2        |                  |
| Object                  | 3        |                  |
| Array                   | 4        |                  |
| Binary data             | 5        |                  |
| Undefined               | 6        | 已废弃。         |
| Object id               | 7        |                  |
| Boolean                 | 8        |                  |
| Date                    | 9        |                  |
| Null                    | 10       |                  |
| Regular Expression      | 11       |                  |
| JavaScript              | 13       |                  |
| Symbol                  | 14       |                  |
| JavaScript (with scope) | 15       |                  |
| 32-bit integer          | 16       |                  |
| Timestamp               | 17       |                  |
| 64-bit integer          | 18       |                  |
| Min key                 | 255      | Query with `-1`. |
| Max key                 | 127      |                  |

## MongoDB 操作符 - $type 实例

如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：

```
db.col.find({"title" : {$type : 2}})
或
db.col.find({"title" : {$type : 'string'}})
```

输出结果为：

```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
```



## MongoDB limit和skip方法

## limit()方法

如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。

基本语法：

```
db.COLLECTION_NAME.find().limit(NUMBER)  
```

> 注：如果你们没有指定limit()方法中的参数则显示集合中的所有数据。

## skip()方法

我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。

语法：

```
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)  
```

> **注:**skip()方法默认参数为 0 。



# MongoDB 排序

在MongoDB中使用使用sort()方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列。

语法：

sort()方法基本语法如下所示：

```
db.COLLECTION_NAME.find().sort({KEY:1})  
```

实例：

```js
db.col.find({},{"title":1,_id:0}).sort({"likes":-1})  { "title" : "PHP 教程" }  { "title" : "Java 教程" }  { "title" : "MongoDB 教程" }
```



> **注：** 如果没有指定sort()方法的排序方式，默认按照文档的升序排列。

# MongoDB 索引限制

## 额外开销

每个索引占据一定的存储空间，在进行插入，更新和删除操作时也需要对索引进行操作。所以，如果你很少对集合进行读取操作，建议不使用索引。

## 内存(RAM)使用

由于索引是存储在内存(RAM)中,你应该确保该索引的大小不超过内存的限制。

如果索引的大小大于内存的限制，MongoDB会删除一些索引，这将导致性能下降。

## 查询限制

索引不能被以下的查询使用：

- 正则表达式及非操作符，如 $nin, $not, 等。
- 算术运算符，如 $mod, 等。
- $where 子句

所以，检测你的语句是否使用索引是一个好的习惯，可以用explain来查看。

## 索引键限制

从2.6版本开始，如果现有的索引字段的值超过索引键的限制，MongoDB中不会创建索引。

## 插入文档超过索引键限制

如果文档的索引字段值超过了索引键的限制，MongoDB不会将任何文档转换成索引的集合。与mongorestore和mongoimport工具类似。

## 最大范围

- 集合中索引不能超过64个
- 索引名的长度不能超过125个字符
- 一个复合索引最多可以有31个字段



索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构



# MongoDB 索引



## ensureIndex() 方法

MongoDB使用 ensureIndex() 方法来创建索引。

### 语法

ensureIndex()方法基本语法格式如下所示：

```
  >db.COLLECTION_NAME.ensureIndex({KEY:1})  
```

语法中 Key 值为你要创建的索引字段，1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可。

### 实例

```
  >db.col.ensureIndex({"title":1})  >  
```

ensureIndex() 方法中你也可以设置使用多个字段创建索引（关系型数据库中称作复合索引）。

```
  >db.col.ensureIndex({"title":1,"description":-1})  >  
```

ensureIndex() 接收可选参数，可选参数列表如下：

| Parameter          | Type          | Description                                                  |
| :----------------- | :------------ | :----------------------------------------------------------- |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为**false**。 |
| unique             | Boolean       | 建立的索引是否唯一。指定为true创建唯一索引。默认值为**false**. |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。 |
| dropDups           | Boolean       | 在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 **false**. |
| sparse             | Boolean       | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 **false**. |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。 |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。 |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language. |

### 实例

在后台创建索引：

```
db.values.ensureIndex({open: 1, close: 1}, {background: true})  
```

通过在创建索引时加background:true 的选项，让创建工作在后台执行



# MongoDB 高级索引

考虑以下文档集合（users ）:

```
{     	"address": {       		"city": "Los Angeles",     		"state": "California",    		"pincode": "123"    	},    	"tags": [        		"music",       		"cricket",     		"blogs"     	],	"name": "Tom Benzamin" } 
```

以上文档包含了 address 子文档和 tags 数组。

## 索引数组字段

假设我们基于标签来检索用户，为此我们需要对集合中的数组 tags 建立索引。

在数组中创建索引，需要对数组中的每个字段依次建立索引。所以在我们为数组 tags 创建索引时，会为 music、cricket、blogs三个值建立单独的索引。

使用以下命令创建数组索引：

```
  >db.users.ensureIndex({"tags":1})  
```

创建索引后，我们可以这样检索集合的 tags 字段：

```
  >db.users.find({tags:"cricket"})  
```

为了验证我们使用使用了索引，可以使用 explain 命令：

```
  >db.users.find({tags:"cricket"}).explain()  
```

以上命令执行结果中会显示 "cursor" : "BtreeCursor tags_1" ，则表示已经使用了索引。

------

## 索引子文档字段

假设我们需要通过city、state、pincode字段来检索文档，由于这些字段是子文档的字段，所以我们需要对子文档建立索引。

为子文档的三个字段创建索引，命令如下：

```
  >db.users.ensureIndex({"address.city":1,"address.state":1,"address.pincode":1})  
```

一旦创建索引，我们可以使用子文档的字段来检索数据：

```
  >db.users.find({"address.city":"Los Angeles"})     
```

记住查询表达式必须遵循指定的索引的顺序。所以上面创建的索引将支持以下查询：

```
  >db.users.find({"address.city":"Los Angeles","address.state":"California"})   
```

同样支持以下查询：

```
  >db.users.find({"address.city":"LosAngeles","address.state":"California","address.pincode":"123"})  
```



# MongoDB 聚合

## aggregate() 方法

### 语法

MongoDB中聚合的方法使用aggregate()。

aggregate() 方法的基本语法格式如下所示：

```
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)  
```

### 实例

集合数据：

```javascript
{     
    title: 'MongoDB Overview',      
    description: 'MongoDB is no sql database',   
    by_user: 'w3cschool.cc',   
    url: 'http://www.w3cschool.cc',  
    tags: ['mongodb', 'database', 'NoSQL'],  
    likes: 100  
}, 
{  
    title: 'NoSQL Overview',    
    description: 'No sql database is very fast',    
    by_user: 'w3cschool.cc',   
    url: 'http://www.w3cschool.cc',   
    tags: ['mongodb', 'database', 'NoSQL'],   
    likes: 10  
},  
{
    title: 'Neo4j Overview',  
    description: 'Neo4j is no sql database',   
    by_user: 'Neo4j',   
    url: 'http://www.neo4j.com',  
    tags: ['neo4j', 'database', 'NoSQL'],  
    likes: 750  
}, 
```

现在我们通过以上集合计算每个作者所写的文章数，使用aggregate()计算结果如下：

```
> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}]) 
{    
    "result" : [      
        {          
            "_id" : "w3cschool.cc",       
            "num_tutorial" : 2     
        },       
        {         
            "_id" : "Neo4j",       
            "num_tutorial" : 1     
        }     
    ],    
    "ok" : 1 
}  
>  
```

以上实例类似sql语句： *select by_user, count(\*)  num_tutorial from mycol group by by_user*  

在上面的例子中，我们通过字段by_user字段对数据进行分组，并计算by_user字段相同值的总和。

`$group` 中 `_id`是必须的，其值为字符串`$字段名`,表示以该字段为基准进行分组，其值可以为`null`表示不分组。`$sum`可以为`1`，类似sql中的`count(*)`，如果为字段名，则对该字段名的值进行求和。

多字段分组

```JavaScript
> db.test.aggregate([{$group: { _id: {title: "$title", by_user:"$by_user"}, num: {$sum:1}}}])
{ "_id" : { "title" : "Neo4j Overview", "by_user" : "Neo4j" }, "num" : 1 }
{ "_id" : { "title" : "NoSQL Overview", "by_user" : "w3cschool.cc" }, "num" : 1 }
{ "_id" : { "title" : "MongoDB Overview", "by_user" : "w3cschool.cc" }, "num" : 1 }
```





下表展示了一些聚合的表达式:

| 表达式    | 描述                                           | 实例                                                         |
| --------- | ---------------------------------------------- | ------------------------------------------------------------ |
| $sum      | 计算总和。                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg      | 计算平均值                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min      | 获取集合中所有文档对应值得最小值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max      | 获取集合中所有文档对应值得最大值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push     | 在结果文档中插入值到一个数组中。               | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}]) |
| $addToSet | 在结果文档中插入值到一个数组中，但不创建副本。 | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}]) |
| $first    | 根据资源文档的排序获取第一个文档数据。         | db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}]) |
| $last     | 根据资源文档的排序获取最后一个文档数据         | db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}]) |



## 管道

管道在Unix和Linux中一般用于将当前命令的输出结果作为下一个命令的参数。  

MongoDB的聚合管道将MongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。管道操作是可以重复的。

  表达式：处理输入文档并输出。表达式是无状态的，只能用于计算当前聚合管道的文档，不能处理其它的文档。

这里我们介绍一下聚合框架中常用的几个操作：

-   $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
-   $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
-   $limit：用来限制MongoDB聚合管道返回的文档数。
-   $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
-   $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
-   $group：将集合中的文档分组，可用于统计结果。
-   $sort：将输入文档排序后输出。
-   $geoNear：输出接近某一地理位置的有序文档。

### 管道操作符实例

1、$project实例

```
    db.article.aggregate(     
    	{
    		$project : {         
    			title : 1 ,      
    			author : 1 ,   
    		}
    	}
    ); 
```

这样的话结果中就只还有_id,tilte和author三个字段了，默认情况下_id字段是被包含的，如果要想不包含_id话可以这样:

```
    db.article.aggregate(    
    	{ 
    		$project : { 
    			_id : 0 ,       
    			title : 1 ,   
    			author : 1     
    		}
    	}
    ); 
```

2.$match实例

```
    db.articles.aggregate( [   
    	{ $match : { score : { $gt : 70, $lte : 90 } } },  
    	{ $group: { _id: null, count: { $sum: 1 } } }                  
    	] );  
```

$match用于获取分数大于70小于或等于90记录，然后将符合条件的记录送到下一阶段$group管道操作符进行处理。

3.$skip实例

```
    db.article.aggregate(      
    	{ $skip : 5 });   
```

经过$skip管道操作符处理后，前五个文档被"过滤"掉。  



# MongoDB 复制（副本集）

## 什么是复制?

- 保障数据的安全性
- 数据高可用性 (24*7)
- 灾难恢复
- 无需停机维护（如备份，重建索引，压缩）
- 分布式读取数据

------

##   MongoDB复制原理

mongodb的复制至少需要两个节点。其中一个是主节点，负责处理客户端请求，其余的都是从节点，负责复制主节点上的数据。  

mongodb各个节点常见的搭配方式为：一主一从、一主多从。

主节点记录在其上的所有操作oplog，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，从而保证从节点的数据与主节点一致。  

MongoDB复制结构图如下所示：

  ![MongoDB复制结构图](MongoDB/replication.png)  

以上结构图总，客户端总主节点读取数据，在客户端写入数据到主节点是，  主节点与从节点进行数据交互保障数据的一致性。  

### 副本集特征：

-  N 个节点的集群
- 任何节点可作为主节点
- 所有写入操作都在主节点上
- 自动故障转移
- 自动恢复

------

## MongoDB副本集设置

在本教程中我们使用同一个MongoDB来做MongoDB主从的实验，  操作步骤如下：

1、关闭正在运行的MongoDB服务器。

  现在我们通过指定 --replSet 选项来启动mongoDB。--replSet 基本语法格式如下：

```shell
mongod --port "PORT" --dbpath "YOUR_DB_DATA_PATH" --replSet "REPLICA_SET_INSTANCE_NAME" 
```

### 实例

```shell
mongod --port 27017 --dbpath "D:\set up\mongodb\data" --replSet rs0  
```

以上实例会启动一个名为rs0的MongoDB实例，其端口号为27017。

启动后打开命令提示框并连接上mongoDB服务。

在Mongo客户端使用命令rs.initiate()来启动一个新的副本集。

我们可以使用rs.conf()来查看副本集的配置

查看副本集状态使用 rs.status() 命令

## 副本集添加成员

添加副本集的成员，我们需要使用多条服务器来启动mongo服务。进入Mongo客户端，并使用rs.add()方法来添加副本集的成员。

### 语法

   rs.add() 命令基本语法格式如下：  

```
>rs.add(HOST_NAME:PORT)  
```

### 实例

假设你已经启动了一个名为mongod1.net，端口号为27017的Mongo服务。  在客户端命令窗口使用rs.add() 命令将其添加到副本集中，命令如下所示：

```
>rs.add("mongod1.net:27017")
```

MongoDB中你只能通过主节点将Mongo服务添加到副本集中，  判断当前运行的Mongo服务是否为主节点可以使用命令db.isMaster() 。

MongoDB的副本集与我们常见的主从有所不同，主从在主机宕机后所有服务将停止，而副本集在主机宕机后，副本会接管主节点成为主节点，不会出现宕机的情况。



# Python 操作MongoDB

## 1. 准备工作

在开始之前，请确保已经安装好了MongoDB并启动了其服务，并且安装好了Python的PyMongo库。

## 2. 连接MongoDB

连接MongoDB时，我们需要使用PyMongo库里面的`MongoClient`。一般来说，传入MongoDB的IP及端口即可，其中第一个参数为地址`host`，第二个参数为端口`port`（如果不给它传递参数，默认是27017）：

```
import pymongo
client = pymongo.MongoClient(host='localhost', port=27017)复制代码
```

这样就可以创建MongoDB的连接对象了。

另外，`MongoClient`的第一个参数`host`还可以直接传入MongoDB的连接字符串，它以`mongodb`开头，例如：

```
client = MongoClient('mongodb://localhost:27017/')复制代码
```

这也可以达到同样的连接效果。

## 3. 指定数据库

MongoDB中可以建立多个数据库，接下来我们需要指定操作哪个数据库。这里我们以test数据库为例来说明，下一步需要在程序中指定要使用的数据库：

```
db = client.test复制代码
```

这里调用`client`的`test`属性即可返回test数据库。当然，我们也可以这样指定：

```
db = client['test']复制代码
```

这两种方式是等价的。

## 4. 指定集合

MongoDB的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表。

下一步需要指定要操作的集合，这里指定一个集合名称为students。与指定数据库类似，指定集合也有两种方式：

```
collection = db.students复制代码
collection = db['students']复制代码
```

这样我们便声明了一个`Collection`对象。

## 5. 插入数据

接下来，便可以插入数据了。对于students这个集合，新建一条学生数据，这条数据以字典形式表示：

```
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}复制代码
```

这里指定了学生的学号、姓名、年龄和性别。接下来，直接调用`collection`的`insert()`方法即可插入数据，代码如下：

```
result = collection.insert(student)
print(result)复制代码
```

在MongoDB中，每条数据其实都有一个`_id`属性来唯一标识。如果没有显式指明该属性，MongoDB会自动产生一个`ObjectId`类型的`_id`属性。`insert()`方法会在执行后返回`_id`值。

运行结果如下：

```
5932a68615c2606814c91f3d复制代码
```

当然，我们也可以同时插入多条数据，只需要以列表形式传递即可，示例如下：

```
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}

result = collection.insert([student1, student2])
print(result)复制代码
```

返回结果是对应的`_id`的集合：

```
[ObjectId('5932a80115c2606a59e8a048'), ObjectId('5932a80115c2606a59e8a049')]复制代码
```

实际上，在PyMongo 3.x版本中，官方已经不推荐使用`insert()`方法了。当然，继续使用也没有什么问题。官方推荐使用`insert_one()`和`insert_many()`方法来分别插入单条记录和多条记录，示例如下：

```
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

result = collection.insert_one(student)
print(result)
print(result.inserted_id)复制代码
```

运行结果如下：

```
<pymongo.results.InsertOneResult object at 0x10d68b558>
5932ab0f15c2606f0c1cf6c5复制代码
```

与`insert()`方法不同，这次返回的是`InsertOneResult`对象，我们可以调用其`inserted_id`属性获取`_id`。

对于`insert_many()`方法，我们可以将数据以列表形式传递，示例如下：

```
student1 = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

student2 = {
    'id': '20170202',
    'name': 'Mike',
    'age': 21,
    'gender': 'male'
}

result = collection.insert_many([student1, student2])
print(result)
print(result.inserted_ids)复制代码
```

运行结果如下：

```
<pymongo.results.InsertManyResult object at 0x101dea558>
[ObjectId('5932abf415c2607083d3b2ac'), ObjectId('5932abf415c2607083d3b2ad')]复制代码
```

该方法返回的类型是`InsertManyResult`，调用`inserted_ids`属性可以获取插入数据的`_id`列表。

## 6. 查询

插入数据后，我们可以利用`find_one()`或`find()`方法进行查询，其中`find_one()`查询得到的是单个结果，`find()`则返回一个生成器对象。示例如下：

```
result = collection.find_one({'name': 'Mike'})
print(type(result))
print(result)复制代码
```

这里我们查询`name`为`Mike`的数据，它的返回结果是字典类型，运行结果如下：

```
<class 'dict'>
{'_id': ObjectId('5932a80115c2606a59e8a049'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}复制代码
```

可以发现，它多了`_id`属性，这就是MongoDB在插入过程中自动添加的。

此外，我们也可以根据`ObjectId`来查询，此时需要使用bson库里面的`objectid`：

```
from bson.objectid import ObjectId

result = collection.find_one({'_id': ObjectId('593278c115c2602667ec6bae')})
print(result)复制代码
```

其查询结果依然是字典类型，具体如下：

```
{'_id': ObjectId('593278c115c2602667ec6bae'), 'id': '20170101', 'name': 'Jordan', 'age': 20, 'gender': 'male'}复制代码
```

当然，如果查询结果不存在，则会返回`None`。

对于多条数据的查询，我们可以使用`find()`方法。例如，这里查找年龄为20的数据，示例如下：

```
results = collection.find({'age': 20})
print(results)
for result in results:
    print(result)复制代码
```

运行结果如下：

```
<pymongo.cursor.Cursor object at 0x1032d5128>
{'_id': ObjectId('593278c115c2602667ec6bae'), 'id': '20170101', 'name': 'Jordan', 'age': 20, 'gender': 'male'}
{'_id': ObjectId('593278c815c2602678bb2b8d'), 'id': '20170102', 'name': 'Kevin', 'age': 20, 'gender': 'male'}
{'_id': ObjectId('593278d815c260269d7645a8'), 'id': '20170103', 'name': 'Harden', 'age': 20, 'gender': 'male'}复制代码
```

返回结果是`Cursor`类型，它相当于一个生成器，我们需要遍历取到所有的结果，其中每个结果都是字典类型。

如果要查询年龄大于20的数据，则写法如下：

```
results = collection.find({'age': {'$gt': 20}})复制代码
```

这里查询的条件键值已经不是单纯的数字了，而是一个字典，其键名为比较符号`$gt`，意思是大于，键值为20。

这里将比较符号归纳为下表。

| 符号   | 含义       | 示例                          |
| ------ | ---------- | ----------------------------- |
| `$lt`  | 小于       | `{'age': {'$lt': 20}}`        |
| `$gt`  | 大于       | `{'age': {'$gt': 20}}`        |
| `$lte` | 小于等于   | `{'age': {'$lte': 20}}`       |
| `$gte` | 大于等于   | `{'age': {'$gte': 20}}`       |
| `$ne`  | 不等于     | `{'age': {'$ne': 20}}`        |
| `$in`  | 在范围内   | `{'age': {'$in': [20, 23]}}`  |
| `$nin` | 不在范围内 | `{'age': {'$nin': [20, 23]}}` |

另外，还可以进行正则匹配查询。例如，查询名字以M开头的学生数据，示例如下：

```
results = collection.find({'name': {'$regex': '^M.*'}})复制代码
```

这里使用`$regex`来指定正则匹配，`^M.*`代表以M开头的正则表达式。

这里将一些功能符号再归类为下表。

| 符号      | 含义           | 示例                                                | 示例含义                           |
| --------- | -------------- | --------------------------------------------------- | ---------------------------------- |
| `$regex`  | 匹配正则表达式 | `{'name': {'$regex': '^M.*'}}`                      | `name`以M开头                      |
| `$exists` | 属性是否存在   | `{'name': {'$exists': True}}`                       | `name`属性存在                     |
| `$type`   | 类型判断       | `{'age': {'$type': 'int'}}`                         | `age`的类型为`int`                 |
| `$mod`    | 数字模操作     | `{'age': {'$mod': [5, 0]}}`                         | 年龄模5余0                         |
| `$text`   | 文本查询       | `{'$text': {'$search': 'Mike'}}`                    | `text`类型的属性中包含`Mike`字符串 |
| `$where`  | 高级条件查询   | `{'$where': 'obj.fans_count == obj.follows_count'}` | 自身粉丝数等于关注数               |

关于这些操作的更详细用法，可以在MongoDB官方文档找到：
https://docs.mongodb.com/manual/reference/operator/query/。

## 7. 计数

要统计查询结果有多少条数据，可以调用`count()`方法。比如，统计所有数据条数：

```
count = collection.find().count()
print(count)复制代码
```

或者统计符合某个条件的数据：

```
count = collection.find({'age': 20}).count()
print(count)复制代码
```

运行结果是一个数值，即符合条件的数据条数。

## 8. 排序

排序时，直接调用`sort()`方法，并在其中传入排序的字段及升降序标志即可。示例如下：

```
results = collection.find().sort('name', pymongo.ASCENDING)
print([result['name'] for result in results])复制代码
```

运行结果如下：

```
['Harden', 'Jordan', 'Kevin', 'Mark', 'Mike']复制代码
```

这里我们调用`pymongo.ASCENDING`指定升序。如果要降序排列，可以传入`pymongo.DESCENDING`。

## 9. 偏移

在某些情况下，我们可能想只取某几个元素，这时可以利用`skip()`方法偏移几个位置，比如偏移2，就忽略前两个元素，得到第三个及以后的元素：

```
results = collection.find().sort('name', pymongo.ASCENDING).skip(2)
print([result['name'] for result in results])复制代码
```

运行结果如下：

```
['Kevin', 'Mark', 'Mike']复制代码
```

另外，还可以用`limit()`方法指定要取的结果个数，示例如下：

```
results = collection.find().sort('name', pymongo.ASCENDING).skip(2).limit(2)
print([result['name'] for result in results])复制代码
```

运行结果如下：

```
['Kevin', 'Mark']复制代码
```

如果不使用`limit()`方法，原本会返回三个结果，加了限制后，会截取两个结果返回。

值得注意的是，在数据库数量非常庞大的时候，如千万、亿级别，最好不要使用大的偏移量来查询数据，因为这样很可能导致内存溢出。此时可以使用类似如下操作来查询：

```
from bson.objectid import ObjectId
collection.find({'_id': {'$gt': ObjectId('593278c815c2602678bb2b8d')}})复制代码
```

这时需要记录好上次查询的`_id`。

## 10. 更新

对于数据更新，我们可以使用`update()`方法，指定更新的条件和更新后的数据即可。例如：

```
condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 25
result = collection.update(condition, student)
print(result)复制代码
```

这里我们要更新`name`为`Kevin`的数据的年龄：首先指定查询条件，然后将数据查询出来，修改年龄后调用`update()`方法将原条件和修改后的数据传入。

运行结果如下：

```
{'ok': 1, 'nModified': 1, 'n': 1, 'updatedExisting': True}复制代码
```

返回结果是字典形式，`ok`代表执行成功，`nModified`代表影响的数据条数。

另外，我们也可以使用`$set`操作符对数据进行更新，代码如下：

```
result = collection.update(condition, {'$set': student})复制代码
```

这样可以只更新`student`字典内存在的字段。如果原先还有其他字段，则不会更新，也不会删除。而如果不用`$set`的话，则会把之前的数据全部用`student`字典替换；如果原本存在其他字段，则会被删除。

另外，`update()`方法其实也是官方不推荐使用的方法。这里也分为`update_one()`方法和`update_many()`方法，用法更加严格，它们的第二个参数需要使用`$`类型操作符作为字典的键名，示例如下：

```
condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 26
result = collection.update_one(condition, {'$set': student})
print(result)
print(result.matched_count, result.modified_count)复制代码
```

这里调用了`update_one()`方法，第二个参数不能再直接传入修改后的字典，而是需要使用`{'$set': student}`这样的形式，其返回结果是`UpdateResult`类型。然后分别调用`matched_count`和`modified_count`属性，可以获得匹配的数据条数和影响的数据条数。

运行结果如下：

```
<pymongo.results.UpdateResult object at 0x10d17b678>
1 0复制代码
```

我们再看一个例子：

```
condition = {'age': {'$gt': 20}}
result = collection.update_one(condition, {'$inc': {'age': 1}})
print(result)
print(result.matched_count, result.modified_count)复制代码
```

这里指定查询条件为年龄大于20，然后更新条件为`{'$inc': {'age': 1}}`，也就是年龄加1，执行之后会将第一条符合条件的数据年龄加1。

运行结果如下：

```
<pymongo.results.UpdateResult object at 0x10b8874c8>
1 1复制代码
```

可以看到匹配条数为1条，影响条数也为1条。

如果调用`update_many()`方法，则会将所有符合条件的数据都更新，示例如下：

```
condition = {'age': {'$gt': 20}}
result = collection.update_many(condition, {'$inc': {'age': 1}})
print(result)
print(result.matched_count, result.modified_count)复制代码
```

这时匹配条数就不再为1条了，运行结果如下：

```
<pymongo.results.UpdateResult object at 0x10c6384c8>
3 3复制代码
```

可以看到，这时所有匹配到的数据都会被更新。

## 11. 删除

删除操作比较简单，直接调用`remove()`方法指定删除的条件即可，此时符合条件的所有数据均会被删除。示例如下：

```
result = collection.remove({'name': 'Kevin'})
print(result)复制代码
```

运行结果如下：

```
{'ok': 1, 'n': 1}复制代码
```

另外，这里依然存在两个新的推荐方法——`delete_one()`和`delete_many()`。示例如下：

```
result = collection.delete_one({'name': 'Kevin'})
print(result)
print(result.deleted_count)
result = collection.delete_many({'age': {'$lt': 25}})
print(result.deleted_count)复制代码
```

运行结果如下：

```
<pymongo.results.DeleteResult object at 0x10e6ba4c8>
1
4复制代码
```

`delete_one()`即删除第一条符合条件的数据，`delete_many()`即删除所有符合条件的数据。它们的返回结果都是`DeleteResult`类型，可以调用`deleted_count`属性获取删除的数据条数。

## 12. 其他操作

另外，PyMongo还提供了一些组合方法，如`find_one_and_delete()`、`find_one_and_replace()`和`find_one_and_update()`，它们是查找后删除、替换和更新操作，其用法与上述方法基本一致。

另外，还可以对索引进行操作，相关方法有`create_index()`、`create_indexes()`和`drop_index()`等。

关于PyMongo的详细用法，可以参见官方文档：http://api.mongodb.com/python/current/api/pymongo/collection.html。

另外，还有对数据库和集合本身等的一些操作，这里不再一一讲解，可以参见官方文档：http://api.mongodb.com/python/current/api/pymongo/。

本节讲解了使用PyMongo操作MongoDB进行数据增删改查的方法。


作者：崔庆才丨静觅链接：https://juejin.im/post/5addbd0e518825671f2f62ee来源：掘金著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





# MongoDB 某个字段类型转换

使用JavaScript遍历，替换值类型，最后保存数据库

```javascript
db.uid2803301701.find({commentCount: {$type: 2}, forwardCount: {$type: 2}, likeCount: {$type: 2}}).forEach(function(x) { x.commentCount = NumberInt(x.commentCount); x.forwardCount= NumberInt(x.forwardCount); x.likeCount = NumberInt(x.likeCount); db.uid2803301701.save(x)})
```

