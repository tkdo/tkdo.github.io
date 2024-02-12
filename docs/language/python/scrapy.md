
### scrapy

```bash
pip install scrapy
scrapy startproject spider
scrapy genspider example example.com
scrapy crawl douban -o douban.csv
```

```bash
pip freeze >> requirement.txt
pip install -r requirement.txt
```


### selector

#### [官方文档](https://docs.scrapy.org/en/latest/topics/selectors.html)
    scrapy使用css和xpath选择器来定位元素，它有四个基础方法
    xpath() 返回选择器列表，每个选择器代表使用xpath语法选择的节点
    css() 返回选择器列表，每个选择器代表使用css语法选择的节点
    extract() 返回被选择元素的unicode字符串
    re() 返回通过正则表达式提取的unicode字符串列表

#### Xpath路径选择
| 符号 | 名称  | 含义 |
|:--------|:--------- |:--------|
| / | 绝对路径| 表示根节点开始选取 |
| // | 相对路径| 表示选择从任意位置的某个节点，而不考虑他们的位置 |

| 表达式 | 说明 |
|:--------| :---------|
| article | 选取所有article元素的所有子节点 |
| /article | 选取根元素article|
| article/a | 选取所有属于article的子元素的a元素|
| //div | 选取所有div子元素(不论出现在文档任何地方)|
| article//div | 选取所有属于article元素的后代的div元素，不管它出现在article之下的任何位置|
| //@class | 选取所有名为class的属性|

#### 索引的使用
    定位第8个td下的，第2个a节点  //*/td[7]/a[1]
    定位第8个td下的，第3个span节点  //*/td[7]/a[1]
    定位最后1个td下的，第最后1个a节点  //*/td[last()]/a[last()]

#### 使用属性
    为了定位更精准，我们可以使用属性符号@。
    定位所有包含name属性的input节点 //input[@name]
    定位含有属性的所有的input节点 //input[@*]
    定位所有value=2的input节点 //input[@value='2']
    使用多个属性定位 //input[@value='2'][@id='3']   //input[@value='2' and @id='3']   

#### 多条件选择
    可以使用|进行或的条件选择
    xpath("text() | .//p/text()  | .//p/span/text()")


#### 使用函数
    contains(s1, s2)  # 返回s1中包含s2的节点
    text() # 仅返回所指元素的文本内容
    string() # 返回所指元素的所有节点文本内容
    //a[contains(@href, 'promote.html')] # 定位href属性中包含“prompt.html”的所有节点
    //a[starts-with(@href, 'ads')] # href属性值是以“/ads”开头的所有a节点
    //a[text()="应用推广"] # 元素内的文本为“应用推广”的所有a节点

#### Xpath轴
    child 选取当前节点的所有子元素
    parent 选取当前节点的所有父节点
    descendant 选取当前节点的所有后代元素（子、孙等）
    ancestor 选取当前节点的所有先辈（父、祖父等）
    desendant-or-self 选取当前节点的所有后代元素（子、孙等）以及当前节点本身
    ancestor-or-self  选取当前节点的所有先辈（父、祖父等）以及当前节点本身
    preceding-sibling 选取当前节点的所有同级节点
    following-sibling 选取当前节点之后的所有同级节点
    preceding 选取文档中当前节点的开始标签之前的所有节点
    following 选取文档中当前节点的结束标签之后的所有节点
    self 选取当前节点
    attribute 选取当前节点的所有属性 
    namespace  选取当前节点的所有命名空间节点























