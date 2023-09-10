
## 描述
huggingface transformers库的文本生成 generate() 函数，是 GenerationMixin 类的实现（class transformers.generation_utils.GenerationMixin），是自回归文本生成预训练模型相关参数的集大成者。因此本文解读一下这些参数的含义以及常用的 Greedy Search、Beam Search、Sampling（Temperature、Top-k、Top-p）等各个算法的原理。



- gready decoding:当num_beams=1 而且do_sample=False时，调用gready_search()方法，每个step生成条件概率最高的词，因此生成单条文本。
-  multinomail sampling:当num_beams=1 且 do_sample=True时，调用sample()方法，对词表做一个采样，而不是选条件概率最高的词，增加多样性。
- beam-search decoding：当num_beams>1 且 do_sample=False时，调用beam_search()方法，做一个num_beams的柱搜索，每次都是贪婪选择topN个柱。
- beam-search multinomial sampling：当num_beams>1 且do_sample=True时，调用beam_sample()方法，相当于每次不再是贪婪的选择topN个柱，而是增加了一些采样。
- diverse beam-search decoding：当num_beams>1 且 num_beam_groups>1时，调用group_beaam_search()方法。
- constrainted beam-search decoding: 当constraints!=None或者force_words_id!=None，实现可用文本生成。

## 输入参数含义
inputs：输入pormpt，如果为空，则用batch_size为1的bos_token_id初始化。对于只有decoder的模型（GPT系列），输入需要时input_ids，对于encode-decoder模型（BART，T5等），输入多样化。
max_length：生成序列的最大长度
min_length：生成序列的最短长度，默认值是10
do_sample：是否开启采样，默认值False，即贪婪找最大条件概率的词。
early_stopping：是否在至少生成num_beams个句子后停止beam-search，默认值是False


https://blog.csdn.net/muyao987/article/details/125917234


