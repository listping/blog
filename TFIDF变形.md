## TF-IDF及其改进算法

### TF-IDF的主要思想

TF-IDF的主要思想是：如果某个词或短语在一篇文章中出现的频率TF高，并且在其他文章中很少出现，则认为此词或者短语具有很好的类别区分能力，适合用来分类。TF-IDF实际上是：TF * IDF，TF词频(Term Frequency)，IDF逆向文件频率(Inverse Document Frequency)。TF表示词条在文档d中出现的频率。IDF的主要思想是：如果包含词条t的文档越少，也就是n越小，IDF越大，则说明词条t具有很好的类别区分能力。

### TF-IDF的不足
对于传统的TF-IDF而言，可以计算出在一文档集合中特定文档里所包含的区别于其他文档的重要词语，换言之就是关键词。而在文本分类问题中，仅仅是每篇文档区分度强的关键词还不足以作为分类的评判标准，即传统的TF-IDF还存在许多不足，在查阅了很多相关论文后我列举了以下一些传统TF-IDF的不足之处如下：

* 没有考虑特征词的位置因素对文本的区分度，词条出现在文档的不同位置时，对区分度的贡献大小是不一样的。

* 按照传统TF-IDF函数标准，往往一些生僻词的IDF(反文档频率)会比较高、因此这些生僻词常会被误认为是文档关键词。(换句话说，如果一个特征项只在某一个类别中的个别文本中大量出现，在类内的其他大部分文本中出现的很少，那么不排除这些个别文本是这个类中的特例情况，因此这样的特征项不具有代表性。)

* 传统TF-IDF函数中的IDF部分只考虑了特征词与它出现的文本数之间的关系，而忽略了特征项在一个类别中不同的类别间的分布情况。

* 对于文档中出现次数较少的重要人名、地名信息提取效果不佳。

### TF-IDF的算法优化
1). 一种词频因子的变体计算公式是： Wtf=1+log(tf) 

    即将词频数值tf取Log值来作为词频权值，比如单词在文档中出现4次，则其词频因子权值为3，公式中的数字1是为了平滑计算用的，因为如果tf值为1的情况下，
    
    取Log后值为0，即原本出现了一次的单词，按照此方法会认为这个单词从来没有从文档中出现过，为避免采用加1进行平滑； 
    
    之所以对词频取Log，是基于：既是一个单词出现了10次，也不应该在计算特征权值时，比出现一次的情况权值大10倍，加入Log机制抑制这种过大的差异。
    
2) IDF改进

a)	IDF=log(本类含特征词文档数m*总文档数N/所有含特征词文档数n+0.01)

b)	用P（Mk）表示特征词Mk在当前类别中的频率，P（Mk）’表示特征词Mk在其他类别中的频率，对IDF计算改进如下：
IDF=log(1+ P（Mk）/ P（Mk）’)

c)	IDF=log(文档所有词语词频之和N/该单词词频之和n)。

