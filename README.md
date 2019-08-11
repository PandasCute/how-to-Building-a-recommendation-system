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

* 1.第一部分代码主要是将dat 转化为csv，我放入了preprocess.ipynb   
* 2.第二部分代码主要做一个基于用户协同过滤的代码,放入UserCF1.ipynb 
UserCF 的原理：     
原理核心：当一个用户A需要个性化推荐时，可以先找到他有相似兴趣的其他用户，然后把那些用户喜欢的、而用户A没听过的物品推荐给A。
a) 找到和目标用户兴趣相似的用户集合

b) 找到这个集合中的用户喜欢的，且目标用户没有听说过的物品推荐给目标用户。


1）这里主要做了一个Jaccard 系数:用于比较有限样本集之间的相似性与差异性。  
UserCF1.ipynb主要是做a第一部分的事情，就是找找到和目标用户兴趣相似的用户集合。

给定两个集合A,B，Jaccard 系数定义为A与B交集的大小与A与B并集的大小的比值，定义如下：     
![jaccard图片](https://github.com/PandasCute/how-to-Building-a-recommendation-system/blob/master/8644ebf81a4c510f05fdbf876959252dd42aa576.jpg)

当集合A，B都为空时，J(A,B)定义为1。      
2）余弦相似度：又称为余弦相似性。通过计算两个向量的夹角余弦值来评估他们的相似度，即w=|A∩B|/√|A|*|B|

* 3.第三部分代码主要做一个基于用户协同过滤的代码2,放入UserCF2.ipynb。
核心就是得到用户之间的兴趣相似度后，UserCF算法会给用户推荐和他兴趣最相似的K个用户喜欢的物品。    

![](https://github.com/PandasCute/how-to-Building-a-recommendation-system/blob/master/918077-20181103141555690-125781261.png)

          
公式度量了UserCF算法中用户u对物品i的感兴趣程度：其中，S(u, K)包含和用户u兴趣最接近的K个用户，N(i)是对物品i有过行为的用户集合，Wuv是用户u和用户v的兴趣相似度，Rvi代表用户v对物品i的兴趣，因为使用的是单一行为的隐反馈数据，所以所有的Rvi=1。        

在UserCF2中我们已经介绍过了如何计算与目标用户最相似的K个用户，接下来就是第二步，推荐商品了。

在UserCF2.ipynb代码中主要核心是：       

'''

def calcuteInterest(frame,similarSeries,targetItemID):
    计算目标用户对目标物品的感兴趣程度
    :param frame: 数据
    :param similarSeries: 目标用户最相似的K个用户
    :param targetItemID: 目标物品
    :return:感兴趣程度
    similarUserID = similarSeries.index                                                 #和用户兴趣最相似的K个用户
    similarUsers = [frame[frame['UserID'] == i] for i in similarUserID]                 #K个用户数据
    similarUserValues = similarSeries.values                                            #用户和其他用户的兴趣相似度
    UserInstItem = []
    for u in similarUsers:                                                              #其他用户对物品的感兴趣程度
        if targetItemID in u['MovieID'].values: UserInstItem.append(u[u['MovieID']==targetItemID]['Rating'].values[0])
        else: UserInstItem.append(0)
    interest = sum([similarUserValues[v]*UserInstItem[v]/5 for v in range(len(similarUserValues))])
    return interest
    
'''

