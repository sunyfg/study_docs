# Mongodb

## NoSQL 数据库简介

NoSQL，泛指非关系型数据库，随着 Web2.0 和 Web3.0 技术的兴起，传统关系型数据库无法应对如此巨大的数据存储和处理。所以非关系型数据库得以发展。

## MongoDB 简介

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的产品。

MongoDB 用C++编写而成，性能良好，可以安装在多种系统上面。

## MongoDB 的启动和关闭

除了图形界面上启动或关闭MongoDB服务之外，还可以通过命令行来启动与关闭MongoDB服务。

```sh
net start 'mongodb server'
net stop 'mongodb server'
```

## 命令行客户端

在命令中调用 MongoDB 的指令，首先要在 Windows 系统配置 PATH 环境变量。

```sh
mongo --host localhost --port 27017
```

## MongoDB 图形客户端

- Robo 3T 是目前最好的可视化 MongoDB 客户端，而且是免费的
- Robomongo 公司还提供了功能更强大的 Studio 3T客户端，该软件为商业软件

## MongoDB 用户管理

### MongoDB 的账户

默认情况下 MongoDB 是不需要账户就能登录使用的，但是为了数据安全，我们还是需要设置一个登录的账户。

MongoDB 内置了很多种用户角色，创建用户的时候要分配角色

### 内置角色

以下四个角色的权限仅限于某个逻辑库，不能管理其他逻辑库：

- Read - 允许用户读取指定逻辑库
- readWrite - 允许用户读写指定逻辑库
- dbAdmin - 可以管理指定逻辑库
- userAdmin - 可以管理指定逻辑库的用户

以下四个角色只能创建在 admin 逻辑库中，可以管理其他逻辑库：

- readAnyDatabase - 只可以把用户创建在admin逻辑库中，允许读取任何逻辑库
- readWriteAnyDatabase - 只可以把用户创建在 admin 逻辑库中，允许读写任何逻辑库
- dbAdminAnyDatabase - 只可以把用户创建在admin逻辑库中，允许管理任何逻辑库
- userAdminAnyDatabase - 只可以把用户创建在admin逻辑库中，允许管理任何逻辑库用户

以下两种角色也是必须创建在admin逻辑库，root角色权限最大：

- clusterAdmin - 只可以把用户创建在admin逻辑库中，允许管理MongoDB集群
- root - 只可以把用户创建在admin逻辑库中，超级管理员，拥有最高权限

### 设置登录账户

首先要切换到admin逻辑库，然后创建root角色账户

```sh
use admin
db.createUser({
		user: "admin",
		pwd: "123456",
		roles: [{role: "root", db: "admin"}]
})
```

### 开启登录验证功能

即便创建了用户，但是 MongoDB 默认没有开启登录验证功能，所以需要开启这个功能。

创建 MongoDB 配置文件 mongo.cnf

```sh
dbpath=[mongodb path]/Server/4.0/data
logpath=[mongodb path]/Server/4.0/log/mongod.log
auth=true
```

### 重新安装 MongoDB 服务

为了让 MongoDB 服务加载 mongo.cnf 文件，需要重新安装 MongoDB 服务

```sh
mongod --config "[mongodb path]/Server/4.0/mongo.cnf" --reinstall
```

### 登录 MongoDB

进入到 MongoDB，如果用户隶属于哪个逻辑库，就先切换到该逻辑库，然后才能登陆

```sh
use admin
db.auth('admin', 'abc123456')
```

## MongoDB 的数据结构

MongoDB 用 BSON（二进制JSON）来保存数据。一条记录就是一个BSON，被称作文档（Document）

某些 BSON 聚集在一起，就形成了集合（Collection）

### 管理逻辑库

创建/切换逻辑库

```sh
use test
```

查看逻辑库

```sh
show dbs
```

删除逻辑库

```sh
db.dropDatabase()
```

### 管理集合

创建集合

```sh
db.createCollection('student')
```

查看集合

```sh
show collections
```

删除集合

```sh
db.student.drop()
```

查看集合记录数量

```js
db.student.count()
```

查看数据空间容量

```js
db.student.dataSize()
```

重命名集合

```js
db.student.renameCollection('stu')
```

### 添加记录

集合的 save 函数允许我们添加一条或者多条记录

```js
db.student.save({
	{name: '李强', sex: '男', age: 24},
	{name: '刘倩', sex: '女', age: 23},
	{name: '赵婷婷', sex: '女', age: 24},
})
```

### 主键值（ObjectId）

在集合中，文档之间都是松散，没有统一的字段约束。为了标识文档的唯一性，MongoDB 为每个文档都添加了主键字段（_id）

ObjectId 是一个12字节的BSON类型字符串

### 时间戳

MongoDB 存储日期会自动转换成格林尼治时区

ObjectId 字段里面包含了时间戳，所以我们可以提取出记录保存的时间：

```sh
> ObjectId('56c56dd4ca446fab71e4c38a').getTimestamp()
> ISODate('2016-02-18T07:08:04Z')
```

### 查询记录

