---
layout: post
title: ML经典算法
subtitle: Part I：整理
cover-img: /assets/img/path.jpg
tags: [ml]
categories: ['Machine Learning']
---


借由这篇文章对经典的机器学习算法进行梳理，查漏补缺，希望能讲得清晰明了🌵


## 基础


首先我们先来了解一下基础知识和概念


### 混淆矩阵(Confusion Matrix)


混淆矩阵经常用于评估分类模型的表现如何

一个混淆矩阵通常如下图所示：

![](https://miro.medium.com/max/712/1*Z54JgbS4DUwWSknhDCvNTQ.png)

其中矩阵中的内容具体如下：

 - true positives (TP): 预测为真，确实也为真

 - true negatives (TN): 预测为假确实也为假

 - false positives (FP): 预测为真，但实际上是假的

 - false negatives (FN): 预测为假，但是实际上是真的
 
 
 混淆矩阵的size一般取决于要分类列的domain中有多少个类别。
 
 
 sensitivity = TP/（TP+FN）
 
 specifity = TN/（TN + FP）
 
 

### Covariance 与 Correlation

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/260ae33870c64fd3fbd31bfc9919e051e007d746)

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/abb59e2e3b35558a610bc00a7c488ccf341d21d1)

* covariance 决定了一个关系是positive trend还是nagative trend,或者是没有关系

但是这种方式有一个缺点，对于scale特别敏感，两个直线的斜度可能是一样的，但是2的scale是1的100X，那么covariance也不一样了


* correlation 就克服了这个缺点，correlation的取值范围为[-1,1]

 - 两个变量的关系是强正向关系：correlation = 1
 
 - 两个变量的关系是强负向关系：correlation = -1
 
 - 两个变量的关系不明显就在0左右


### R^2


![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Coefficient_of_Determination.svg/800px-Coefficient_of_Determination.svg.png)

左边的图是点的值离均值的距离的平方，就是我们这里的var(mean)

右边的图代表点的值离预测的line的距离的平方，就是我们这里的var(line)


var(mean) ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/aec2d91094ee54fbf0f7912d329706ff016ec1bd)

