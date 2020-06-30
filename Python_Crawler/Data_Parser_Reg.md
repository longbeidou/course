# 数据解析之正则表达式

> 常见的元字符列表

| 元字符 | 含义                                                        |
| ------ | ----------------------------------------------------------- |
| `\`    | 转义字符，将后一个字符标记为特殊字符或将元字符转为原意字符  |
| `.`    | 匹配除换行(\n)以外的所有字符                                |
| `^`    | 匹配字符串的开始位置，在集合(`[]`)中表示“非”                |
| `$`    | 匹配字符串的结束位置                                        |
| `?`    | 匹配前面子表达式0次或一次                                   |
| +      | 匹配前面子表达式至少出现一次                                |
| `*`    | 匹配前面子表达式0次或多次                                   |
| `()`   | 标记一个子表达式的开始和结束位置，其结束符号“)”**是**元字符 |
| `[`    | 字符组的起始符号，其结束符号“]”不是元字符                   |
| `{`    | 标记限定符的开始，其结束符号“}”不是元字符                   |
| `|`    | 表示“或”                                                    |

> 常用表达式列表

| 表达式  | 含义                                                     |
| ------- | -------------------------------------------------------- |
| `\w`    | 匹配字母、数字、下划线                                   |
| `\W`    | 匹配非字母、非数字、非下划线                             |
| `\d`    | 匹配数字                                                 |
| `\D`    | 匹配非数字                                               |
| `\b`    | 匹配单词的开始或结束                                     |
| `\B`    | 匹配非单词的开始或结束                                   |
| `\s`    | 匹配任何空白字符，如回车、空格、制表符等                 |
| `\S`    | 匹配任何非空白字符                                       |
| `{n}`   | 匹配前面子表达式n次                                      |
| `{n,m}` | 匹配前面子表达式n到m次                                   |
| `{n,}`  | 匹配前面子表达式n次以上                                  |
| `[xyz]` | 表示字符集，匹配所包含的任意一个字符                     |
| `[a-z]` | 表示字符范围，能匹配范围内的任意一个字符                 |
| `(abc)` | 组合，将几个项组合成为一个单元，可以对这个单元使用限定符 |

> 汉字的表达范围

`[\u4e00-\u9fa5]`

> 运算符优先级

| 运算符                           | 优先权 | 说明                           |
| -------------------------------- | ------ | ------------------------------ |
| `\`                              | 最高   | 转义字符                       |
| `()` `(?:)` `(?=)` `[]`          | 高     | 圆括号和方括号                 |
| `*` `+` `?` `{n}` `{n,}` `{n,m}` | 中     | 限定符                         |
| `^` `$` `\任何元字符` `任何字符` | 低     | 定位点和序列（即：位置和顺序） |
| `|`                              | 最低   | 选择符“或”                     |

> findall：匹配所有

```python
import re

str = 'abbbbb'
pattern = re.compile('a(.*)')
r = pattern.findall(str)
print(r)
# ['bbbbb']
pattern = re.compile('a(.*?)')
r = pattern.findall(str)
print(r)
# ['']
```

> match：从头开始匹配，只匹配一次

```python
import re

pattern = re.compile('^\d+$')
r = pattern.match('2343t')
print(r)
# None
r = pattern.match('2343')
print(r.group())
# 2343
```

> search：从任意位置开始匹配

```python
import re

pattern = re.compile('\w+')
r = pattern.search('asdf**sldfj*230')
print(r.group())
# asdf
r = pattern.search('*****')
print(r)
# None
```

> sub：替换字符串

```python
import re

pattern = re.compile('\d+')
r = pattern.sub('*', '333sldkjf999')
print(r)
# *sldkjf*
```

> split：字符串拆分

```python
import re

pattern = re.compile('\*\*')
r = pattern.split('aaaa**bbbb***ccc')
print(r)
# ['aaaa', 'bbbb', '*ccc']
```

> 查找抓取网页的所有a链接的地址

```python
import re
import requests

url = 'http://www.so.com/'
r = requests.get(url).content.decode()
all_data = re.compile('<a href="(.*?)" data-linkid="(\d+)" target="_blank">').findall(r)
all_a = [info[0] for info in all_data]
print(all_a)
```