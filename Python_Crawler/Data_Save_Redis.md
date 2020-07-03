# 数据存储之Redis

[TOC]

## 端口号

`6379`

## 启动redis

* 服务端

  ```bash
  redis-server
  ```

* 客户端

  ```bash
  redis-cli
  ```

## 数据操作

### 切换数据库

​	`select num`

### string

* 增加	 `set`   /   `mset`  /   `set key time value`
* 读取     `get`   /   `mget`
* 追加     `append key value`

### hash

* 存单个属性		`hset key name value`

* 存储多个属性    `hset key name1 value1 name2 value`

* 读取信息

  `hkeys key`  

  `hget key name`    /   `hmget key1 name1 name2`

  `hvals key` 

  `hdel key name`

### list

* 增加	`lpush key`   /    `rpush key`

* 弹出    `lpop`   /   `rpop`

* 读取    `lrange key start end`

* 指定位置插入    

  `linsert key before ori_value value`

  `linsert key after ori_value value`

* 根据索引值修改    `lset key index value`

* 删除

  `lrem key count value`

  * count > 0 从头删除
  * count < 0 从尾删除
  * count = 0 符合条件的全部删除

### set

* 增加	`sadd`
* 查看    `smembers key`
* 删除    `srem key value...`
* 判断是否在集合中    `sismenber`

### zset

* 增加	`zadd key score1 value [score2 value]...`

* 查看

  * 根据范围查看   	`zrange key 0 -1`
  * 根据权重查看       `zrangebyscore key score1 score2`
  * 根据值获取权重    `zscore key value`

* 删除

  `zrem key value`   /   `zremrangebyscore key score1 score2`

### key

​	`keys *`    /   `keys kk*`  

​	`exsits key`  

​	`type key`  

​	`del key`   

​	`expire key time`

### 清空数据库

​	清空当前数据库    `flushdb`

​	清空所有数据库    `flushall`

## Python和Redis的交互

### 安装redis包

```bash
pip install redis
```

### 常用的使用方法

* 创建客户端

  client = redis.StrictRedis(host="", port="")

* 增加 / 修改

  client.set(key, value)

* 修改

  client.delete(key)

* 查看

  * value = client.get(key)
  * keys = client.keys()

### 案例Code

```python
import redis

client = redis.StrictRedis(host="127.0.0.1", port=6379, db=1)

# 操作string
is_true = client.set(name='str', value="This is String.")
str_length = client.append('str', '****')
str_value = client.get('str')

# 操作hash
add_count = client.hset('student', key="name", value="lili")
add_count = client.hset("student", mapping={"age": 30, "score": 99})
key_list = client.hkeys('student')
value_list = client.hvals('student')
del_count = client.hdel('student', 'age')

# 操作list
add_count = client.rpush('queue', 'aaa', 'bbb', 'ccc', 'ddd')
res_list = client.lrange('queue', 0, -1)
pop_value = client.lpop('queue')
list_length = client.linsert('queue', where='before', refvalue='bbb', value="****")
del_count = client.lrem('queue', count=-1, value="ccc")

# 操作set
add_count = client.sadd('pool', 'aaa', 'aaa', 'bbb', 'ccc', 'ddd')
all_dict = client.smembers('pool')
del_count = client.srem('pool', 'aaa')
is_true = client.sismember('pool', 'aaa')

# 操作zset
add_count = client.zadd('zpool', mapping={'aaa': 93, 'bbb': 932, 'ccc': 99})
all_list = client.zrange('zpool', 0, -1)
part_list = client.zrangebyscore('zpool', 10, 100, withscores=True)
del_count = client.zrem('zpool', 'aaa')
del_count = client.zremrangebyscore('zpool', 500, 1000)

keys_list = client.keys()
client.delete('str')
client.expire('str', 10)
client.exists('str')
key_type = client.type('str')
```

