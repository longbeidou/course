# 数据解析之Beautiful Soup

[TOC]

## 文档地址

[https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/)

## 安装 Beautiful Soup 4

`pip install beautifulsoup4`

## 安装解析器

`pip install lxml`

`pip install html5lib`

## 解析器对比

| 解析器           | 使用方法                                                     | 优势                                                  | 劣势                                            |
| ---------------- | ------------------------------------------------------------ | ----------------------------------------------------- | ----------------------------------------------- |
| Python标准库     | `BeautifulSoup(markup, "html.parser")`                       | Python的内置标准库执行速度适中文档容错能力强          | Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差 |
| lxml HTML 解析器 | `BeautifulSoup(markup, "lxml")`                              | 速度快文档容错能力强                                  | 需要安装C语言库                                 |
| lxml XML 解析器  | `BeautifulSoup(markup, ["lxml-xml"])`  <br />`BeautifulSoup(markup, "xml")` | 速度快唯一支持XML的解析器                             | 需要安装C语言库                                 |
| html5lib         | `BeautifulSoup(markup, "html5lib")`                          | 最好的容错性以浏览器的方式解析文档生成HTML5格式的文档 | 速度慢不依赖外部扩展                            |

## 对象的种类

* Tag：与XML或HTML原生文档中的tag相同
* NavigableString：Tag中的字符串内容形式
* BeautifulSoup：一个文档的全部内容
* Comment：一个特殊类型的 `NavigableString` 对象，文档中的备注类型，其输出的内容不包括注释符号。

## 遍历文档树

* 子节点

  .comtents	获取其直接子节点，并返回一个列表

  .children	获取其直接子节点的可遍历对象

  .descendants	获取其子，子的子。。的可遍历对象

* 父节点

  .parent	某一个节点的父节点

  .parents	某一个节点的可遍历父节点集

* 兄弟节点

  .next_sibling	获取下一个兄弟节点

  .previous_sibling	获取前一个兄弟节点

  .next_sibings	所有该节点的下一个兄弟节点的遍历对象

  .previous_siblings	该节点的所有前面兄弟节点的遍历对象

* 输出字符串

  .string	当标签下面只有一个字符串时，正常输出字符串内容，否则输出none

  .strings	当tag下面有多个字符串时，当前标签下面的所有字符串的遍历对象

  .stripped_strings	当前标签下面的所有字符串（去除空格和空行）的遍历对象

## 搜索文档树

* find()与find_all()的区别

  find直接返回找到的第一个结果，如果没有结果的话是NONE。

  find_all是返回符合条件的tag的list，如果没有结果的话输出空的list。

* find_all( [name](https://links.jianshu.com/go?to=https%3A%2F%2Fbeautifulsoup.readthedocs.io%2Fzh_CN%2Fv4.4.0%2F%23id35) , [attrs](https://links.jianshu.com/go?to=https%3A%2F%2Fbeautifulsoup.readthedocs.io%2Fzh_CN%2Fv4.4.0%2F%23css) , [recursive](https://links.jianshu.com/go?to=https%3A%2F%2Fbeautifulsoup.readthedocs.io%2Fzh_CN%2Fv4.4.0%2F%23recursive) , [string](https://links.jianshu.com/go?to=https%3A%2F%2Fbeautifulsoup.readthedocs.io%2Fzh_CN%2Fv4.4.0%2F%23id36) , [**kwargs](https://links.jianshu.com/go?to=https%3A%2F%2Fbeautifulsoup.readthedocs.io%2Fzh_CN%2Fv4.4.0%2F%23keyword) )

  name：查找所有名字符合name的tag，参数可以是所有的过滤器

  attrs：指定的tag属性attrs={"id":"link3"},传递一个字典进去

  recursive：确定是否只搜索直接子节点，如果只想搜索直接子节点，=false

  string：需要查找的字符串内容，可以是字符串/列表/正则/true

  keyword：需要搜索的tag属性。

  limit：设定需要查找的数量。

* select() // CSS选择器

## 使用案例

```python
from bs4 import BeautifulSoup
import requests, re

url = 'https://www.runoob.com/'
haders = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.113 Safari/537.36'
}
r = requests.get(url)
html = r.content.decode()
soup = BeautifulSoup(html, 'lxml')
title_tag = soup.head.title
title_text = soup.head.title.string
meta_first_tag = soup.meta
meta_first_tag_name = soup.meta.name
meta_first_tag_attrs = soup.meta.attrs
head_contents = soup.head.contents
a_list = soup.find_all('a', title='菜鸟教程')
a_attr_list = soup.find_all(attrs={'data-id': 'index'})
a_text_list = soup.find_all(text=re.compile('菜鸟'))
css_select = soup.select('#footer')
result_text = soup.select('#footer a')[-1].get_text()
result_attr = soup.select('#footer a')[-1].get('href')
```