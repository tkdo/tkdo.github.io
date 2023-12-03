
## Tqdm 介绍
描述：Tqdm 是一个快速，可扩展的Python进度条，可以在 Python 长循环中添加一个进度提示信息，用户只需要封装任意的迭代器tqdm(iterator)
## 测试案例
### 1. 简单案例
```python
from tqdm import tqdm
import time

for i in tqdm(range(100)):
	time.sleep(0.1)
```
### 2. 使用trange替换
描述：将tqdm(range(100))替换为trange(100)，实现效果相同
```python
from tqdm import trange
import time

for i in trange(100):
	time.sleep(0.1)
```
### 3. set_description
描述：通过tqdm提供的set_description方法可以实时查看每次处理的数据
```python
from tqdm import tqdm
import time
pbar = tqdm(["a","b","c","d"])
for c in pbar:
  time.sleep(1)
  pbar.set_description(f"Processing {c}")
```
### 4. update更新进度条
描述：使用update手动更新进度条完成度
```python
from tqdm import tqdm
import time
#total参数设置进度条的总长度
with tqdm(total=100) as pbar:
  for i in range(100):
    time.sleep(0.05)
    #每次更新进度条的长度
    pbar.update(1)
```
### 5. 使用with关键字
```python
from tqdm import tqdm
import time
#total参数设置进度条的总长度
pbar = tqdm(total=100)
for i in range(100):
  time.sleep(0.05)
  #每次更新进度条的长度
  pbar.update(1)
#关闭占用的资源
pbar.close()
```
### 6. tqdm命令行
描述：更多探索，tqdm --help
```bash
time find . -name '*.py' -type f -exec cat \{} \; | tqdm | wc -l
3298173it [00:20, 157910.88it/s]
real	0m21.015s
user	0m5.945s
sys	0m11.775s
```
### set_postfix
描述：通过set_description和set_postfix方法设置进度条显示信息
```python
from tqdm import trange
from random import random,randint
import time
with trange(100) as t:
  for i in t:
    #设置进度条左边显示的信息
    t.set_description("GEN %i"%i)
    #设置进度条右边显示的信息
    t.set_postfix(loss=random(),gen=randint(1,999),str="h",lst=[1,2])
    time.sleep(0.1)
```
### 8. 多层次嵌套
描述：通过tqdm也可以很简单的实现嵌套循环进度条的展示
```python
from tqdm import tqdm
import time
for i in tqdm(range(20), ascii=True,desc="1st loop"):
  for j in tqdm(range(10), ascii=True,desc="2nd loop"):
    time.sleep(0.01)
```
### 9. 多进程处理
描述：在使用多进程处理任务的时候，通过tqdm可以实时查看每一个进程任务的处理情况
```python
from time import sleep
from tqdm import trange, tqdm
from multiprocessing import Pool, freeze_support, RLock

L = list(range(9))

def progresser(n):
  interval = 0.001 / (n + 2)
  total = 5000
  text = "#{}, est. {:<04.2}s".format(n, interval * total)
  for i in trange(total, desc=text, position=n,ascii=True):
    sleep(interval)

if __name__ == '__main__':
  freeze_support() # for Windows support
  p = Pool(len(L),
       # again, for Windows support
       initializer=tqdm.set_lock, initargs=(RLock(),))
  p.map(progresser, L)
  print("\n" * (len(L) - 2))
```
### 10. pandas中使用tqdm


```python
import pandas as pd
import numpy as np
from tqdm import tqdm
df = pd.DataFrame(np.random.randint(0, 100, (100000, 6)))
tqdm.pandas(desc="my bar!")
df.progress_apply(lambda x: x**2)
```
### 11. 注意多行问题
描述：在使用tqdm显示进度条的时候，如果代码中存在print可能会导致输出多行进度条，此时可以将print语句改为tqdm.write，代码如下
```python
from tqdm import tqdm
import time

for i in tqdm(range(10),ascii=True):
  tqdm.write("come on")
  time.sleep(0.1)
```
