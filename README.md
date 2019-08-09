# how-to-Building-a-recommendation-system
## 这是一个构建推荐系统的git.

为了实现推荐系统，不得不从原来的基础打牢，今天主要复现之前的代码-2019-08-09

今天主要学习经典的案例：
主要fork 来源于：[推荐系统实例](https://github.com/lpty/recommendation)  

主要介绍 3大模型：
* 协同过滤 UserCF 的模型 
基于用户协同过滤算法  
* 基于隐语义（LFM）的模型  
* 基于图（PersonalRank）的模型  
**参与人**: [小兔子乖乖](https://github.com/PandasCute)  
**数据集**：[movielens](http://grouplens.org/datasets/movielens/1m)   

我主要把这部分代码做成了Notebook 代码格式，一是为了学习，而是为了萌新能看懂，主要是做成我们好看到的。

1.第一部分代码主要是将dat 转化为csv，我放入了preprocess.ipynb
2.第二部分代码主要做一个基于用户协同过滤的代码,放入UserCF1.ipynb    
1）这里主要做了一个Jaccard 系数:用于比较有限样本集之间的相似性与差异性。     

给定两个集合A,B，Jaccard 系数定义为A与B交集的大小与A与B并集的大小的比值，定义如下：     
![jaccard图片](https://github.com/PandasCute/how-to-Building-a-recommendation-system/blob/master/8644ebf81a4c510f05fdbf876959252dd42aa576.jpg)

当集合A，B都为空时，J(A,B)定义为1。      
2）余弦相似度：又称为余弦相似性。通过计算两个向量的夹角余弦值来评估他们的相似度，即w=|A∩B|/√|A|*|B|
