# PageRank算法简介
### 1.1 开门见山
PageRank算法以前就是Google的屠龙刀，倚天剑，降龙十八掌。Google显示符合用户搜索的网页的依次显示呢？在以前Google及用过PageRank算法，对每个目标网页进行附上权值，权值大的就靠前显示，权值小的就靠后显示。对，PageRank算法就是给每个网页附加权值的~。PageRank算法借鉴学术界论文重要性的评估方法：谁被引用的次数多，谁就越重要。注：PageRank算法不单单是按照“被索引数”来给网页付权值的，用PR值表示每个网页被PageRank算法附加的权值。
### 1.2 PageRank算法的核心细想

PageRank算法的核心思想就跃然纸上：

 * 如果一个网页被很多其他网页链接到的话说明这个网页比较重要，也就是PageRank值会相对较高
 * 如果一个PageRank值很高的网页链接到一个其他的网页，那么被链接到的网页的PageRank值会相应地因此而提高

这就是为什么在后面讲解的例子中计算PR(A)时，如果B对A有出链，PR(B)就会给PR（A）增加贡献~~
## 2. 算法原理
### 2.1 基本概念

先了解几个基本概念，一遍后面内容理解

（1）出链

如果在网页A中附加了网页B的超链接B-Link，用户浏览网页A时可以点击B-Link然后进入网页B。上面这种A附有B-Link这种情况表示A出链B。可知，网页A也可以出链C，如果A中也附件了网页C的超链接C-Link。

（2）入链

上面通过点击网页A中B-Link进入B，表示由A入链B。如果用户自己在浏览器输入栏输入网页B的URL，然后进入B，表示用户通过输入URL入链B

（3）无出链

如果网页A中没有附加其他网页的超链接，则表示A无出链

（4）只对自己出链

如果网页A中没有附件其他网页的超链接，而只有他自己的超链接A-Link，则表示A只对自己出链

（5）PR值

一个网页的PR值，概率上理解就是此网页被访问的概率，PR值越高其排名越高。
## 3. 计算公式

下面给出计算PR值可能遇到的几种不同情况
对于一个页面A，那么它的PR值为：
* （1）PR(A) = (1 - d) + d(PR(T1)/C(t1) + … + R(Tn)/C(tn))
 
  *	PR(A) 是页面A的PR值
  *	PR(Ti)是页面Ti的PR值，在这里，页面Ti是指向A的所有页面中的某个页面
  *	C(Ti)是页面Ti的出度，也就是Ti指向其他页面的边的个数
  *	d 为阻尼系数，其意义是，在任意时刻，用户到达某页面后并继续向后浏览的概率，
  该数值是根据上网者使用浏览器书签的平均频率估算而得，通常d=0.85
  
* （2）还有一个版本的公式：

    * PR(A) = (1 - d)/N + d(PR(T1)/C(t1) + … + R(Tn)/C(tn))
    * N为页面的总数