find 函数可以从集合中提取记录，参数为查询条件：

```js
db.student.find();
db.student.find({name: '李强', sex: '男'});
db.student.find({sex: '女', age: {$gte: 20}});
```

### 表达式

MongoDB 的表达式必须写成 JSON 格式：

| 序号 | 表达式  | 意义     |
| ---- | ------- | -------- |
| 1    | $lt     | 小于     |
| 2    | $gt     | 大于     |
| 3    | $lte    | 小于等于 |
| 4    | $gte    | 大于等于 |
| 5    | $in     | 包括     |
| 6    | $nin    | 不包括   |
| 7    | $ne     | 不等于   |
| 8    | $all    | 全部     |
| 9    | $not    | 全部     |
| 10   | $or     | 或关系   |
| 11   | $exists | 含有字段 |

### 正则表达式

MongoDB 支持正则表达式查找数据：

```js
db.student.find({name: /^李/});
db.student.find({name: /^[a-zA-Z]{2,10}$/});
```

### 分页查找数据

可以利用 skip() 和 limit() 实现分页查询：

```js
db.student.find().limit(10);
db.student.find().skip(20).limit(10);
```

### 数据排序

sort() 函数可以用来对结果集排序。1 代表升序，-1 代表降序

```js
db.student.find().sort({name: 1});
db.student.find().sort({name: -1}).skip(10).limit(10);
```

### 排除重复

distinct() 函数代替 find() 函数查找不重复的记录：

```js
db.student.distinct('name');
db.student.distinct('name').sort(function() {return -1});
db.student.distinct('name').slice(0,5);
```

### 修改记录

update() 和 updateMany() 函数能实现对记录的修改：

```js
db.collection.updateMany({condition}, {$set: {data}});
```

```js
db.student.update({name: '李强'}, {$set: {age: 26, classno: '2-6'}})
db.student.updateMany({sex: '男', age: {$gte: 25}}, {$set: {classno: '2-6'}});
```

$unset 可以删除记录中的字段：

```js
db.student.updateMany({}, {$unset: {city: 1, tel: 1}});
```

$push 可以向数组属性添加元素：

$pull 可以删除数组属性的元素：

```js
db.teacher.update({name: 'Jack'}, {$push: {role: '教务主任'}})
db.teacher.update({name: 'Jack'}, {$pull: {role: '副校长'}})
```

### 删除记录

remove() 函数可以删除记录：

```js
db.student.remove()
db.student.remove({class: '6-2', sex: '男'})
```

### 创建索引的语法

因为MongoDB中存放了大量的数据，所以为了加快数据检索速度，需要为集合设置索引：

```js
db.collection.createIndex({keys: 1}, options)
```

索引字段按照升序排列，属性值为 1，降序排列属性值为 -1

### 创建索引的案例

因为创建索引的过程会阻塞 MongoDB，影响其他增删改查操作，所以 background 参数代表在空闲的时候创建索引：

```js
db.student.createIndex({name: 1})
db.student.dropIndexes()
db.student.dropIndex('name_1')
db.student.createIndex({name: 1}, {background: true, name: 'index_name'})
db.student.getIndexes()
```

### 唯一性索引

唯一性索引只能创建在每个记录都含有的公共字段上，在非公共字段上是不能创建唯一性索引的。

```js
db.student.createIndex({sid: 1}, {background: true, unique: true})
```

### 创建索引的原则

- 数据量很大的集合必须创建索引，相反则不需要创建索引
- 集合的数据读取多过写入，则需要创建索引
- 给经常被当做查询条件的字段设置索引

### 导出集合数据

mongoexport 命令可以把一个集合的数据导出成 JSON 或者 CSV 格式的文件：

```sh
mongoexport --host=localhost --port=27017 -u admin -p abc123456 --authenticationDatabase=admin -d school -c student -f '_id,name,sex,age' -o /Users/sunyanfeng/studeng.json
```

### 导入集合数据

mongoimport 命令可以把 JSON 或者 CSV 文件的数据导入到某个集合中：

```sh
mongoimport --host=localhost --port=27017 -u admin -p abc123456 --authenticationDatabase=admin -d test -c student --file /Users/sunyanfeng/studeng.json
```

### 导出逻辑库的数据

`mongodump` 命令可以导出逻辑库的数据：

```sh
mongodump --host=localhost --port=27017 -u admin -p abc123456 --authenticationDatabase=admin -d school -o /Users/sunyanfeng/
```

### 导入逻辑库的数据

`mongorestore` 命令可以把导出的数据导入到逻辑库中：

```sh
mongorestore --host=localhost --port=27017 -u admin -p abc123456 --authenticationDatabse=admin --drop -d school /Users/sunyanfeng/school
```

## Python 操作 MongoDB

`Python` 语言有很多操作 MongoDB 的模块，其中 `pymongo` 是特别常用的驱动模块：

```sh
pip install pymongo
```

### 创建链接

`MongoClient` 是 `MongoDB` 的客户端代理对象，可以用来执行增删改查操作，而且还内置了连接池：

