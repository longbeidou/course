# 数据存储之MySQL

[TOC]

## 查看版本号

```bash
select version();
```

## 查看当前时间

```bash
select now();
```

## 查看当前数据库

```bash
select database();
```

## 视图

对于复杂查询的封装

* 创建视图

  ``` ba
  create view view_name as sql
  ```

* 查看视图的结果

  ``` bash
  select * from view_name
  ```

* 删除视图

  ``` bash
  drop view view_name
  ```

## 索引

 * 创建索引

   ```bash
   create index index_name on tables_name(filed_name(length));
   ```

 * 查看索引

   ```bash
   show index from table_name;
   ```

 * 删除索引

   ```bash
   drop index index_name on table_name;
   ```

## Python和MySQL的交互

### 安装pymysql

```bash
pip install pymysql
```

### 常用方法

* 连接数据库

  ```py
  connector = pymysql.Connect(host='', port=3306, db='', user='', passwd='', charset='utf8')
  ```

  

* 创建游标对象

  ```base
  cursor = connector.cursor()
  ```

  

* 执行sql语句

  ```base
  result = cursor.execute(sql)
  ```

  

* 获取结果

  ```base
  result = cursor.fetchall()
  result = cursor.fetchone()
  ```

  

* 关闭游标

  ```base
  cursor.close()
  ```

  

* 关闭连接

  ```base
  connector.close()
  ```

  

* 提交事务

  ```base
  connector.close()
  ```

  

* 执行事务回滚

  ```base
  connector.rollback()
  ```

  

### 与Python交互的code

```python
import pymysql

try:
    conn = pymysql.Connect(host='localhost', port=3306, db='test_db', user='root', passwd='root', charset='utf8')
    cur = conn.cursor()

    insert_sql = "insert into student(name, age, score, class) value('xiao wang', 29, 38, 1)"
    insert_count = cur.execute(insert_sql)

    update_sql = "update student set age = 99 where id < 8"
    update_count = cur.execute(update_sql)

    select_sql = "select * from student"
    select_count = cur.execute(select_sql)

    result_one = cur.fetchone()
    result_all = cur.fetchall()
    result_many = cur.fetchmany(3)

    delele_sql = "delete from student where id > 9998"
    delete_count = cur.execute(delele_sql)

    conn.commit()
    cur.close()
    conn.close()
except Exception as e:
    conn.rollback()
```

