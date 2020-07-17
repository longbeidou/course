# Selinum使用案例

[toc]

## 百度搜索

```python
from selenium import webdriver
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as ec

option = webdriver.FirefoxOptions()
option.headless = True
driver = webdriver.Firefox(options=option, executable_path='./webdriver/firefox.exe')
baidu_url = 'http://www.baidu.com/'
driver.get(baidu_url)
print('Page title: ', driver.title)

search_input = WebDriverWait(driver, 10).until(
    # lambda d: d.find_element_by_id('kw')
    ec.presence_of_element_located((By.XPATH, '//input[@id="kw"]'))
)
search_input.send_keys('Python 文档')
submit_btn = WebDriverWait(driver, 10).until(lambda d: d.find_element_by_id('su'))
submit_btn.click()

search_result = WebDriverWait(driver, 10).until(
    lambda d: d.find_elements_by_xpath('//h3[contains(@class,"t")]/a[1]'))
res = []
for item in search_result:
    text = item.text
    link = item.get_attribute('href')
    print(text)
    print(link)
    res.append((text, link))
print('Count: ', len(res))

driver.close()
driver.quit()
```



## 微信公众号文章搜索

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
import time

url = 'https://weixin.sogou.com/'
query_str = 'Python'
options = webdriver.FirefoxOptions()
# options.headless = True
with webdriver.Firefox(executable_path='./webdriver/firefox.exe', options=options) as driver:
    driver.get(url)
    print('Page title:', driver.title)
    input_query = WebDriverWait(driver, 10).until(lambda d: d.find_element_by_id('query'))
    input_query.send_keys(query_str)
    submit_btn = driver.find_element_by_xpath('//input[@uigs="search_article"]')
    submit_btn.click()

    result = WebDriverWait(driver, 10).until(
        lambda d: d.find_elements_by_xpath('//div[@class="news-box"]/ul/li/div[@class="txt-box"]/h3/a'))

    pages_list = []
    for item in result:
        pages_list.append({
            'title': item.text,
            'link': item.get_attribute('href'),
            'item': item
        })

    target_page = pages_list[2].get('item').click()
    time.sleep(1)

    window_handles = driver.window_handles
    driver.switch_to.window(window_handles[-1])

    def get_page_info(d):
        return {
            'title': d.find_element_by_id('activity-name').text,
            'author': d.find_element_by_xpath('//div[@id="meta_content"]/span[@class="rich_media_meta rich_media_meta_text"]').text,
            'date': d.find_element_by_id('publish_time').text
        }
    page_info = WebDriverWait(driver, 60).until(get_page_info)
    print('Page title: ', driver.title)
    print(pages_list)
    print(page_info)
    time.sleep(10)
```



## 操作单页面后台管理系统

```python 
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as ec
from selenium.webdriver.common.by import By
import time

login_url = "http://localhost:8080"
options = webdriver.FirefoxOptions()
options.headless = False
with webdriver.Firefox(executable_path='./webdriver/firefox.exe', options=options) as driver:
    driver.get(login_url)
    driver.maximize_window()
    user_name = input("Please input user name:")
    pwd = input("Please input password:")
    form = WebDriverWait(driver, 20).until(ec.presence_of_element_located((By.TAG_NAME, 'form')))
    form.find_element_by_xpath('//input[@placeholder="Login Name"]').send_keys(user_name)
    form.find_element_by_xpath('//input[@type="password"]').send_keys(pwd)
    form.find_element_by_xpath('//button[@class="el-button sign el-button--primary"]').click()

    alert_box = WebDriverWait(driver, 20).until(ec.presence_of_element_located((By.XPATH, '//div[@class="msg-main-com-alert-message-v2"]')))
    plate_num = alert_box.find_element_by_xpath('//div[@class="top-driver msg-top-driver"]/div[1]/span').text
    print(plate_num)

    driver.find_element_by_xpath('//*[@id="app"]/div/div[1]/ul/li[3]').click()
    # Click alert type
    driver.find_element_by_xpath('//*[@id="app"]/div/div[2]/div[2]/div[3]/div[2]/div[1]/div/input').click()
    driver.find_element_by_xpath('//ul[@class="el-scrollbar__view el-select-dropdown__list"]/li[5]').click()

    alert_list_box = WebDriverWait(driver, 10).until(lambda d: d.find_element_by_xpath('//div[@class="ry-middle ry-scroll"]'))
    driver.execute_script("window.alert('Hello!')")

    time.sleep(12)
    print(driver.title)
```

