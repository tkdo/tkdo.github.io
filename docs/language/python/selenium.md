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
is_display() 判断元素是否可见
is_enabled() 判断元素是否可用
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
##### 访问有道翻译网站，输入单词，并获取翻译后的内容
```python
#encoding:utf-8
from datetime import time
from selenium import webdriver
# 调用环境变量指定的PhantomJS浏览器创建浏览器对象
from selenium.webdriver.common.by import By
driver = webdriver.Chrome("./chromedriver.exe")
# get方法会一直等到页面被完全加载,然后才会继续程序,通常测试会在这里选择 time.sleep(2)
driver.get("https://fanyi.youdao.com/")
time.sleep(4)
# 获取输入框
input = driver.find_element(By.ID,"inputOriginal")
# 输入内容
input.send_keys("hello")
# 获取翻译按钮
tbtn = driver.find_element(By.ID,"transMachine")
# 先获取遮挡的广告条 点击关闭按钮
close_btn = driver.find_element(By.CSS_SELECTOR,".guide-con .guide-close")
close_btn.click()
# 点击翻译
tbtn.click()
# 获取翻译后的内容
transTarget = driver.find_element(By.ID,"transTarget")
print(transTarget.text)
```











