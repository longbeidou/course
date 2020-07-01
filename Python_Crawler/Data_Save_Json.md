# 数据存储之json

> 处理字符串

* loads() 	将字符串转变为json
* dumps()  将json转变为字符串

> 处理文件对象

* load()	  将文件转变为json
* dump()   将json转变为文件

> 基础使用方法

```python
import json

j = {"name": "Ryan", "num": 99}
j_str = '{"name": "Ryan", "num": 99}'
print(json.dumps(j))
print(json.loads(j_str))

with open('json_data.json', 'w') as fp:
    json.dump(j, fp)
r = json.load(open('json_data.json', 'r'))
print(r)
```