```python
from pymongo import MongoClient
client = MongoClient(host='localhost', port=27017)
client.admin.authenticate('admin', 'abc123456')
```

### 数据导入

`insert_one` 和 `insert_many` 两个函数可以向 MongoDB 写入数据：

```python
client.school.teacher.insert_one({'name': '李璐'})
client.school.teacher.insert_many([
	{'name': '陈刚'},
	{'name': '郭丽丽'}
])
```

### 数据查询

`find_one` 和 `find` 两个函数可以从 MongoDB 中查询数据：

```py
teachers = client.school.teacher.find({})
for one in teachers:
	print(one['_id'], one['name'])
print('--------------------')
teacher = client.school.teacher.find_one({'name': '李璐'})
print(teacher['_id'], teacher(['name']))
```

### 数据修改

`update_one` 和 `update_many` 两个函数可以修改 MongoDB 数据：

```py
client.school.teacher.update_many({}, {'$set': {'role': ['班主任']}})
client.school.teacher.update_one({'name': '李璐'}, {'$set': {'sex': '女'}})
client.school.teacher.update_one({'name': '李璐'}, {'$push': {'role': '年级主任'}})
```

### 数据删除

`delete_one` 和 `delete_many` 两个函数可以删除 MongoDB 数据：

```py
client.school.teacher.delete_one({'name': '李璐'})
client.school.teacher.delete_many({})
```

### 其他操作

| 序号 | 函数       | 功能               |
| ---- | ---------- | ------------------ |
| 1    | `skip`     | 用于数据分页查询   |
| 2    | `limit`    | 用于数据分页查询   |
| 3    | `count`    | 查询记录总数       |
| 4    | `distinct` | 查询不重复的字段值 |
| 5    | `sort`     | 对记录排序         |

### 其他操作案例

`distinct` 和 `sort` 函数的参数类型很特殊，需要注意：

```py
teachers = client.school.teacher.find({}).skip(0).limit(10)
teachers = client.school.teacher.distinct('name')
teachers = client.school.teacher.find().sort([('name', -1)])
```

## MongoDB 的文件存储

### 存储文件

- 把文件直接保存在硬盘上：这种方式最简单，但是并不适用于分布式部署环境
- 把文件存储在文件服务器上：NAS 可以存放大量的文件，也能应对分布式环境，但是对文件的管理显得不方便
- 把文件存放在NoSQL数据库：普通SQL数据库，不适合存储文件，但是 MongoDB 却额外提供了文件存储

### GridFS 存储引擎

GridFS 是 MongoDB 的文件存储方案，主要用于存储超过 16 M（BSON文件限制）的文件（如：图片、音频等），对大文件有着更好的性能

### GridFS 存储原理

GridFS 使用两个集合来存储文件，一个是 `chunks` 集合，用来存放文件；另一个集合时 `files` ，用来存放文件元数据

GridFS 会把文件分割成若干 `chunks` (256KB)，然后在 files 集合记录它们

### 链接 GridFS

默认情况下 MongoClient 不提供操作 GridFS，需要创建 GridFS 对象：

```py
from gridfs import GridFS
db = client.school
gfs = GridFS(db, collection='book')
```

### 保存文件

`put` 函数可以把文件存储在 GridFS 中：

```py
file = open('/Users/sunyanfeng/Linux就该这么学.pdf', 'rb')
args = {'type': 'PDF', 'keyword': 'linux'}
gfs.put(file, filename='Linux就该这么学.pdf'， **args)
file.close()
```

### 查询文件

`find` 和 `fine_one` 函数可以查询 GridFS 中存储的文件：

```py
book = gfs.find_one({'filename': 'Linux就该这么学.pdf'})
print(book.filename)
print('%dM' % (math.ceil(book.length / 1024 / 1024)))
print('---------')
```

```py
books = gfs.find({'type': 'PDF'})
for one in books:
	# UTC 转换成北京时间
  uploadDate = one.uploadDate + datetime.timedelta(hours=8)
  # 格式化日期
  uploadDate = uploadDate.strftime('%Y-%m-%d %H:%M:%S')
  print(one._id, one.filename, uploadDate)
```

### 判断是否存储了文件

`exists` 函数可以判断 GridFS 是否存储了某个文件：

```py
from bson.objectid import ObjectId
rs = gfs.exists(ObjectId('5c2es345dg543f34d98f'))
print(rs)
rs=gfs.exists(**{'type': 'PDF'})
print(rs)
```

### 读取文件

`get` 函数可以从 GridFS 中读取文件，并且只能通过主键查找文件：

```py
document = gfs.get(ObjectId('5d8fg989s0d9f89dfdc98'))
file=open('/Users/sunyanfeng/Linux手册.pdf', 'wb')
file.write(document.read())
file.close()
```

### 删除文件

`delete` 函数可以从 GridFS 中删除文件，并且只能通过主键先查找记录：

```py
from bson.objectid import ObjectId
...
gfs.delete(ObjectId('5d8fg989s0d9f89dfdc98'))
```









