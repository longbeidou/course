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
  * db.dropCollection()

> 文档内容操作



> 索引查询

* 查看所有索引
  * getIndexs()
* 执行索引
  * explain('executionStats')
* 自定义索引
  * db.sy.ensureIndex({name: 1})
* 删除索引
  * 删除索引
    * dropIndex("name_1")

> 备份和恢复



> 与Python的交互



