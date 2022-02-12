# Embedding技术
## 背景
无论是搜索场景，还是推荐场景，还是使用深度学习还有其它等等的使用场景，都会使用到Embeding，下面我们介绍下Embeding的算法及其优化方法；、

### Embedding可以被划分为四类：
* 第一种是基于矩阵分解的方式，其中的代表方法是SVD；
* 第二种是基于内容的Embedding，它包含如word2vec、fasttext、glove等静态Embedding和如ELMo、GPT、BERT等动态Embedding；
* 第三种是基于物品序列的Embedding，代表方法是item2vec；
* 最后一类是基于图的Embedding，分为浅层图模型和深层图模型。浅层图模型有deepwalk、Node2vec、EGES，深层图模型有GCN、GraphSAGE等。

### 静态表征：word2vec
*首先来介绍一下静态表征。从名字我们就可以看出，静态表征方法主要是对一个词生成Embedding，且它在生成之后是固定的。这类方法的优势是我们可以提前确定词向量，在后面线上使用的时候就相当于“查字典”的操作，比较方便。它的劣势也很明显，针对一词多义的情况，静态表征无法解决。

*静态表征方法中具有代表性的模型是word2vec，它通过神经网络建立上下文和目标词的语言模型，得到的隐藏层权重可以用来表示每个词的词向量。Word2vec有两个架构：CBOW和Skip-gram。CBOW的输入是中心词的上下文词，输出是中心词。它的训练样本可以通过滑动窗口进行建立。Skip-gram是将中心词作为输入，输出是中心词的上下文。由于每个词是通过one-hot的方式进行表征，one-hot值和隐藏层的权重相乘就可以得到词的唯一Embedding表示。通过这一方法，我们可以用得到的Embedding来作为中心词的向量表征。

*word2vec的优化策略有两种。第一个是层次softmax，第二个是负采样。层次softmax根据词频建立一棵二叉树，把N分类问题变成了logN次二分类问题。负采样每次让一个训练样本更新网络中一小部分权重来降低整个梯度下降过程的计算量。

### 动态表征：ELMo
之前提到静态表征对一词多义无能为力。例如“bank”这个词，它即可以表示河岸又可以表示银行，但它在静态表征训练的词向量表中对应了唯一的Embedding。动态表征代表模型之一是ELMo。ELMo可以看作一个前向语言模型和后向语言模型的拼接，它的目标是最大化前向和后向的对数似然概率。由于采用LSTM作为特征抽取器，相较于现在流行的Transformer特征提取器，它的特征抽取能力有限。

### 动态表征：BERT
Bert可以被理解为多个transformer encoder的堆叠。由于Bert是双向语言模型，若使用传统语言模型的训练方式会出现see itself的问题，所以Bert借鉴了完形填空的方式，使用Masked Language Model的方式做预训练。又由于bert是一个通用语言模型，它会涉及句子层面的下游任务，所以bert的预训练任务中还加入了Next Sentence Prediction。所以Bert的预训练是一个多任务学习。

### 序列Embedding和图Embedding
序列Embedding比较经典的模型是盒马Embedding，是item2vec的改进版。盒马没有完全使用item id来作为商品的表征，而是考虑到item的属性信息，例如product id、brand id、category id等。将这些信息组成的列表作为商品的表征。这种方法的好处是将属性映射至同一向量空间，即使有item的某些属性维度缺失，依然可以通过其他维度来计算item的Embedding。所以，这种做法在冷启动阶段会有很好的效果。

阿里的EGES是一个图Embedding，它通过用户行为序列来构建商品关系图，然后使用deepwalk来生成一些序列。基于生成的序列，它使用skip-gram模型，增加了商品的side information并基于不同商品的side information不同权重来训练生成商品的Embedding表征。

### Airbnb Embedding和YoutubeNet。
接下来介绍两个业界比较经典的Embedding模型：Airbnb Embedding和YoutubeNet。

Airbnb Embedding的主要特点是分群Embedding，以及改进的负采样方法。分群Embedding是指模型没有直接使用item id，而是使用了商品相关属性的拼接，相当于对item进行了聚类。在改进的负采样中，Airbnb Embedding将下单行为作为正样本，结合自身业务对负采样的策略进行调整。

YoutubeNet将用户的连续特征、序列特征和离散特征全部进行Embedding化后拼接到一起输入一个DNN。在最后进行softmax层分类前的embedding就可以作为用户的Embedding特征。网络的目标是预测当前用户对video的喜好程度，那么在这种情况下softmax层得到的输出就可以作为video的向量表示。他们会将video向量离线建立索引，在线上使用的时候直接将用户Embedding和video Embedding做点乘操作即可。


## 参考
1.[苏永浩：Embedding技术在商业搜索与推荐场景的实践](https://mp.weixin.qq.com/s/9XypFylrfome2T5A0hWt6g)

