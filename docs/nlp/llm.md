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




