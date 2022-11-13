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
###  创建虚拟环境
conda create -n env-py37-t15 python=3.7.6
###  删除虚拟环境
conda remove -n env-py37-t15 --all 

### 2. 把ipykernel安装到虚拟环境中
conda install -n env-py37-t15  tensorflow==1.15

### 3. 开启虚拟环境
conda activate env-py37-t15

### 4. 关闭虚拟环境
conda deactivate

### 5. 搜索conda可用的package
(base) ssdeMacBook-Pro:~ ss$ anaconda search -t conda xgboost
### 6. conda安装package
(base) ssdeMacBook-Pro:~ ss$ conda install -c anaconda py-xgboost