var(line)![](https://wikimedia.org/api/rest_v1/media/math/render/svg/107a9fb71364b9db3cf481e956ad2af11cba10a1)


R^2 = (var(mean) - var(line))/var(mean)


### AUC 和 ROC

受试者工作特征曲线（receiver operating characteristic curve），简称ROC 曲线

ROC 曲线描述的是真正例率和假正例率之间的关系，，也就是收益（真正例）与代价（假正例）之间的关系

* FPR假正例率：FP/(FP+TN)

* TPR假正例率：TP/（TP+FN）

ROC 空间将 FPR 定义为 X 轴，TPR 定义为 Y 轴，并映射在由 (0, 0)、(0, 1)、(1, 0)、(1, 1) 四个点围成的正方形里。在这个正方形里，从 (0, 0) 到 (1, 1) 的对角线代表了一条分界线，叫作无识别率线，它将 ROC 空间划分为左上／右下两个区域。


完美的模型体现在 ROC 空间上的 (0, 1) 点：

$FPR = 0$ 意味着没有假正例，没有负例被掺入；$TPR = 1$ 意味着没有假负例，没有正例被遗漏。也就是说，不管分类器输出结果是正例还是反例，都是 100% 完全正确。

不同类型的模型具有不同的 ROC 曲线。决策树这类模型会直接输出样例对应的类别，也就是硬分类结果，其 ROC 曲线就退化为 ROC 空间上的单个点。相比之下，朴素贝叶斯这类输出软分类结果，也就是属于每个类别概率的模型就没有这么简单了。将软分类概率转换成硬分类结果需要选择合适的阈值，每个不同的阈值都对应着 ROC 空间上的一个点，因此整个模型的性能就是由多个离散点连成的折线。
阈值的选择是至关重要的，一般选择ROC上的拐点位置。下图是一个ROC曲线

![](https://static001.geekbang.org/resource/image/70/cf/7099084f10b6fd014b198ef0a13c57cf.png)

模型的 ROC 曲线越靠近左上方，其性能就越好，更具体的评估需要AUC

AUC是曲线下的面积，因此其取值必然在 0 到 1 之间

用AUC来判断哪个模型更好，对于完全随机线，其AUC是0.5，如果


### PCA 


维数灾难：

一般的经验法则是每个属性维度需要对应至少 5 个数据样本，可当属性维数增加而样本数目不变时，过少的数据就不足以体现出属性背后的趋势，从而导致过拟合的发生。


降维的方法：

 - 特征选择 直接砍掉一些特征
 - 特征提取 将原始的属性整合成为新的属性
 
 PCA将原来的点shift到avg为原点的图，这个时候画一条线来fit图形，找到一个line使得点到line的距离最小或者使得点map到line上距离原点平方和最大。
 
 这条线叫PC1，slope = 0.25，我们mix4个g1和1个g2，计算一个unit vector，
 
 ![](/assets/img/blog/Snip20200628_33.png)
 
 PC2是过原点与PC1垂直的线，然后我们将PC1旋转至水平，重新画数据点
 
 ![](/assets/img/blog/Snip20200628_34.png)
 
 ![](/assets/img/blog/Snip20200628_35.png)
 
 
 ![](/assets/img/blog/Snip20200628_36.png)
 

### 梯度提升(Gradient Boosting)


**什么是boosting？**


Boosting是将一个弱学习方法变成强学习方法。

在Boosting中每个新树都fit原来数据的变种。

首先我们了解一下AdaBoost的原理：

 - 在Adaboost中，首先训练一个决策树，其中每个观测值都分配了相等的权重
 
 - 在评估第一棵树之后，我们增加那些难以分类的观测值的权重，并降低那些易于分类的观测值的权重。第二个树生长在此加权数据上，循环往复
 
 - 最终的最终组合模型产生的预测是之前树模型预测的加权和
 
 
 与AdaBoost很相似,Gradient Boosting也是通过对上一轮分类错误的总结来更好的训练模型。
 
 AdaBoost采用的是增加上一轮学习错误样本的权重的策略，而在Gradient Boosting中则将负梯度作为上一轮基学习器犯错的衡量指标


**回归**

当一个Gradient Boost用于预测连续的值，我们说用Gradient Boost来回归


* 使用一个叶子结点

* 创建一个树

* sclae 这个树



  STEP 1: 初始化
  
    ![](https://www.zhihu.com/equation?tex=f_0%28x%29+%3D+%5Cmathop%7B%5Carg%5Cmin%7D%5Climits_%5Cgamma+%5Csum%5Climits_%7Bi%3D1%7D%5EN+L%28y_i%2C+%5Cgamma%29)
         
         
  STEP 2: 
         for m :1 -> M:
         
   (A) 计算负梯度：
   
    ![](https://www.zhihu.com/equation?tex=%5Ctilde%7By%7D_i+%3D+-%5Cfrac%7B%5Cpartial+L%28y_i%2Cf_%7Bm-1%7D%28x_i%29%29%7D%7B%5Cpartial+f_%7Bm-1%7D%28x_i%29%7D%2C+%5Cqquad+i+%3D+1%2C2+%5Ccdots+N)
         

    (B) 通过最小化平方误差
    
    ![](https://www.zhihu.com/equation?tex=%5Cqquad+w_m+%3D+%5Cmathop%7B%5Carg%5Cmin%7D%5Climits_w+%5Csum%5Climits_%7Bi%3D1%7D%5E%7BN%7D+%5Cleft%5B%5Ctilde%7By%7D_i+-+h_m%28x_i%5C%2C%3B%5C%2Cw%29+%5Cright%5D%5E2)

    (C) 使用line search确定步长 
    
    ![](https://www.zhihu.com/equation?tex=%5Cqquad+%5Crho_m+%3D+%5Cmathop%7B%5Carg%5Cmin%7D%5Climits_%7B%5Crho%7D+%5Csum%5Climits_%7Bi%3D1%7D%5E%7BN%7D+L%28y_i%2Cf_%7Bm-1%7D%28x_i%29+%2B+%5Crho+h_m%28x_i%5C%2C%3B%5C%2Cw_m%29%29)
    
    (D) 迭代
    
    ![](https://www.zhihu.com/equation?tex=f_m%28x%29+%3D+f_%7Bm-1%7D%28x%29+%2B+%5Crho_m+h_m%28x%5C%2C%3B%5C%2Cw_m%29)

 STEP 3:输出![](https://www.zhihu.com/equation?tex=f_M%28x%29)




#### 梯度提升用于分类(Gradient Boosting Classification)





## 机器学习经典算法


我们总结几个用于数据挖掘的经典机器学习算法，对每个算法描述了其原理以及对应的需要注意的地方。


### 决策树( Decision Tree )

* 决策树：

决策树基本上就是把我们以前的经验总结出来。例如基于湿度、温度等来判断是否会下雨


* 种类：

我们经常用的决策树是CART决策树，使用 _Gini impurity_ 来衡量纯度

还有其他种类的决策树如ID3(使用信息商增益来衡量纯度)和C4.5（使用信息商的增益率来衡量纯度）

 * 纯度的衡量方法：

  - 信息熵(entropy)如下：

  ![](https://static001.geekbang.org/resource/image/74/d5/741f0ed01c53fd53f0e75204542abed5.png)

  - 信息增益

  ![](https://static001.geekbang.org/resource/image/bf/34/bfea7626733fff6180341c9dda3d4334.png)

  举个例子，如果条件D将子集划分成为D1:3，D2：7的时候那么信息熵增益表示如下

  ![](https://static001.geekbang.org/resource/image/f2/82/f23f88a18b1227398c2ab3ef445d5382.png)


  - 对于Gini impurity 选择具有最小Gini impurity的条件
  
  ![](https://static001.geekbang.org/resource/image/f9/89/f9bb4cce5b895499cabc714eb372b089.png)


* **CART决策树的剪枝**

CART决策树的剪枝主要采用的是CCP方法，它是一种后剪枝的方法，英文全称叫做cost-complexity prune，中文叫做代价复杂度。这种剪枝方式用到一个指标叫做节点的表面误差率增益值

公式如下：

![](https://static001.geekbang.org/resource/image/6b/95/6b9735123d45e58f0b0afc7c3f68cd95.png)

 - 其中Tt代表以t为根节点的子树，
 - C(Tt)表示节点t的子树没被裁剪时子树Tt的误差
 - C(t)表示节点t的子树被剪枝后节点t的误差
 - \|Tt\|代子树Tt的叶子数，剪枝后，T的叶子数减少了\|Tt\|-1。

所以节点的表面误差率增益值等于节点t的子树被剪枝后的误差变化除以剪掉的叶子数量。

因为我们希望剪枝前后误差最小，所以我们要寻找的就是最小α值对应的节点，把它剪掉。这时候生成了第一个子树。重复上面的过程，继续剪枝，直到最后只剩下根节点，即为最后一个子树。

在众多结果中选择gini最小的一个



* **如何处理数字型的数据呢**

如果有属性是数字的属性，那么做以下步骤：

* 首先将这一属性排序

* 然后，计算相邻矩阵的平均值

* 把每一个平均值都试一下当作分类的标准，计算gini impurity，选取最小的那个。


* **rank data**

和上面的步骤相似，不过这个不用计算avg，直接用rank<=1当作一个条件计算gini


优点：很容易建立，并且很容易解释

缺点：特别容易overfit，可能限于局部最小值。


### 随机森林( Random Forest )

使用随机森林

**如何处理缺失值**

proximity临近矩阵，思想是如果两个sample在一个叶子结点结束，那么判断这两个sample是相似的。



**如果sample中有确实值如何处理**

例如

```
|CP | GB | BA | weight | HD |
| Y |  N | ?  | 168    | ?  |

其中HD是要预测的值，但是BA缺失
```

- 1.首先拷贝一份，这条记录，两份分别填上HD的两种可能值 Y 或 N

- 2.猜BA是什么（推断）

- 3.沿着树向下，找到最正确的那一个，填回BA


优点：适用于低维数据，准确率高



### 线性回归


线性回归一般使得R2最小化来实现最好估计

'R^2 = (var(mean) - var(line))/var(mean)'

R^2在\[0,1\]上取值，当R^2 = 0的时候代表没有关系，反之，当R^2 = 1的时候代表有关系。






### 逻辑回归( Logistic Regression)


逻辑回归通常用于预测true or false 这种问题。

属性可以是离散的也可以是连续的

与linear regression不同的是使用最大似然估计而不是R2来衡量

是一个伯努利分布





### 支持向量机( SVM)


先来看一个例子，我们要怎么把这样的小球分为两类呢？

![](https://static001.geekbang.org/resource/image/14/eb/144d72013a808e955e78718f6df3d2eb.jpg)

SVM帮我们找到了方法,二维平面变成了三维空间。原来的曲线变成了一个平面。这个平面，我们就叫做超平面

![](https://static001.geekbang.org/resource/image/f3/34/f3497cd97c8bb06e952efcdae6059434.jpg)


{:.box-warning}

**SVM的大体思想是：将低维数据移向高维，并通过一个超平面现来分开不同类别。**

用SVM计算的过程就是帮我们找到那个超平面的过程，这个超平面就是我们的SVM分类器。

注意：但是SVM并不是真正的移动数据，只是看作是高维的


* SVM为了实现robust，增加对outlier的容忍度增加了一个“分类间隔”，英文叫做margin。

超过了这个margin就会出现分类错误

![](https://static001.geekbang.org/resource/image/50/ea/506cc4b85a9206cca12048b29919a7ea.jpg)


* **核函数(超平面)**

数学表达式为

![](https://static001.geekbang.org/resource/image/76/28/765e87a2d9d6358f1274478dacbbce28.png)

w、x是n维空间里的向量，其中x是函数变量；w是法向量。


* **margin**

margin是一个类别到超平面的最小距离，表示为

![](https://static001.geekbang.org/resource/image/83/76/8342b5253cb4c294c72cef6802814176.png)


* **SVM的分类**

  SVM有硬间隔、软间隔和非线性的SVM，其中硬间隔不允许分类错误，软间隔允许一定量的分类错误，非线性，取决于kernal的选择。
  
  
* **如果想要用SVM做多分类而不是二分类问题怎么办**

  针对这种情况，我们可以将多个二分类器组合起来形成一个多分类器，常见的方法有“一对多法”和“一对一法”两种。
  
   - 一对多法
   
     ```
     假设我们要把物体分成A、B、C、D四种分类，那么我们可以先把其中的一类作为分类1，其他类统一归为分类2。这样我们可以构造4种SVM，分别为以下的情况：

     （1）样本A作为正集，B，C，D作为负集；

     （2）样本B作为正集，A，C，D作为负集；

     （3）样本C作为正集，A，B，D作为负集；

     （4）样本D作为正集，A，B，C作为负集。

     这种方法，针对K个分类，需要训练K个分类器，分类速度较快，但训练速度较慢，因为每个分类器都需要对全部样本进行训练，
     
     而且负样本数量远大于正样本数量，会造成样本不对称的情况，而且当增加新的分类，比如第K+1类时，需要重新对分类器进行构造。
     ```
   - 一对一法
   
   的初衷是想在训练的时候更加灵活。我们可以在任意两类样本之间构造一个SVM，这样针对K类的样本，就会有C(k,2)类分类器。
   
   ```
   比如我们想要划分A、B、C三个类，可以构造3个分类器：

   （1）分类器1：A、B；

   （2）分类器2：A、C；

   （3）分类器3：B、C。

   当对一个未知样本进行分类时，每一个分类器都会有一个分类结果，即为1票，最终得票最多的类别就是整个未知样本的类别。

   这样做的好处是，如果新增一类，不需要重新训练所有的SVM，只需要训练和新增这一类样本的分类器。而且这种方式在训练单个SVM模型的时候，训练速度快。

   但这种方法的不足在于，分类器的个数与K的平方成正比，所以当K较大时，训练和测试的时间会比较慢。
   ```


   
      

### K近邻( KNN)


投票机制，离我最近的k个邻居都是什么类别，我们选择投票最高的邻居的类别

**K值的选择**

* 如果 K 值比较小

  就相当于未分类物体与它的邻居非常接近才行。这样产生的一个问题就是，如果邻居点是个噪声点，那么未分类物体的分类也会产生误差，这样KNN分类就会产生过拟合。

* 如果K值比较大

  相当于距离过远的点也会对未知物体的分类产生影响，虽然这种情况的好处是鲁棒性强，但是不足也很明显，会产生欠拟合情况，也就是没有把未分类物体真正分类出来。


在工程上，我们一般采用交叉验证的方式选取 K 值。

交叉验证的思路就是，把样本集中的大部分样本作为训练集，剩余的小部分样本用于预测，来验证分类模型的准确性。

所以在KNN算法中，我们一般会把K值选取在较小的范围内，同时在验证集上准确率最高的那一个最终确定作为K值。


**距离的计算方法**

* 欧氏距离:最长用的距离公式

![](https://static001.geekbang.org/resource/image/40/6a/40efe7cb4a2571e55438b55f8d37366a.png)

* 曼哈顿距离：在几何中应用较多

![](https://static001.geekbang.org/resource/image/bd/aa/bda520e8ee34ea19df8dbad3da85faaa.png)

* 闵可夫斯基距离:一组距离

![](https://static001.geekbang.org/resource/image/4d/c5/4d614c3d6722c02e4ea03cb1e6653dc5.png)

* 切比雪夫距离

二个点之间的切比雪夫距离就是这两个点坐标数值差的绝对值的最大值，用数学表示就是：max(|x1-y1|,|x2-y2|)



**KD树**

KNN的计算过程是大量计算样本点之间的距离。为了减少计算距离次数，提升KNN的搜索效率，人们提出了KD树（K-Dimensional的缩写）。

KD树是对数据点在K维空间中划分的一种数据结构。

在KD树的构造中，每个节点都是k维数值点的二叉树。既然是二叉树，就可以采用二叉树的增删改查操作，这样就大大提升了搜索效率。




### K均值(K-mean)


选择一个合适的k

随机选择三个点，作为initial cluster的中心

那么遍历所有点，计算这个点离哪个cluster center最近，归类为这个cluster

我们现在有了一个cluster的结果，对于每个cluster，重新计算均值，计算center

重复上述步骤，直到center不发生变化

如何选择k值呢

有一个拐点的值，正好的最好的k


**区分K-Means和KNN这两个算法**

 - 首先，这两个算法解决数据挖掘的两类问题。K-Means是聚类算法，KNN是分类算法。

 - 这两个算法分别是两种不同的学习方式。K-Means是非监督学习，也就是不需要事先给出分类标签，而KNN是有监督学习，需要我们给出训练数据的分类标识。

 - 最后，K值的含义不同。K-Means中的K值代表K类。KNN中的K值代表K个最接近的邻居




### 朴素贝叶斯( Naive Bayes)

* **贝叶斯原理**

![](https://static001.geekbang.org/resource/image/99/4b/99f0e50baffa2c572ea0db6c5df4474b.png)

朴素贝叶斯常常用于文本分类情感分析

* **为什么叫朴素贝叶斯呢？**

因为假设所有的变量都是独立的，这种假设因此有了naive bayes


多项式朴素贝叶斯通常用于分类问题

举个例子：

|编号｜身高｜体重｜男女|
|:----|:----|:----|:----|
|1 |高｜ 重｜男｜
|2 |矮｜ 轻｜女｜

如果使用这种方法来预测是男女的话，假设我们用A代表属性，用A1, A2分别为身高=高、体重=中。一共有两个类别，假设用C代表类别，那么C1,C2分别是：男、女，在未知的情况下我们用Cj表示。

那么我们的预测公式为

![](https://static001.geekbang.org/resource/image/55/64/556ae2a160ce37ca48a456b7dc61e564.png)

求得P(C1|A1A2A3)和P(C2|A1A2A3)的概率即可，然后比较下哪个分类的可能性大，就是哪个分类结果。



### Apriori


Apriori是关联规则挖掘的重要算法，帮助我们发现项与项（item与item）之间的关系。

* **几个关于关联规则的重要概念**

使用一个例子🌰进行说明：有如下表格

![](https://static001.geekbang.org/resource/image/f7/1c/f7d0cc3c1a845bf790b344f62372941c.png)

* 支持度

  支持度是个百分比，它指的是某个商品组合出现的次数与总次数之间的比例。

  '“牛奶”的支持度是4/5=0.8。'


* 置信度

  置信度是个条件概念，就是说在A发生的情况下，B发生的概率是多少。它指的就是当你购买了商品A，会有多大的概率购买商品B，

  '我们能看到，在4次购买了牛奶的情况下，有2次购买了啤酒，所以置信度(牛奶→啤酒)=0.5，而在3次购买啤酒的情况下，有2次购买了牛奶，所以置信度（啤酒→牛奶）=0.67。'


* 提升度

  代表的是“商品A的出现，对商品B的出现概率提升的”程度。
  
  '提升度(A→B)=置信度(A→B)/支持度(B)'
  
  提升度有三种可能：

     - 提升度(A→B)>1：代表有提升；

     - 提升度(A→B)=1：代表有没有提升，也没有下降；

     - 提升度(A→B)<1：代表有下降。

 
 **Aproiri的工作原理**
 
 先设置一个最小支持度，如果支持度小于这个值的话那么就不继续考虑它，不频繁集的子集也不会是一个频繁集
 
 
 1. 首先，我们先计算单个商品的置信度，也就是得到K=1项的置信度
 
 2. 于是筛选商品的频繁项集
 
 3. 两两组合获得k = 2的支持度
 
 4. 筛选频繁集
 
 5. k = 3的支持度
 
 6. 筛选后得到结果
  
  
**Apriori在计算的过程中有以下几个缺点**

        可能产生大量的候选集。因为采用排列组合的方式，把可能的项集都组合出来了；

        每次计算都需要重新扫描数据集，来计算每个项集的支持度。



**FP-Growth算法**

为了改进Apriori浪费时间空间的缺点

 - 创建了一棵FP树来存储频繁项集。在创建前对不满足最小支持度的项进行删除，减少了存储空间。

 - 整个生成过程只遍历数据集2次，大大减少了计算量。


FP-Growth的原理：

1. 创建项头表的作用是为FP构建及频繁项集挖掘提供索引。

   这一步的流程是先扫描一遍数据集，对于满足最小支持度的单个项（K=1项集）按照支持度从高到低进行排序，这个过程中删除了不满足最小支持度的项。

2. 再次扫描生成FP-Growth

  ![](https://static001.geekbang.org/resource/image/ea/92/eadaaf6585379815e62aad99386c7992.png)
  
3. FP树进行挖掘

  以“啤酒”的节点为例，从FP树中可以得到一棵FP子树
  
  ![](https://static001.geekbang.org/resource/image/99/0f/9951cda824fc9823136231e7c8e70d0f.png)

4. 面包的频繁

  ![](https://static001.geekbang.org/resource/image/41/13/41026c8f25b64b01125c8b8d6a19a113.png)

 
 

### EM模型


EM的英文是Expectation Maximization，所以EM算法也叫最大期望算法。

**工作原理**

EM算法是一种求解最大似然估计的方法，通过观测样本，来找出样本的模型参数。

分为EM两部分，EM算法中的E步来进行观察，然后通过M步来进行调整A和B的参数，最后让碟子A和碟子B的参数不再发生变化为止。


例子：我们以抛硬币为例，抛A和B两个硬币十次，记录下正面的次数，前提是我们不知道这个硬币是A还是B，有表格如下

![](https://static001.geekbang.org/resource/image/c8/e4/c8b3f2489735a21ad86d05fb9e8c0de4.png)

- 1.初始化参数。我们假设硬币A和B的正面概率（随机指定）是θA=0.5和θB=0.9。

- 2.计算期望值。假设实验1投掷的是硬币A，那么正面次数为5的概率为：

   ![](https://static001.geekbang.org/resource/image/09/e0/09babe7d1f543d6ff800005d556823e0.png)
   
   假设实验1是硬币B，那么概率为
   
   ![](https://static001.geekbang.org/resource/image/7b/f6/7b1bab8bf4eecb0c55b34fb8049374f6.png)
   
   然后我们对实验2~5重复上面的计算过程，可以推理出来硬币顺序应该是{A，A，B，B，A}。
     
- 3.通过猜测的结果{A, A, B, B, A}来完善初始化的参数θA和θB。

    然后一直重复第二步和第三步，直到参数不再发生变化。


**EM聚类**


相比于K-Means算法，EM聚类更加灵活，比如下面这两种情况，K-Means会得到下面的聚类结果。

![](https://static001.geekbang.org/resource/image/ba/ca/bafc98deb68400100fde69a41ebc66ca.jpg)

而EM模型会得到下面的结果：

![](https://static001.geekbang.org/resource/image/18/3b/18fe6407b90130e5e4fa74467b1d493b.jpg)


**why**

因为K-Means是通过距离来区分样本之间的差别的，且每个样本在计算的时候只能属于一个分类，称之为是硬聚类算法。

而EM聚类在求解的过程中，实际上每个样本都有一定的概率和每个聚类相关，叫做软聚类算法。


* **EM算法的各种模型**

EM算法可以理解成为是一个框架，在这个框架中可以采用不同的模型来用EM进行求解。

常用的EM聚类有GMM高斯混合模型和HMM隐马尔科夫模型。

GMM（高斯混合模型）聚类就是EM聚类的一种。比如上面这两个图，可以采用GMM来进行聚类。


**GMM**

* E：通常，我们可以假设样本是符合高斯分布的。每个高斯分布都属于这个模型的组成部分（component），要分成K类就相当于是K个组成部分。这样我们可以先初始化每个组成部分的高斯分布的参数，然后再看来每个样本是属于哪个组成部分。这也就是E步骤。

* M：再通过得到的这些隐含变量结果，反过来求每个组成部分高斯分布的参数，即M步骤。反复EM步骤，直到每个组成部分的高斯分布参数不变为止。

这样也就相当于将样本按照GMM模型进行了EM聚类。




### AdaBoost


{:.box-note}

集成算法一般有两种Boost和Bagging

* bagging的场景类似把专家召集到一个会议桌前,当做一个决定的时候,让 K 个专家(K 个模型)分别进行分类,然后选择出现次数最多的那个类作为最终的分类结果。

* Boosting再学习相当于把 K 个专家(K 个分类器)进行加权融合,形成一个新的超级专家(强分类器),让这个超级专家做判断。


**AdaBoost 的英文全称是 Adaptive Boosting,中文含义是自适应提升算法。**

AdaBoost由多个弱分类器加权得到一个强分类器

 ![](/assets/img/blog/Snip20200628_31.png)

权重的选择如下：其中e代表分类错误率

![](/assets/img/blog/Snip20200628_32.png)


AdaBoost与Random Forest 不同：

* Random Forest构建一个完整的树，Adaboost构建一个树桩(stump)，树桩包含一个或者两个叶子结点

* AdaBoost的每个stump的weight都不一样，而RF的权重都相等

* AdaBoost的出现的顺序很重要，而RF中所有的树都是独立的

步骤：

 1）初始的每个stump的weight， _weight = 1/num_
 
 2) 找到gini最小的属性
 
 3） 有两个概念 _total_error = 分类错误的数量 * weight_
 
   - _amount_of_say = 1/2 * log((1-total_error)/total_error)_
 
 4) 更新weight  
   
    - 如果出现了错误，那么增加weight _new weight = old weight * e^(amount of say)_
    - 如果分类正确，那么减少weight _new weight = old weight * e^(-amount of say)_
    
    然后将weightnormalize一下，使得和为1
    
 5） 新的sample的集合，
 


### PageRank


 PageRank 算法目的就是要找到优质的网页,越优质的网页排序越高
 
 举个例子🌰
 
 ![](/assets/img/blog/Snip20200628_27.png)
 
 这代表了互联网中网页的相互引用
 
 这个图中包含了入链和出链，一个网页的影响力为入链网页的影响力加权之和
 
 ![](/assets/img/blog/Snip20200628_28.png)
 
 上图中的PR计算就是它的转移矩阵乘以它的影响力矩阵，其中影响力矩阵初始化为1/4，不断迭代直到结果不变为止
 
 ![](/assets/img/blog/Snip20200628_29.png)
 
 
 **PageRank中存在的问题**
 
 等级泄漏和等级沉没问题
 
 * 等级泄漏：一个网站没有出链，那么最后其他网站都会变为0
 
 * 等级沉没:一个网站只有出链，没有入链，那么最后网站PR会变成0
 
 
 解决方法：
 
 PR随机浏览模型：除了用户自己跳转以外，还有小概率随机浏览其他任意页面
 

 ![](/assets/img/blog/Snip20200628_29.png)
 
其中d是阻尼因子，1-d = 0.15代表用户直接通过输入网址来访问页面的概率，N是网页总数




### XGBoost


**Regression工作原理**

我们使用例子 药量-药的作用 

1. 初始预测 药的作用 = 0.5

2. 和GBoost一样，XGBoost也是使用residual来fit一个树

```
第一步：

每个tree开始的时候只有一个叶子结点，记录了所有的residuals，现在来计算一个similarity score

similarity score = sum of residuals,squared/(number of residuals + lambda)

lambda是规则化参数，减少单个点对于整体模型的影响程度，防止过拟合，lamda越大增益越小

正负的residual抵消了,similarity = 4

第二步：

（1）是否把这些residuals分成不同类会表现更好

avg(dosage) = 15, 用dosage<15来区分{-10.5}{6.5,7.5,-7.5}

left = 110.25  right = 14.08

现在来计算叶子的residual比root的好多少
Gain = left +right - simlarity(root)

Gain = 120.33,

继续try不同的threshold   dosage<22.5 gain = 4; dosage <30, gain =56.33;  

都小于dosage<15,所以dosage<15作为第一个结点


（2）继续分裂，{6.5,7,5,-7,5} 还可以分，

   dosage < 22.5, gain = 22.7, dosage < 30, gain = 140.7,
   choose dosage < 30 


第三步骤：剪枝

选择一个gama值，例如gama = 130， 

自底向上扫描
  if gain-gama < 0 , remove the branch
  else do nothing
  
如果底下的branch没有移除，那么上面的就算差是负数也不会移除

第四步，计算output value

output value =  sum of residuals/(num of residuals + lambda)



```

新一轮的训练

![](/assets/img/blog/Snip20200628_37.png)

learning rate called eta ,default 0.3


### LightGBM



## 经典问题

### 样本不均衡的处理方法


**正负样本是什么比例叫做样本不均衡呢？**

当正负样本比例超过1:3的时候就出现来不均衡

**不均衡的样本有什么后果呢**

负样本的Recall高，正样本的precision高Recall低

**解决方法**

解决问题主要是两种思路：减少正样本的数量或者增加负样本的数量

  * 欠采样法(undersampleing)：减少正样本的数量，根据采样使得数量级相近
  
       但是采样不当会造成重要信息的丢失，所以要小心
       
       方法： - 分为核心与非核心样本，从非核心样本中删除数据
             - 将原来的大样本划分为N个集合，将划分后的集合与少量样本结合形成N个训练集
  
  
  
  * 过采样法：增加负样本的数量
  
     要是直接复制少量样本进数据集中，只增加了少量样本的数量但是没增加具体信息，容易造成过拟合问题
     
     方法： - 在少量样本中加入随机高斯噪声等，如SMOTE方法
           - 阈值移动：一般我们分类将阈值设置为0.5，但是当样本不均衡的时候，我们可以自定义阈值
                    
                    'y/1-y > m+/m-'


### K-means的聚类流程

```
1. 给定k值，随机选择k个点作为聚类中心

2. 计算每个点到k个聚类中心的欧式距离，找到最小的距离，归类为那个聚类

3. 重新计算聚类中心

4. 重复2-3步骤，直到聚类结果不再发生变化
```

**既然初始值是随机选择的那么10个k—means模型中怎么知道哪一个模型好**

可以使用轮廓系数，计算内聚度和不相似度，

  - 内聚度衡量的是一个cluster内部的相似程度 a
  - 不相似度是衡量各个cluster之间的区分度 b
  
  ![](https://pic1.zhimg.com/80/v2-96c450cae7ea32f836cd841238a311b8_1440w.png)
  
  由公式可知，S取值范围为（-1, 1），当S越接近于1，则聚类效果越好，月接近-1，聚类效果越差。
  
  
  或者可以使用交叉验证
  
  把数据集分为m个block，每次用一个block作为验证集，使得cluster内部距离的平方和最小(SSE)
  
  
 **k-prototype**
 
 
 一种kmeans的变种，可以处理离散的属性值 'D = p1 + a * p2 '
 

### LR的原理


 LR简单来说，将属性值输入一个LR模型中，会判断属性值是属于哪一类，A的概率是p，B的概率是1-p
 
 如果p > 0.5那么认为属于A类，否则属于B类


### 1TB的数据排序

ITB对于内存来说太大了，无法全部导入内存中去，那么我们采用分块的方法，将大文件分块成小文件

假设内存100M，1G数据分为10个100M，依次载入内存中进行排序，然后存入硬盘，

最后归并排序，将10个100M的前9M放入内存中，10M留做输出缓存区。


