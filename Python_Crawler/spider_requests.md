# 通过第三方的requests实现发送HTTP请求的功能

> 安装requests

`pip install requests`

> requests 地址

[点击直达](https://requests.readthedocs.io/en/latest/)

> 通过requests发送get请求

```python
import requests

url = 'http://www.baidu.com/s'
params = {
    'wd': 'Python 教程'
}
header = {
'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}

response = requests.get(url, headers=header, params=params)
content = response.content.decode()
request_header = response.request.headers
request_cookie = response.request._cookies
response_header = response.headers
response_code = response.status_code
response_cookie = response.cookies

with open('spider_requests.html', 'w', encoding='utf-8') as f:
    f.write(content)

print(request_cookie)
print(request_header)
print(response_header)
print(response_code)
print(response_cookie)
```

> 通过requests发送post请求

```python
import requests

url = 'http://localhost:8080/api/callEvent'
data = {'name': 'test'}
response = requests.post(url, data)
print(response.content.decode())
print(response.request.body)
print(response.request.url)
```

> 通过requests获取返回的json内容

```python
import requests

url = 'https://api.github.com/user'
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
response = requests.get(url, headers=headers, timeout = 1)
json_data = response.json()
print(json_data)
```

> 通过免费的IP代理来实现HTTP请求

```python
import requests

url = 'http://www.baidu.com/s'
params = {'wd': 'Python 教程'}
header = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
free_proxy = {'http': '123.139.56.238:9999'}
response = requests.get(url, headers=header, proxies=free_proxy)
print(response.status_code)
```

> 忽略SSL证书

设置verify为False，表示不是CA证书，是自己颁发的证书

```python
header = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
url = 'https://www.12306.cn/index/'
response = requests.get(url, headers=header, verify=False)
print(response.content.decode())
```

> 通过Cookie实现自动登录

```python
import requests

header = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
login_url = 'http://www.longbeidou.cn/zb_system/cmd.php?act=verify'
content_url = 'http://www.longbeidou.cn/zb_system/admin/index.php?act=ArticleMng'
login_form_data = {
    'btnPost': '登录',
    'username': 'longbeidou',
    'password': 'f44892a4ac0561c3dfc6098770e14dab',
    'savedate': 1
}
session = requests.session()
login_response = session.post(login_url, data=login_form_data, headers=header)

with open('dashboard.html', 'w', encoding='utf-8') as f:
    f.write(login_response.content.decode())

content_response = session.get(content_url, headers=header)

with open('content.html', 'w', encoding='utf-8') as f:
    f.write(content_response.content.decode())
```