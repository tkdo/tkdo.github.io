
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
    scrapy使用css和xpath选择器来定位元素，它有四个基础方法
    xpath() 返回选择器列表，每个选择器代表使用xpath语法选择的节点
    css() 返回选择器列表，每个选择器代表使用css语法选择的节点
    extract() 返回被选择元素的unicode字符串
    re() 返回通过正则表达式提取的unicode字符串列表


