## conda资源包地址
https://repo.anaconda.com/archive/

## 设置conda环境变量
```bash
# 设置anaconda3环境配置
export ANACONDA_HOME=/root/pangu/env/anaconda3
export PATH="$ANACONDA_HOME/bin:$PATH"
alias python="$ANACONDA_HOME/bin/python"
```
## conda虚拟环境相关问题
### 创建虚拟环境
conda create -n env-py37-t15 python=3.7.6
### 删除虚拟环境
conda remove -n env-py37-t15 --all 

### 安装pack到虚拟环境
conda install -n env-py37-t15  tensorflow==1.15

### 开启虚拟环境
conda activate env-py37-t15

### 关闭虚拟环境
conda deactivate

### 搜索conda可用的package
(base) ssdeMacBook-Pro:~ ss$ anaconda search -t conda xgboost
### conda安装package
(base) ssdeMacBook-Pro:~ ss$ conda install -c anaconda py-xgboost
### conda 指定通道搜索安装包版本
conda search -c conda-forge gym
conda install -n env-py37-t15 -c conda-forge gym


## pip 
### pip设置下载timeout
pip --default-timeout=1000 install pandas

### 设置清华源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/peterjc123/
conda config --set show_channel_urls yes

