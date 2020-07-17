# Selenium文档

[TOC]

## 参考文档

* [https://selenium-python.readthedocs.io/](https://selenium-python.readthedocs.io/)
* [https://www.selenium.dev/documentation/zh-cn/](https://www.selenium.dev/documentation/zh-cn/)



## 安装

1. 安装selenium

   ```bash
   pip install selenium
   ```

   

2. 下载webdriver的二进制文件

   > 注意：把webdriver的二进制文件的地址加入环境变量，如果不加入需要在代码中指定地址



## 基本使用

```python
from selenium import webdriver

option = webdriver.FirefoxOptions()
# 无头浏览器
option.headless = True
driver = webdriver.Firefox(executable_path='./webdriver/firefox.exe', options=option)
driver.get('http://www.baidu.com')

driver.close()
```



## 控制浏览器

### 打开网站

```python
driver.get('http://www.baidu.com')
```

### 获取当前URL

```python
driver.getCurrentUrl()
```

### 后退

```python
driver.back()
```

### 前进

```python
driver.forward()
```

### 刷新

```python
driver.refresh()
```

### 获取标题

```python
driver.title
```

### 窗口和标签页

WebDriver 没有区分窗口和标签页。如果你的站点打开了一个新标签页或窗口，Selenium 将允许您使用窗口句柄来处理它。 每个窗口都有一个唯一的标识符，该标识符在单个会话中保持持久性。

```python
driver.current_window_handle
```

### 切换窗口和标签页

要使用新窗口，您需要切换到它。 如果只有两个选项卡或窗口被打开，并且你知道从哪个窗口开始， 则你可以遍历 WebDriver， 通过排除法可以看到两个窗口或选项卡，然后切换到你需要的窗口或选项卡。

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 启动驱动程序
with webdriver.Firefox() as driver:
    # 打开网址
    driver.get("https://seleniumhq.github.io")

    # 设置等待
    wait = WebDriverWait(driver, 10)

    # 存储原始窗口的 ID
    original_window = driver.current_window_handle

    # 检查一下，我们还没有打开其他的窗口
    assert len(driver.window_handles) == 1

    # 单击在新窗口中打开的链接
    driver.find_element(By.LINK_TEXT, "new window").click()

    # 等待新窗口或标签页
    wait.until(EC.number_of_windows_to_be(2))

    # 循环执行，直到找到一个新的窗口句柄
    for window_handle in driver.window_handles:
        if window_handle != original_window:
            driver.switch_to.window(window_handle)
            break

    # 等待新标签页完成加载内容
    wait.until(EC.title_is("SeleniumHQ Browser Automation"))
```

### 创建新窗口(或)新标签页并且切换

```python
# 打开新标签页并切换到新标签页
driver.switch_to.new_window('tab')

# 打开一个新窗口并切换到新窗口
driver.switch_to.new_window('window')
```

### 关闭窗口或标签页

```python
#关闭标签页或窗口
driver.close()

#切回到之前的标签页或窗口
driver.switch_to.window(original_window)
```

### 在会话结束时退出浏览器

```python
driver.quit()
```

### 获取窗口大小

```python
width = driver.get_window_size().get("width")
height = driver.get_window_size().get("height")
```

### 设置窗口大小

```python
driver.set_window_size(1024, 768)
```

### 得到窗口的位置

```python
x = driver.get_window_position().get('x')
y = driver.get_window_position().get('y')
```

### 设置窗口位置

```python
driver.set_window_position(0, 0)
```

### 最大化窗口

```python
driver.maximize_window()
```

### 最小化窗口

```python
driver.minimize_window()
```

### 全屏窗口

```python
driver.fullscreen_window()
```



## 定位元素

### 1. 查找一个元素

 	1. find_element
     * 格式：find_element(By.**, 'str')
     * ID = "id"
       XPATH = "xpath"
       LINK_TEXT = "link text"
       PARTIAL_LINK_TEXT = "partial link text"
       NAME = "name"
       TAG_NAME = "tag name"
       CLASS_NAME = "class name"
       CSS_SELECTOR = "css selector"
 	2. find_element_by_id
 	3. find_element_by_name
 	4. find_element_by_xpath
 	5. find_element_by_link_text
 	6. find_element_by_partial_link_text
 	7. find_element_by_tag_name
 	8. find_element_by_class_name
 	9. find_element_by_css_selector

### 2. 查找多个元素

1. find_elements
   * 格式：find_elements(By.**, 'str')
   * ID = "id"
     XPATH = "xpath"
     LINK_TEXT = "link text"
     PARTIAL_LINK_TEXT = "partial link text"
     NAME = "name"
     TAG_NAME = "tag name"
     CLASS_NAME = "class name"
     CSS_SELECTOR = "css selector"
2. find_elements_by_name
3. find_elements_by_xpath
4. find_elements_by_link_text
5. find_elements_by_partial_link_text
6. find_elements_by_tag_name
7. find_elements_by_class_name
8. find_elements_by_css_selector



## 页面交互

### 输入文字

```python
element.send_keys('string');
```

### 点击

```python 
element.click()
```

### 动作链

```python
from selenium.webdriver import ActionChains
from selenium import webdriver

driver = webdriver.Firefox()
ActionChains(driver).drag_and_drop(element, target).perform()
```

### 页面切换

```python
window_handles = driver.window_handles
driver.switch_to_window(window_handles[-1])
```

### 保存网页

```python
driver.save_screenshot('pic-name.jpg')
```



## 等待

* 使用方法

  `WebDriverWait(driver, timeout=3).until(some_condition)`

  ```python
  from selenium.webdriver.support.ui import WebDriverWait
  from selenium.webdriver.support import expected_conditions as EC
  
  element = WebDriverWait(driver, 10).until(
  	EC.presence_of_element_located((By.ID, "myDynamicElement"))
  )
  ```

* 判断条件

  * title_is
  * title_contains
  * presence_of_element_located
  * visibility_of_element_located
  * visibility_of
  * presence_of_all_elements_located
  * text_to_be_present_in_element
  * text_to_be_present_in_element_value
  * frame_to_be_available_and_switch_to_it
  * invisibility_of_element_located
  * element_to_be_clickable
  * staleness_of
  * element_to_be_selected
  * element_located_to_be_selected
  * element_selection_state_to_be
  * element_located_selection_state_to_be
  * alert_is_present



## 页面加载策略

* normal

  默认策略。

  此配置使Selenium WebDriver等待整个页面的加载. 设置为 **normal** 时, Selenium WebDriver将保持等待, 直到 返回 load 事件

* eager

  这将使Selenium WebDriver保持等待, 直到完全加载并解析了HTML文档, 该策略无关样式表, 图片和subframes的加载.

* none

* 使用方法

  ```python
  options = Options()
  options.page_load_strategy = 'eager'
  ```

  