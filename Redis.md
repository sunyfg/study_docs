# Redis

## Mac 安装 Redis

### 官方文档

[https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-mac-os/](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-mac-os/)

### 前提准备

```sh
brew --version
```

### Installation

```sh
brew install redis
```

This will install Redis on your system.

### 在前台启动和停止Redis

```
redis-server
```

If successful, you'll see the startup logs for Redis, and Redis will be running in the foreground.

To stop Redis, enter `Ctrl-C`.

### 启动和停止Redis使用laund

As an alternative to running Redis in the foreground, you can also use `launchd` to start the process in the background:

```sh
brew services start redis
```

This launches Redis and restarts it at login. You can check the status of a `launchd` managed Redis by running the following:

```sh
brew services info redis
```

If the service is running, you'll see output like the following:

```sh
redis (homebrew.mxcl.redis)
Running: ✔
Loaded: ✔
User: miranda
PID: 67975
```

To stop the service, run:

```sh
brew services stop redis
```

### Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```sh
redis-cli
```

This will open the Redis REPL. Try running some commands:

```
127.0.0.1:6379> lpush demos redis-macOS-demo
OK
127.0.0.1:6379> rpop demos
"redis-macOS-demo"
```

### 配置文件

```sh
[Service]
Type=simple
ExecStart=/opt/homebrew/opt/redis/bin/redis-server /opt/homebrew/etc/redis.conf
Restart=always
WorkingDirectory=/opt/homebrew/var
StandardOutput=append:/opt/homebrew/var/log/redis.log
StandardError=append:/opt/homebrew/var/log/redis.log
```

```sh
/opt/homebrew/etc/redis-sunyanfeng.conf
/opt/homebrew/var/log/redis-sunyanfeng.log
```

修改 `ExecStart` 指定自定义配置文件 `/opt/homebrew/etc/redis-sunyanfeng.conf`

修改 `StandardOutput` 和 `StandardError` 指定自定义 log 文件 `/opt/homebrew/var/log/redis-sunyanfeng.log` 

## Redis 持久化

为了防止意外宕机丢失数据，Redis提供了两种持久化的方式

- RDB 持久化
- AOF 持久化
- 混合持久化 - Redis 4 之后支持

## Redis 常用参数

- port: 端口号，默认6397
- bind: 允许的IP，默认只允许本机访问
- time: client空闲多少秒后关闭链接，默认0代表无限制
- loglevel: 日志级别，分为：debug、verbose、notice、warning，默认为notice
- logfile: 日志文件地址
- syslogenabled: 把日志记录到系统日志，默认yes
- databases: 逻辑库的数量，默认16
- save: RDB文件同步的频率
- rdbcompression: 同步RDB文件的时候是否采用压缩，默认yes
- dbfilename: 镜像文件名称，默认dump.rdb
- dir: rdb文件的目录，默认Redis目录
- requirepass: 访问密码，默认无需密码
- maxclients: 最大连接数，默认无限制
- maxmemory: 占用内存的大小，默认无限制
- appendonly: 开启AOF备份
- appendfsync: AOF同步的频率，分为 no、everysec、always

## Redis的五种数据类型

* 字符串
* 哈希
* 列表
* 集合
* 有序集合

### 字符串

* String 类型既可以保存普通文字，也可以保存序列化的二进制数据
* String 类型最大可以存储512M数据

```shell
SET email sunyanfeng@163.com
GET email
DEL email
```

#### 字符串指令

* GETRANGE: 获得截取字符串内容

```
> GETRANGE email 0 3
```

* STRLEN: 获得字符串长度

```
> STRLEN email
```

* SETEX: 设置带有过期时间（秒）的 KEY-VALUE

```
> SETEX city 5 Shanhai
```

* MSET: 设置多个 KEY-VALUE

```
> MSET username Tom sex male
```

* MGET: 获得多个 VALUE

```
> MGET username sex
```

* APPEND: 用于在字符串结尾追加内容

```
> SET temp ABCD
> APPEND temp 1234 # ABCD1234
```

* INCR: 数字自增加1

```
> INCR num
```

* INCRBY: 数字加上指定的整数值

```
> INCRBY num 25
```

* INCRBYFLOAT: 数字加上指定的浮点数

```
> INCRBYFLOAT num 3.5
```

* DECR: 数字自增减1

```
> DECR num
```

* DECRBY: 数字减去指定的整数值

```
> DECRBY num 10
```

### 哈希类型

当我们觉得 VALUE需要保存更复杂的结构化数据，这时候可以使用哈希类型

```
key -> value={name,age,sex,mobile}
```

#### 哈希指令

* HSET: 设置哈希表字段

```
> HSET 8000 ename Tom
> HSET 8000 job salesman
```

- HMSET: 设置哈希表多个字段

```
> HMSET 8000 ename Tom job salesman deptno 10
```

- HGET: 获得哈希表字段值

```
> HGET 8000 ename
```

- HMGET: 获得多个哈希表字段值

```
> HMGET 8000 ename job deptno
```

- HGETALL: 获得所有哈希表字段值

```
> HGETALL 8000
```

- HKEYS: 获得所有哈希表字段名

```
> HKEYS 8000
```

- HLEN: 哈希表中的字段数量

```
> HLEN 8000
```

- HEXISTS: 判断哈希表是否存在某个字段

```
> HEXISTS 8000 job
```

- HVALS: 获得哈希表的所有字段值

```
> HVALS 8000
```

- HDEL: 删除哈希表的字段

```
> HDEL 8000 job deptno
```

- HINCRBY: 让哈希表某个字段值加上指定的整数值

```
> HINCRBY 8000 deptno 10
```

- HINCRBYFLOAT: 让哈希表某个字段值加上指定的浮点数

```
> HINCRBYFLOAT 8000 sal 350.5
```

### 列表类型

当我们需要向 VALUE 保存序列化的数据，可以使用**列表类型**

```
> RPUSH dname 技术部 后勤部 售后部
> LPUSH dname 秘书处
> LSET dname 2 销售部
> LRANGE dname 0 -1
```

#### 列表指令

- LLEN: 获得列表长度

```
> LLEN dname
```

- LINDEX: 获得列表某个元素

```
> LINDEX dname 0
```

- LINSERT: 在某个位置插入元素

```
> LINSERT dname BEFORE 秘书处 董事会
```

- LPOP: 删除列表最左边的元素

```
> LPOP dname
```

- RPOP: 删除列表最右边的元素

```
> RPOP dname
```

### 集合类型

如果我们需要列表中的元素不可以重复，可以使用集合类型

```
> SADD empno 8000
> SADD empno 8001
> SADD empno 8002
> SMEMBERS empno
```

#### 集合指令

- SCARD: 获得集合长度

```
> SCARD empno
```

- SISMEMBER: 判断是否含有某个元素

```
> SISMEMBER empno 8000
```

- SREM: 删除元素

```
> SREM empno 8000 8001
```

- SPOP: 随机删除并返回集合的某个元素

```
> SPOP empno
```

- SRANDMEMBER: 随机返回集合中的元素

```
> SRANDMEMBER empno 5
```

### 有序集合类型

有序集合类型有排序功能的集合，Redis 会按照元素分数值排序

```
> ZADD keyword 0 "鹿晗" 0 "张朝阳" 0 "马云"
> ZINCRBY keyword 1 "鹿晗"
> ZINCRBY keyword 5 "马云"
> ZINCRBY keyword 2 "张朝阳"
> ZREVRANGE keyword 0 -1
```

#### 有序集合指令

- ZCARD: 获得有序集合长度

```
> ZCARD keyword
```

- ZCOUNT: 查询某个分数值区间内的元素数量

```
> ZOUNT keyword 5 10
```

- ZSCORE: 返回元素的分数值

```
> ZSCORE keyword "马云"
```

- ZRANGE: 获得有序集合的内容（升序）

```
> ZRANGE keyword 0 -1
```

- ZREVRANGE: 获得有序集合的内容（降序）

```
> ZREVRANGE keyword 0 -1
```

- ZRANGEBYSCORE: 获得分数值区间内的集合内容（升序）

```
> ZRANGEBYSCORE keyword 5 10
> ZRANGEBYSCORE keyword 5 (10
> ZRANGEBYSCORE keyword 100000 +inf
```

- ZREVRANGEBYSCORE: 获得分数值区间内的集合内容（降序）

```
> ZREVRANGEBYSCORE keyword 10 5
```

- ZREM: 删除有序集合中的元素

```
> ZREM keyword "马云" "张朝阳"
```

- ZREMRANGEBYRANK: 删除排名区间内的元素

```
> ZREMRANGEBYRANK keyword 0 2
```

- ZREMRANGEBYSCORE: 删除分数值区间内的元素

```
> ZREMRANGEBYSCORE keyword -inf (5000
```

## Key 命令

- DEL: 删除记录

```
> DEL keyword
```

- EXISTS: 判断是否存在某个key

```
> EXISTS employee
```

- EXPIRE: 设置记录过期时间（秒）

```
> EXPIRE employee 5
```

- EXPIREAT: 设置记录的过期时间（UNIX时间戳）

```
> EXPIREAT employee 15448032000
```

- MOVE: 把记录迁移到其他逻辑库

```
> MOVE keyword 1
```

- RENAME: 修改Key名称

```
> RENAME employee tmp
```

## Redis 的性能

Redis 是单线程模型的NoSQL数据库，由C语言编写，官方提供的数据是可以达到100000+的QPS（每秒内查询次数）

## 编写启动脚本

在Redis目录下创建一个批处理文件，用来启动Redis的时候加载配置文件

```sh
# start.bat
redis-server redis.windows.conf
```

## 事务机制

### 回顾数据库事务机制

- 数据库引入事务机制是为了防止对数据文件直接操作的时候出现意外宕机，引发数据的错乱
- undo 和 redo 日志保证了业务操作的原子性

### Redis 为什么引入事务机制？

Redis 是异步单线程执行，也就是一个线程对应所有的客户端。哪个客户端上传了命令，线程就会执行，所以并不能保证一个客户端的多个命令，不会被其他客户端的命令插队

### Redis 事务的特点

Redis 的事务跟数据库事务是有明显差异的，它并不满足数据库事务的 ACID 属性。Redis 的事务更像是批处理执行。

| 序号 | 属性   | Redis | MySql |
| ---- | ------ | ----- | ----- |
| 1    | 原子性 | No    | Yes   |
| 2    | 一致性 | Yes   | Yes   |
| 3    | 隔离性 | Yes   | Yes   |
| 4    | 持久性 | No    | Yes   |

### 如何保持事务一致性？

为了保证事务的一致性，在开启事务之前必须要用 WATCH 命令监视要操作的记录。

```
> WATCH kill_num kill_user
```

### 如何开启事务？

- 利用 MULTI 命令开启一个事务

```
> MULTI
```

- 开启事务后的所有操作都不会立即执行，只有执行 EXEC 命令的时候才会批处理执行

```
> INCR kill_num
> RPUSH kill_user 9502
> EXEC
```

### 如何取消事务？

- Redis 并没有事务的回滚机制，所以并不能保证原子性
- 事务在没有提交之前，是可以取消事务的。如果事务已经提交执行，就无法取消了

```
> MULTI
> ......
> DISCARD
```

## redis-py 模块

利用 `pip` 命令安装 `redis-py` 模块：

```sh
pip install redis -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

模块主页：https://pypi.org/project/redis/

### 创建链接

```
import redis
r = redis.Redis({
	host='localhost',
	port=6379,
	password='123456',
	db=0
})
```

### 创建连接池

```
import redis
pool = redis.ConnectionPool({
	host='localhost',
	port=6379,
	password='123456',
	db=0,
	max_connections=20
})
```

### 创建和关闭链接

从连接池中获取的链接，不必关闭，垃圾回收的时候，链接会自动被归还到连接池：

```
r = redis.Redis(
	connection_pool=pool
)
...
del r
```

### 操作指令

字符串操作：

```py
r.set('country', '英国')
r.set('city', '伦敦')
city = r.get('city').decode('utf-8')
print(city)
```

```
r.delete('country', 'city')
r.mset({'country': '德国', 'city': '柏林'})
result = r.mget('country', 'city')
for one in result:
	print(one.decode('utf-8'))
```

列表操作：

```
r.rpush('dname', '董事会', '秘书处', '财务部', '技术部')
r.lpop('dname')
result = r.lrange('dname', 0, -1)
for one in result:
	print(one.decode('utf-8'))
```

集合和有序集合操作：

```
r.sadd('employee', 8001, 8002, 8003)
r.srem('employee', 8001)
result = r.smembers('employee')

r.zadd('keyword', {'马云': 0, '张朝阳': 0, '丁磊': 1})
r.zincrby('keyword', '10', '马云')
result = r.zrevrange('keyword', 0, -1)
```

哈希操作：

```py
r.hmset('9527', {'name': 'Scott', 'sex': 'male', 'age': 35})
r.hset('9527', 'city', '纽约')
r.hdel('9527', 'age')
r.hexists('9527', 'name')
result = r.hgetall('9527')
```

### redis-py 的事务函数

`redis-py` 模块用 `pipeline` （管道）的方式向 Redis 服务器传递批处理命令和执行事务：

```py
pipline = con.pipeline()
pipline.watch(...)
pipline.multi()
pipline.execute()
pipline.reset()
```

### 练习一

用 Python 程序模拟 300 名观众，为 5 位嘉宾（马云、丁磊、张朝阳、马化腾、李彦宏）随机投票，最后按照降序排序结果：

```py
# redis_db
import redis

try:
    pool = redis.ConnectionPool(
        host='localhost',
        port=6379,
        password='123456',
        db=0,
        max_connections=20
    )
except Exception as e:
    print(e)

```



```py
from redis_db import pool
import redis
import random

con = redis.Redis(
    connection_pool=pool
)

try:
    con.delete('ballot')
    con.zadd('ballot', {'马云': 0, '丁磊': 0, '张朝阳': 0, '马化腾': 0, '李彦宏': 0})
    names = ['马云', '丁磊', '张朝阳', '马化腾', '李彦宏']
    for i in range(300):
        num = random.randint(0, 4)
        name = names[num]
        con.zincrby('ballot', 1, name)
    result = con.zrevrange('ballot', 0, -1, True)
    for one in result:
        print(one[0].decode('utf-8'), int(one[1]))
except Exception as e:
    print(e)
finally:
    del con

```



