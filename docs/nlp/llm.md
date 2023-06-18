当模型特别大的时候，他的结构可能没那么重要了。量变引起质变。随机delta tuning模型参数，最后达到指定层效果相同。

    PromptPapers: https://github.com/thunlp/PromptPapers
    DeltaPapers: https://github.com/thunlp/DeltaPapers

    Programming Toolkit
    OpenPrompt: https://github.com/thunlp/OpenPrompt
    OpenDelta: https://github.com/thunlp/OpenDelta

    PromptDemo: https://colab.research.google.com/drive/10syott1zXaQkjnlxOiSXKDFGy68SWR0y?usp=sharing

    BMInf
    FP8 <- INT8
    ANCE论文



硬件
ubuntu 20.04 
RTX 3090 24G
CUDA 11.7


配置cuda环境
export CUDA_HOME=/usr/local/cuda-11.7
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH






cpu 适合逻辑计算
gpu 大量重复值计算

cpu给gpu发送控制信息，控制gpu进行运算。

.cuda to device 就是传输到gpu上进行计算。






前向传播，x是输入，w是参数，y是输出结果

为了计算参数的梯度

需要模型输出的梯度与模型输入进行一个外积。
为了完成这个计算，需要把模型输入放到显卡里面。

在前向传播的过程中，把所有的线性层都先保存到显卡里面。
直到反向传播用完则清除。从显卡释放。



模型梯度和模型二次项相关的历史信息。m和v
g是梯度。其中mv和梯度g等数量级。

mvg放到先看梯度。

m和v加起来是两倍参数量。



数据并行

有个参数服务器
参数复制到所有显卡上，
数据划分成3份。分别计算梯度，然后聚合（规约）取平均。
规约结果放到参数服务器上，







广播
广播算子，是把一个数据广播到所有其他显卡上。

reduce，规约
可以求和、取平均。

二号显卡得到的数据是in0+in1+in2+in3


all reduce
所有显卡都会得到相同的out=in0+in1+in2+in3
先进行规约然后在进行广播

reduce scatter
每张显卡只得到一部分的规约结果。











