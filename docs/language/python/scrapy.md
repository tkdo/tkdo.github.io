
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