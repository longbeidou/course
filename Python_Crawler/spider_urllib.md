# 通过urllib实现发送HTTP请求的功能

## 通过urllib实现好搜网页搜索结果的抓取

```python
import urllib.request
import urllib.parse
import string

# 在URL中存在中文的处理方式
so_url = 'http://www.so.com/s?q=' + 'Python教程'
headers = {
'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
encode_so_url = urllib.parse.quote(so_url, safe=string.printable)
so_response = urllib.request.urlopen(encode_so_url)
so_content = so_response.read().decode('utf-8')

with open('so_result.html', 'w', encoding='utf-8') as f:
    f.write(so_content)
```

## 通过cookie实现自动登录

```python
import urllib.request
import http.cookiejar
import urllib.parse
import string

# 通过Cookie自动登录
login_url = 'http://www.your_website.com/login'
login_form_data = {
    'username': 'your_name',
    'password': 'your_password',
}

cookie_jar = http.cookiejar.CookieJar()
cookie_handler = urllib.request.HTTPCookieProcessor(cookie_jar)
opener = urllib.request.build_opener(cookie_handler)
headers = {
'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.67 Safari/537.36'
}
encode_form_data = urllib.parse.urlencode(login_form_data).encode('utf-8')
login_request = urllib.request.Request(login_url, headers=headers, data=encode_form_data)
login_response = opener.open(login_request)
login_content_str = login_response.read().decode('utf-8')

with open('login_response.html', 'w', encoding='utf-8') as f:
    f.write(login_content_str)

user_agent = login_request.get_header('User-agent')
request_headers = login_request.headers
response_header = login_response.headers
full_url = login_request.full_url


content_url = 'http://www.longbeidou.cn/zb_system/admin/index.php?act=ArticleMng'
content_request = urllib.request.Request(content_url, headers=headers)
content_response = opener.open(content_request)

with open('admin_page.html', 'wb') as f:
    f.write(content_response.read())

print(content_response)
```

## 处理异常

```python
import urllib.request
from urllib.error import URLError, HTTPError, ContentTooShortError

url = 'http://www.baidu666.com/'

try:
    response = urllib.request.urlopen(url, timeout=5)
    print(response.geturl())
    print(response.info())
    print(response.getcode())
except AttributeError as e:
    print('AttributeError', e)
except ContentTooShortError as e:
    print('ContentTooShortError', e)
except HTTPError as e:
    print('HTTP Error:', e)
except URLError as e:
    print('URL Error:', e)
finally:
    print('This is final')
```