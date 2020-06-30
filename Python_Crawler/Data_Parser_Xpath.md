# 数据解析之Xpath

> 安装 `lxml` 库

`pip install lxml`

> Xpath语法

- 根节点：`/`
- 跨节点：`//`
- 获取标签：`//tag[@attr="attr_value"]` eg: `//a[@data-num='9']`
- 获取标签里的内容: `text()`
- 获取标签的属性：`@attr_name` eg: `@href`

