# 数据存储之csv

> 使用方法

* writer(fp)
* writerow()    写入一行
* writerows()  写入多行

> 基础使用方法

```python
import csv

title = ['Name', 'Number']
data = [
    ['Class 1', 73],
    ['Class 2', 43],
    ['Class 3', 56],
    ['Class 4', 53]
]

fp = open('csv_data.csv', 'w')
writer = csv.writer(fp)
writer.writerow(title)
writer.writerows(data)
fp.close()
```