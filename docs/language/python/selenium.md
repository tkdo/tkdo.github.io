#### 下载地址 
```
https://registry.npmmirror.com/binary.html
```
#### 问题
```
https://stackoverflow.com/questions/67212594/attributeerror-pathdistribution-object-has-no-attribute-name
https://stackoverflow.com/questions/76614392/getting-error-when-i-run-selenium-script-value-error-timeout-value-connect-was
https://stackoverflow.com/questions/60296873/sessionnotcreatedexception-message-session-not-created-this-version-of-chrom
https://stackoverflow.com/questions/60296873/sessionnotcreatedexception-message-session-not-created-this-version-of-chrome
brew install --cask chromedriver
```
#### 元素定位
##### 获取单个元素
```python
driver.find_element(By.ID，"inputorigina”）
driver.find_element(By.CSS_SELECTOR，"#inputoriginal"）
driver.find_element(By.TAG_NAME，"div"）
driver.find_element（By.NAME，"username")
driver.find_element(By.LINK_TEXT,"下一页"）
```
##### 如果找不到相应的元素会报错
```python
selenium.common.exceptions.NosuchElementException: Message:no such element:Unable to 7ocate element:XX
```

##### 获取多个元素
```python
driver.find_elements(By.ID，"inputoriginal"）
driver.findelements(By.CSS_SELECTOR，"inputoriginal"）
driver.find_elements(By.TAG_NAME,"div")
driver.find_elements(By.NAME，"username")
driver.find_elements(By.LINK_TEXT，"下一页"）
```
##### 内容获取
```python
size 返回元素大小
text 获取元素的文本 <div>hello</div>
title 获取页面的title
current_url 获取当前页面URL
get_attribute() 获取属性值<a href="www.baidu.com"></a>
is_display() 判断元素是否可见，浏览器渲染加载出来，
is_enabled() 判断元素是否可用，是否可用，有可能被广告遮盖，不可以点击，但是已经加载出来。
```
##### 窗口操作
```python
maximize_window() 窗口最大化
set_window_size(100,100) 浏览器大小 设置浏览器的宽高(像素点)
set_window_position(300,200) 浏览器位置 设置浏览器位置
back() 后退
forward() 前进
refresh() 刷新
close() 关闭浏览器按钮(关闭单个窗口)
quit() 关闭webDriver启动的窗口
```
#### 案例

##### 访问百度，输入搜索词，并点击搜索
```python
#encoding:utf-8
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
# 调用键盘按键操作时需要引入的keys包
#driver = webdriver.Chrome(executable_path="/Users/ss/Pangu/PythonHome/sharingan/driver/chromedriver")
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
# 调用环境变量指定的PhantomJS浏览器创建浏览器对象
def __driver():
    driver = webdriver.Chrome(ChromeDriverManager().install())
    return driver

def test01():
    time.sleep(2)
    # get方法会一直等到页面被完全加载,然后才会继续程序,通常测试会在这里选择
    driver = __driver() 
    driver.get("http://www.baidu.com/")
    time.sleep(1)
    # id="kw"时百度搜索输入框
    kw = driver.find_element(By.ID,"kw") 
    # 输入字符串
    kw.send_keys("中国")
    time.sleep(3)
    # id="su"是百度搜索按钮，click是模拟点击
    # su = driver.find_element(By.ID,"su").click()
    driver.find_element(By.CSS_SELECTOR, "#su").click()
    time.sleep(3)
    # 关闭浏览器
    driver.quit()
```

##### 访问有道翻译网站，输入单词，并获取翻译后的内容
```python
def test02():
    driver = __driver()
    # get方法会一直等到页面被完全加载,然后才会继续程序,通常测试会在这里选择 time.sleep(2)
    driver.get("https://fanyi.youdao.com/")
    print(driver.title)
    print(driver.current_url)
    time.sleep(2)
    driver.find_element(By.CSS_SELECTOR,".tab-header .tab-left .color_text_1").click()
    # 获取输入框    
    input = driver.find_element(By.ID,"js_fanyi_input")
    # 输入内容
    input.send_keys("hello")
    time.sleep(2)
    transTarget = driver.find_element(By.ID,"js_fanyi_output_resultOutput")
    print(transTarget.size)
    print(transTarget.text)
    print(transTarget.get_attribute("href"))
    print(transTarget.is_displayed())
    print(transTarget.is_enabled())
```
##### 爬当当网书籍的数据
```python
def test03():
    driver.implicitly_wait(30)
    # get方法会一直等到页面被完全加载,然后才会继续程序,通常测试会在这里选择 time.sleep(2)
    driver.get("https://www.dangdang.com/")
    driver.maximize_window()
    # 获取输入框
    key = driver.find_element(By.ID,"key_S")
    key.send_keys("科幻")
    # 获取搜索框 点击搜索
    search = driver.find_element(By.CSS_SELECTOR,"#form_search_new .button")
    search.click()
    # 获取商品标题及价格
    for i in range(5):
        shoplist = driver.find_elements(By.CSS_SELECTOR,".shoplist li")
        for li in shoplist:
            print(li.find_element(By.CSS_SELECTOR,"a").get_attribute("title"))
            print(li.find_element(By.CSS_SELECTOR,".search_now_price").text)
    # 获取下一页的按钮
    next = driver.find_element(By.LINK_TEXT,"下一页")
    next.click()
```












