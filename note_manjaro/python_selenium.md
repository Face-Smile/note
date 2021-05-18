# python-selenium

## 1. 快速上手

```python
from selenium import webdriver

url = 'http://www.goubanjia.com/'

# 初始化浏览器
browser = webdriver.Chrome(executable_path='/home/smilejack/selenium_driver/chromedriver')

# 访问指定url
browser.get(url)

# 获取网页html
print(browser.page_source)

# 关闭浏览器
browser.quit()
```



## 2. 定位

### 1. 使用`find_element_by ......`方法进行元素定位



### 2. 执行js实现屏幕滑动

```
browser.execute_script("window.scrollTo(0, document.body.scrollHeight); var lenOfPage=document.body.scrollHeight; return lenOfPage;")
```





## 3. 输入 



对查找的元素进行输入, 使用`send_keys()` 方法

![image-20191109223058867](python_selenium/image-20191109223058867.png)





## 4. 配置

### 1. Chrome

#### 1. 不加载图片

```python
from selenium import webdriver

url = 'http://www.goubanjia.com/'

# 配置
chrome_opt = webdriver.ChromeOptions()
prefs = {"profile.managed_default_content_settings.images": 2}
chrome_opt.add_experimental_option("prefs", prefs)

# 初始化一个浏览器
browser = webdriver.Chrome(executable_path='/home/smilejack/selenium_driver/chromedriver', options=chrome_opt)

# 访问指定url
browser.get(url)
# 获取网页html
print(browser.page_source)

# 关闭浏览器
browser.quit()
```



