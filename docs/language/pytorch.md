
### 1.1 model.train()
model.train()的作用是启用 Batch Normalization 和 Dropout。
    如果模型中有BN层(Batch Normalization）和Dropout，需要在训练时添加model.train()。model.train()是保证BN层能够用到每一批数据的均值和方差。对于Dropout，model.train()是随机取一部分网络连接来训练更新参数。

### 1.2 model.eval()
model.eval()的作用是不启用 Batch Normalization 和 Dropout。

    如果模型中有BN层(Batch Normalization）和Dropout，在测试时添加model.eval()。model.eval()是保证BN层能够用全部训练数据的均值和方差，即测试过程中要保证BN层的均值和方差不变。对于Dropout，model.eval()是利用到了所有网络连接，即不进行随机舍弃神经元。
    训练完train样本后，生成的模型model要用来测试样本。在model(test)之前，需要加上model.eval()，否则的话，有输入数据，即使不训练，它也会改变权值。这是model中含有BN层和Dropout所带来的的性质。
    在做one classification的时候，训练集和测试集的样本分布是不一样的，尤其需要注意这一点。

### pytorch tensorborad
```python
from torch.utils.tensorboard import SummaryWriter
import numpy as np 
writer = SummaryWriter("logs")
for i in range(100):
    writer.add_scalar("y=2x", 2*i, i)
writer.close()
```


### pytorch检验GPU是否可用
```python
import torch
torch.cuda.is_available()
```


### __call__方法使用
```python
class Person:
    def __call__(self, name):
        print(f"__call__Hello:{name}")

    def hello(self, name):
        print(f"hello:{name}")

person = Person()
person("zhangsan")
person.hello("lisi")
```

### transformer使用
```python
from torch.utils.tensorboard import SummaryWriter
writer = SummaryWriter("logs")
from PIL import Image

img_path = "assets/image/reward_function.png"
img = Image.open(img_path)
print(img)

from torchvision import transforms
# transforms ToTensor
trans_totensor = transforms.ToTensor()
img_tensor = trans_totensor(img)
print(img_tensor)
writer.add_image("toTensor", img_tensor)

# Normalize
print(img_tensor[0][0][0])
print(img_tensor.shape)
trans_norm = transforms.Normalize([6, 3, 2, 1], [9, 3, 5, 2])
img_norm = trans_norm(img_tensor)
print(img_norm[0][0][0])
writer.add_image("Normalize", img_norm, 2)

# Resize
trans_resize = transforms.Resize((64, 64))
img_resize = trans_resize(img)
img_resize = trans_totensor(img_resize)
print(img_resize)
writer.add_image("Resize", img_resize, 3)

# Compose
trans_compose = transforms.Compose([transforms.Resize(64),
                                    transforms.ToTensor()])
img_compose = trans_compose(img)
writer.add_image("Compose", img_compose, 4)

# RandomCrop
trans_compose2 = transforms.Compose([transforms.RandomCrop(64),
                                    transforms.ToTensor()])
for i in range(10):
    img_crop = trans_compose2(img)
    writer.add_image("RandomCrop", img_crop, i + 4)

writer.close()
```



