# 中文错别字检测方案

## 1. 引言
对英语语种来说，词与词之间用空格隔开，而且构造简单，词均由26个字母组合而成，因此，无论是拼写检查还是纠错均可以使用简单的方案完成。对于拼写检查可以使用字典树进行匹配检查，对于纠错可以使用编辑距离和贝叶斯方法完成。但对中文来说，输入的句子没有天然的分割，需要先行分词

对于拼写检查
## 2. 数据准备

- **获取语料：** 可抓取互联网语料或使用公开语料
- **预处理：** 利用分词工具进行分词，去停用词，低频词和数字等归为单独的一类(如“_UNK”,“<DIGIT>”)
- **构建词表：** 构建词表，dict形式，key为词，value为词频

## 3. 检测
检测分两种方式：
1. **N-Gram**
    - 从头到尾遍历句子，在每个位置使用字级别的n-gram模型计算概率，概率低于阈值(超参数)的位置记为待纠错位置
    - 将待纠错位置与上下文组合，在字典中匹配，若所有组合在字典中都匹配不到，则确定为错误位置
2. **单字**
    - 分词是根据词典进行分割的，分词后是单个字符即认为和上下文未构成词，被视为错字

## 4. 纠错
- 候选句子构造：将上一步被判定为错误的字进行同音字和形近字的替换(使用混淆集)，将替换后的字分别与前后字进行组合，到字典中匹配，将匹配到的作为候选，构造候选句子
- 候选句子打分：利用语言模型对所有候选句子进行打分，得分最高且大于阈值的即为最终纠正后的结果

## 5. 评测
  选用**SIGHAN Bake-off**语料训练分别评测检测和纠错模型的Precision和Recall，确定合适的超参数（阈值和n-gram大小）

## 6. Seq2Seq and Attention
以上模型为传统方法，无监督训练，但稍显简单粗暴，下面想说一下个人猜测可行的算法（由于时间有限，没能查阅文献，所以一下为个人猜测，以后可以跑跑实验）。

1. **训练集：** Seq2Seq为深度学习方法，是有监督学习，需要构造训练集。具体可以在现有语料上进行构造
    - 选取语料，先要通过标点等切分为短句子，并设置句长阈值，高于阈值的丢弃。这是因为过长的句子可能会影响RNN性能。然后进行分词等预处理。
    - 对句子中每个词语随机执行同音替换和模糊音替换，但替换个数要做限制

2. **模型**
    - 首先用Encoder将输入句子编码为句向量
    - 利用Decoder进行解码，输出正确结果
    - 可采用双向RNN，并加入Attention机制
    - 考虑使用额外的方法判断每个位置的词是否需要被解码，因为以上模型存在正确的词被错误解码的情况
    - 可使用GLUE标准，结合F1值进行评测