# 数据解析之Xpath

[TOC]

## 安装 `lxml` 库

`pip install lxml`

## Xpath语法

- 根节点：`/`
- 跨节点：`//`
- 获取标签：`//tag[@attr="attr_value"]` eg: `//a[@data-num='9']`
- 获取标签里的内容: `text()`
- 获取标签的属性：`@attr_name` eg: `@href`

## 通过Xpath获取360搜索的相关信息

```python
import re, requests
from lxml import etree

url = 'http://www.so.com'
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Safari/537.36'
}
response = requests.get(url, headers=headers)
html = response.content.decode()
xpath_data = etree.HTML(html)
title = xpath_data.xpath('/html/head/title/text()')
key_words = xpath_data.xpath('/html/head/meta[@name="keywords"]/@content')[0]
nav_links = xpath_data.xpath('//header/section/nav/a/@href')
nav_name = xpath_data.xpath('//header/section/nav/a/text()')
```