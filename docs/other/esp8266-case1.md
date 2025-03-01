# 1. adog流程

<div align="center">
  <img src="https://raw.githubusercontent.com/tkdo/device/refs/heads/main/svg/term-serving.drawio.svg" width="100%">  <!-- 居中且宽度占父容器 80% -->
</div>


# 2. serving 本地搭建


## 2.1 minconda安装
  下载安装地址：https://www.anaconda.com/docs/getting-started/miniconda/install

## 2.2 python==3.9.5环境配置
```bash
conda create -n dseek python=3.9.5
```
## 2.3 安装依赖包

## 2.4 下载deepseek
要求：GPU/‌16GB显存 ‌CPU‌:8核/32G内存

下载代码：https://modelscope.cn/models/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B/files

```bash
pip install modelscope
modelscope download --model deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B README.md --local_dir ./dir
```
## 2.5 代码
https://raw.githubusercontent.com/tkdo/hzoo/refs/heads/main/api.py

## 2.5 启动接口
```bash
uvicorn api:app --reload
```

## 2.6 UI界面效果


