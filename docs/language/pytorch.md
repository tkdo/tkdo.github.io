
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
```python
RuntimeError: NCCL error in: ../torch/csrc/distributed/c10d/ProcessGroupNCCL.cpp:46, unhandled cuda error, NCCL version 2.10.3
>>> print(torch.backends.cudnn.version())
8302
>>> import torch; print(torch.__version__)
1.12.1+cu113
>>> import torch; print(torch.version.cuda)
11.3
pip3 install torch==1.8.1+cu111 torchvision==0.9.1+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```


```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.57.02    Driver Version: 470.57.02    CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  VGPU(0) Tesla T4    On   | 00000000:3B:00.0 Off |                    0 |
| N/A   47C    P0    45W /  70W |   3393MiB / 13573MiB |     25%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
报错
运行代码时报错：TypeError: clamp(): argument 'min' (position 2) must be Number, not Tensor
原因分析
结合github链接：https://github.com/Denys88/rl_games/pull/142 ,发现是torch.clamp()函数在pytorch1.9.0之前不支持Tensor作为clamp()的max/min参数，而1.9.0及之后支持。
解决方案
升级pytorch大于等于1.9.0版本，我这里升级到1.12.0
pip3 install torch==1.8.1+cu111 torchvision==0.9.1+cu111 -f https://download.pytorch.org/whl/torch_stable.html
pip3 install torch==1.10.1+cu111 torchvision==0.11.2+cu111 -f https://download.pytorch.org/whl/cu111/torch_stable.html
https://blog.csdn.net/BIT_HXZ/article/details/127604680
RuntimeError: NCCL error in: /pytorch/torch/lib/c10d/ProcessGroupNCCL.cpp:38, unhandled cuda error, NCCL version 2.7.8 #4530
pip3 install torch==1.8.1+cu111 torchvision==0.9.1+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```

```python
 File "ptuning/main.py", line 411, in <module>
    main()
  File "ptuning/main.py", line 123, in main
    model = AutoModel.from_pretrained(model_args.model_name_or_path, config=config, trust_remote_code=True)
  File "/usr/local/lib/python3.8/dist-packages/transformers/models/auto/auto_factory.py", line 475, in from_pretrained
    model_class = get_class_from_dynamic_module(
  File "/usr/local/lib/python3.8/dist-packages/transformers/dynamic_module_utils.py", line 443, in get_class_from_dynamic_module
    return get_class_in_module(class_name, final_module.replace(".py", ""))
  File "/usr/local/lib/python3.8/dist-packages/transformers/dynamic_module_utils.py", line 164, in get_class_in_module
    module = importlib.import_module(module_path)
  File "/usr/lib/python3.8/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1014, in _gcd_import
  File "<frozen importlib._bootstrap>", line 991, in _find_and_load
  File "<frozen importlib._bootstrap>", line 975, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 671, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 848, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "xxxx/huggingface/modules/transformers_modules/THUDM/chatglm2-6b/b1502f4f75c71499a3d566b14463edd62620ce9f/modeling_chatglm.py", line 14, in <module>
    from torch.nn.utils import skip_init
ImportError: cannot import name 'skip_init' from 'torch.nn.utils' (/usr/local/lib/python3.8/dist-packages/torch/nn/utils/__init__.py)
```
解决方案
```
pip3 install torch==1.10.1+cu111 torchvision==0.11.2+cu111 -f https://download.pytorch.org/whl/cu111/torch_stable.html
https://blog.csdn.net/BIT_HXZ/article/details/127604680
```
