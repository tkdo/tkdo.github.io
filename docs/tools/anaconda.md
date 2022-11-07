## conda资源包地址
https://repo.anaconda.com/archive/

## conda虚拟环境相关问题
### 1. 把ipykernel安装到虚拟环境中
conda install -n env-py37-tf20  ipykernel

### 2. 开启虚拟环境
conda activate env-py37-tf20

### 3. 关闭虚拟环境
conda deactivate

### 4. 搜索conda可用的package
(base) ssdeMacBook-Pro:~ ss$ anaconda search -t conda xgboost
### 5. conda安装package
(base) ssdeMacBook-Pro:~ ss$ conda install -c anaconda py-xgboost




