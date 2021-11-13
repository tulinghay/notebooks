### String

```sh
#切换库
select 1#切换到1号库
select 3#切换到3号库

# 设置k1的值为lucy
set k1 lucy
#查看所有的key
keys *
#判断k1是否存在
exists k1
# 查看k1的类型
type k1
#删除k1
del k1
unlink k1 #也是删除，是非阻塞删除
#设置k1的10秒钟的过期时间
expire k1 10
#查看k1的剩余多久过期，-1表示永不过期
ttl k1
#查看当前库里面有多少个key
dbsize
#清空当前库
flushdb
#通杀全部库
flushall
#取k1的value
get k1
# 追加，将12追加到k1的value后面
append k1 12
#获得k1的value长度
strlen k1
#当k1不存在时设置为123
setnx k1 123

# 将k3中的值加1，只能对数字操作，如果为空则新增值为1   原子操作
incr k3
# 将k3中的值减1，只能对数字操作      原子操作
decr k3
#定义增步长，这里加10
incrby k4 10
#定义减步长，这里减10
decrby k4 10

#mset设置多个kv
mset k1 v1 k2 v3 k3 v3
#mget获取多个k的v
mget k1 k2 k3
#mset设置多个kv，如果里面有键已经存在则此条命令执行失败，返回0
msetnx k11 v11 k12 v12 k1 v1
# 获取k1的v的0-3索引，长度为4
getrange k1 0 3
#创建k1并设置过期时间   setex <key> <过期时间> <value>
setex k1 20 v3
#设置新值，并返回旧值
getset k1 v1


```

redis扩容机制：当分配的空间小于1m时，每次扩容都是加倍扩容，如果大于1m，则后面的扩容每次扩充1m，字符串最大长度不超过512m

### List

双向链表

```sh
#从左边/右边插入多个kv
lpush/rpush k1 v1 k2 v2   
#从左边/右边吐出一个值
lpop/rpop k1
#从k1 链表中吐出一个值插入到k2的左边
rpoplpush k1 k2 
#按照索引下标获得元素（从左到右）
lrange k1 0 -1
#0左边第一个，-1右边第一个，（0-1表示获取所有）
lrange mylist 0 -1
#按照索引下标获取元素，从左到右
lindex k1 1
#获取列表长度
llen k1
#从value的前面插入newvalue    linsert <key> before/after <value> <newvalue>
linsert k1 before/after v1 v2
# 从左边删除n个value，从左到右   lrem <key> <n> <value>
lrem k1 1 v1 
#将列表下标为index的值替换成value
lset <key> <index> <value>

```

### Set

```sh
#将一个或多个member元素加入到集合key中，已经存在的元素被忽略
sadd <key> <value1> <value2> ...
#取出该集合的所有值
smembers <key>
#判断k1中是否存在v1，不存在返回0，存在返回1
sismember k1 v1
#返回该集合的元素个数
scard <key>
#删除k1中的v1和v2
srem k1 v1 v2
#随机从k1中吐出值
spop k1
#随机从集合中取出n个值，不会删除
srandmember k1 n
#从k1中的value值移动到k2
smove k1 k2 value
#返回两个集合的交集元素
sinter k1 k2 
#返回两个集合的并集元素
sunion k1 k2
#返回两个集合的差集元素
sdiff k1 k2 

```

### Hash

```sh
#设置user的id属性
hset user:1001 id 1 
#设置user的name属性
hset user:1001 name huangda
hget user:1001 id
hget user:1001 name
#批量设置hash的属性值
hmset user:1002 id 2 name lisi
#看user:1002中是否存在id1这个域
hexists user:1002 id1
#列出该hash集合的所有field
hkeys user:1002
#列出该hash的所有value
hvals user:1002 
#把id这个域的值+1
hincrby user:1002 id 1
#当id这个域不存在时，新建将其设置为1，存在则返回0
hsetnx user:1002 id 1
```

### Zset

```sh
#将一个或多个元素及其score值加入到有序集中
zadd topn 200 java 300 c++ 400 mysql 500 php
#取出topn中的所有元素
zrange topn 0 -1
#返回所有元素，包括评分
zrange topn 0 -1 withscores
#取出topn中评分在300-500的值
zrangebyscore topn 300 500
#取出topn中评分在300-500的值，withscores包含分数
zrangebyscore topn 300 500 withscores
#评分500-200倒序输出
zrevrangebyscore topn 500 200 
#给topn中的java评分+50
zincrby topn 50 java
#删除topn中的php
zrem topn php 
#统计200 - 300有几个值
zcount topn 200 300
#查看topn中java的排名
zrank topn java
#带分数展示所有元素
zrange topn 0 -1 withscores


```

### 发布订阅

```bash
#订阅频道
SUBSCRIBE channel1
#频道信息推送
publish channel1 messagecontent
```

## 新数据类型

### Bitmaps本身不是数据类型，只是一个字符串

见视频：https://www.bilibili.com/video/BV1Rv41177Af?p=15&spm_id_from=pageDriver

```sh
#设置第1位是1
setbit user:2020 1 1
#设置第5位是1
setbit user:2020 5 1

#获取第五位的值
getbit user:2020 5
#获取整个user:2020的1数量
bitcount user:2020 


bittop and/or/not/xor   复合操作
eg: 给user:2020和user:2021的1 9都设置为1，这个时候并集查询
	setbit user:2020 1 1
	setbit user:2020 5 1
	setbit user:2020 9 1
	setbit user:2020 5 1
	setbit user:2020 9 1
	
```

### HyperLogLog类似于元组，元素唯一

### Geospatial 保存地理位置的

```bash
geoadd china:city shanghai 12.12 12.11
#获取两个的直线距离
geodist china:city shanghai beijing  
#获取半径内的元素
georadius
```



