### 环境安装 
    1)到这 https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
    下载 Anaconda3-5.1.0-Windows-x86_64.exe    对应 python3.6.3
    2)到这 https://pypi.tuna.tsinghua.edu.cn/simple/opencv-python/
    下载opencv 3.4.1.15

### opencv通道
    opencv 默认读取格式是 BGR 格式，matplotlib 或其他库的读取格式可能是 RGB 的
    opencv 读取并用 opencv 自带的展示函数不需要进行通道转换，但 opencv 读取后用其他库展示图片需要通道转换   
