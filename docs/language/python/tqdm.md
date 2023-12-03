Tqdm 是一个快速，可扩展的Python进度条，可以在 Python 长循环中添加一个进度提示信息，用户只需要封装任意的迭代器tqdm(iterator)

```python
from tqdm import tqdm
import time

for i in tqdm(range(100)):
	time.sleep(0.1)
```









