# 数据存储之MongoDb

> 端口号

​	27017

> 启动数据库

* 启动服务端
  * mongod
  * service mongodb start
  * service mongodb stop
* 启动客户端
  * mongo
* 设置权限
  * mongod -auth
    * use admin
  * 查看所有用户
    * show users
  * 创建用户
    * db.createUser({user: "user_name", pwd: "your_pwd", roles: ["root"]})
  * 删除用户
    * db.dropUser('user_name')

> 数据库的操作

* 查看所有数据库
  * show dbs
* 切换数据库
  * use db_name
* 查看当前数据库
  * db
* 删除数据库
  * user db_name
  * db.dropDatabase()

> 集合的操作

* 查看所有集合
  * show collections

* 创建集合
  * db.createCollection()
* 删除集合
  * db.collection_name.drop()

> 文档内容操作

* 增加
  * db.collection_name.insert()
* 删除
  * db.collection_name.remove()

* 修改
  * db.collection_name.update({查询条件}, {$set: 修改的内容}, {multi: true})
  * 默认只修改符合条件的第一个
* 查询
  * 基本查询
    * db.collection_name.find()	查询所有
    * db.collection_name.find({条件}})    查询部分
    * 比较运算
      * $lt:    less than
      * $lte:  less than equal
      * $gt:   greater than
      * $gte: greater than equel
      * $ne:  not equal
    * 逻辑运算
      * $and
      * $or
    * 范围运算
      * in
      * nin
    * 正则表达式
      * /....../
      * $regex
      * 忽略大小写
        * /...../i
        * $option: "i"
    * 定义函数
  * 复合查询（aggregate()	管道）
    * $group
      * eg:  db.collection_name.aggregate([$group: {_id: "$filed_name"}])
      * $avg / $sum / $max / $min / $first / $last / $push
    * $match     筛选数据
    * $project    投影
    * $sort
      * 1：升序
      * -1：降序
    * $skip
    * $limit
    * $unwind    拆分文档
  * 查询结果显示
    * limit	限制个数
    * skip
    * 投影
      * 1：字段显示
      * 0：字段不显示
    * sort
      * 1：升序
      * -1：降序
    * count    统计个数
      * db.collection_name.find().count()
      * db.collection_name.count({条件})
    * distinct
      * db.collection.distinct('filed_name', {条件})

> 索引查询

* 查看所有索引
  * getIndexes()
* 执行索引
  * explain('executionStats')
* 自定义索引
  * db.sy.ensureIndex({name: 1})
* 删除索引
  * 删除索引
    * dropIndex("name_1")

> 备份和恢复

* mongodump备份数据库

  * 命令格式

    mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表 -o 文件存放路径

  * 导出所有数据库

    mongodump -o /data/mongobak/

  * 导出指定数据库

    mongodump -d database_name -o /data/mongobak/database_name.bak

* mongorestore恢复数据库

  * 命令格式

    mongorestore -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 --drop 文件存在路径

    --drop：先删除所有的记录，然后恢复.

  * 恢复所有的数据库

    mongorestore /data/mongobak/

  * 恢复指定的数据库

    mongorestore -d database_name /data/mongobak/database_name.bak

* mongoexport导出集合

  * 命令格式

    mongoexport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 -f 字段 -q 条件导出 --csv -o 文件名

    -f 导出指定字段，以逗号分割，-f uid,name,age导出uid,name,age这三个字段
    -q 可以根据查询条件导出，-q '{ "uid" : "100" }' 导出uid为100的数据
    --csv 表示导出的文件格式为csv的。

  * 导出整张表

    mongoexport -d database_name -c collection_name -o /collection_path/collection_name.dat

  * 导出集合中的部分字段

    mongoexport -d database_name -c collection_name --csv -f uid,name,age -o /collection_path/name.csv

* mongoimport导入集合

  * 命令格式

    * 恢复整表导出的非csv文件

      mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --upsert --drop 文件名

      --upsert:插入或者更新现有数据

    * 恢复部分字段的导出文件

      mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --upsertFields 字段 --drop 文件名

      --upsertFields:更新部分的查询字段，必须为索引,以逗号分隔.

    * 恢复导出的csv文件

      mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --type 类型 --headerline --upsert --drop 文件名

      --type：导入的文件类型（默认json）

> 与Python的交互

* 安装pymongo

  ```bash
  pip install pymongo	
  ```

* 官方文档

  [https://api.mongodb.com/python/3.4.0/index.html](https://api.mongodb.com/python/3.4.0/index.html)

* 连接数据库

  client = pymongo.MongoClient()

* 创建数据库

  * db = client.dbName
  * db = client['db-name']

* 创建集合
  * collection = db.collectionName
  * collection = db['collection-name']
* 增加数据
  * insert_one
  * insert_many
* 删除数据
  * delete_one
  * delete_many
* 修改数据
  * update_one
  * update_many
* 查询数据
  * find
  * find_one

> 控制台Code

```bash
db.stu.find({
    $where: function() {
        return this.age > 20
    }
})

db.stu.find({ age: {$gt: 20} })
db.stu.remove({ name: "A" })
db.stu.update( {name: "小丽"}, {$set: {name: "小丽丽丽"}}, {multi: true})

db.stu.aggregate([
	{$match:{age:{$lt:50}}},
	{$group:{_id:"$gender", sumage:{$sum:"$age"}, avgage:{$avg:"$age"}, allName: {$push: "$name"} }},
    {$sort: {avgage: 1}},
	{$project:{sumage:true, allName: true}},
    {$unwind: "$allName"},
    {$limit: 8},
    {$skip: 5}
])

for (var i = 0; i <= 500000; i++) {
    db.data.insert({ _id: i, user: "user" + i, age: i})
}

db.data.find({user: 'user99983'}).explain('executionStats')
db.data.ensureIndex({user: 1})
db.data.dropIndex('user_1')
```

> 与Python交互的Code

```python
import pymongo
from pprint import pprint

try:
    mongo_client = pymongo.MongoClient(host="127.0.0.1", port=27017, username="Ryan", password="123456")
    dbs = mongo_client.list_database_names()

    for db_name in dbs:
        # print(db_name)
        pass

    new_db = mongo_client['test']
    new_collection = new_db['student']

    # 插入数据
    stu_one = {"name": 'AAA', "age": 23, "score": 83}
    stu_many = [
        {"name": 'BBB', "age": 43, "score": 83},
        {"name": 'CCC', "age": 13, "score": 74},
        {"name": 'DDD', "age": 23, "score": 456},
        {"name": 'EEE', "age": 21, "score": 234},
        {"name": 'FFF', "age": 55, "score": 1},
    ]
    inserted_id = new_collection.insert_one(stu_one).inserted_id
    inserted_ids = new_collection.insert_many(stu_many).inserted_ids

    # 查询数据
    query = {"age": {"$gt": 15}}
    filed_list = { "age": 1, "score":1 }
    query_one = new_collection.find_one(query, filed_list)
    # print(query_one)
    query_many = new_collection.find(query, filed_list)

    for info in query_many:
        # pprint(info)
        pass

    # 修改数据
    query = { "name": "AAA" }
    new_value = { "$set": { "age": 999 }}
    modified_count_one = new_collection.update_one(query, new_value).modified_count
    modified_count_many = new_collection.update_many(query, new_value).modified_count

    # 删除数据
    query = { "name": "BBB" }
    deleted_count_one = new_collection.delete_one(query).deleted_count
    deleted_count_many = new_collection.delete_many(query).deleted_count
except Exception as e:
    print(e)
finally:
    mongo_client.close()
```