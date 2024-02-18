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
#### 显示等待
```python
from selenium.webdriver.support.ui import WebDriverwait
from selenium.webdriver.support import expected_conditions as EC
#程序每0.5秒检查，是否满足:标题包含“百度一下”这个条件，检查是否满足条件的最长时间为:15秒，超过15秒仍未满足条件则抛出异常
WebDriverwait（driver，15).until（Ec.title_contains（“百度一下”）
#程序每0.5秒检查，是否满足:某定位的元素出现，检查是否满足条件的最长时间为:15秒，超过15秒仍未满足条件则抛出异常
WebDriverwait(driver，15）.until（Ec.visibility_of_element_located(By.CSS_SELECTOR，“XX”））
```
#### 隐式等待
```python
implicitly_wait(time_to_wait)
设置时间单位为秒 例如30秒 implicitly_wait(30) 意思是超过30秒没有定位到一个元素 程序就回抛出异常,期间会一直轮询查找定位元素
```
#### 鼠标及键盘操作
##### 鼠标操作
```python
context_click() 右击
double_click() 双击
drag_and_drop() 拖动
move_to_element() 悬停
perform()    执行
```
##### 百度打开更多
```python
def test04():
    driver  = __driver()
    from datetime import time
    from selenium import webdriver
    # 调用环境变量指定的PhantomJS浏览器创建浏览器对象
    from selenium.webdriver import ActionChains
    from selenium.webdriver.common.by import By
    # get方法会一直等到页面被完全加载,然后才会继续程序,通常测试会在这里选择 time.sleep(2)
    driver.get("https://www.baidu.com/")
    more = driver.find_element(By.LINK_TEXT,"更多")
    # 将鼠标移动到更多按钮
    ActionChains(driver).move_to_element(more).perform()
```

##### 键盘操作
```python
send_keys（Keys,BACK_SPACE）删除键（Backspace）
send_keys（Keys.SPACE）空格键（Space）
send_keys（Keys.TAB）制表键（Tab）
send_keys（Keys·ESCAPE）回退键（ESC）
send_keys（Keys·ENTER）回车键（Enter）
send_keys（Keys.CONTROL，'a'）全选（Ctrl+A）
send_keys（Keys.CONTROL，'c'）复制（Ctr1+C）
```

##### 百度键盘操作
```python
from selenium.webdriver.common.keys import Keys
driver.get("https://www.baidu.com/")
element = driver.find_element(By.ID,"kw")
#输入用户名
element.send_keys("admin1")
#删除1
element.send_keys(Keys.BACK_SPACE)
#全选
element.send_keys(Keys.CONTROL,'a')
#复制
element.send_keys(Keys.CONTROL,'c')
#粘贴
element.send_keys(Keys.CONTROL,'v')
```

#### 滚动条
    在HTML页面中，由于前端技术框架的原因，页面元素为动态显示，元素根据滚动条的下拉而被加载
    #1.设置]avascritp脚本控制滚动条，（0:左边距；1000:上边距；单位像素）
    js="window.scro11To(0,1000)"
    #2，WebDriver调用js脚本方法
    driver.execute_script(js）

####  窗口截图
    自动化脚本是由程序去执行的，因此有时候打印的错误信息并不是十分明确。如果在执行出错的时候对当前窗口截图保存，那么通过图片就可以非常直观地看到出错的原因
    #截取当前窗口
    driver.get_screenshot_as_file("./demo.png")







