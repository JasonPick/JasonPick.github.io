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

ROC是一条线，AUC是曲线下的面积

人们通常用ROC来决定选哪个threshold更好

用AUC来判断哪个模型更好





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

### 决策树( Decision Tree )

决策树基本上就是把我们以前的经验总结出来。例如基于湿度、温度等来判断是否会下雨


我们经常用的决策树是CART决策树，使用 _Gini impurity_ 来衡量纯度

还有其他种类的决策树如ID3(使用信息商增益来衡量纯度)和C4.5（使用信息商的增益率来衡量纯度）


信息熵(entropy)如下：

![](https://static001.geekbang.org/resource/image/74/d5/741f0ed01c53fd53f0e75204542abed5.png)

信息增益

![](https://static001.geekbang.org/resource/image/bf/34/bfea7626733fff6180341c9dda3d4334.png)

举个例子，如果条件D将子集划分成为D1:3，D2：7的时候那么信息熵增益表示如下

![](https://static001.geekbang.org/resource/image/f2/82/f23f88a18b1227398c2ab3ef445d5382.png)


对于Gini impurity 选择具有最小Gini impurity的条件


**CART决策树的剪枝**

CART决策树的剪枝主要采用的是CCP方法，它是一种后剪枝的方法，英文全称叫做cost-complexity prune，中文叫做代价复杂度。这种剪枝方式用到一个指标叫做节点的表面误差率增益值

公式如下：

![](https://static001.geekbang.org/resource/image/6b/95/6b9735123d45e58f0b0afc7c3f68cd95.png)

 - 其中Tt代表以t为根节点的子树，
 - C(Tt)表示节点t的子树没被裁剪时子树Tt的误差
 - C(t)表示节点t的子树被剪枝后节点t的误差
 - |Tt|代子树Tt的叶子数，剪枝后，T的叶子数减少了|Tt|-1。

所以节点的表面误差率增益值等于节点t的子树被剪枝后的误差变化除以剪掉的叶子数量。

因为我们希望剪枝前后误差最小，所以我们要寻找的就是最小α值对应的节点，把它剪掉。这时候生成了第一个子树。重复上面的过程，继续剪枝，直到最后只剩下根节点，即为最后一个子树。





**如何处理数字型的数据呢**

如果有属性是数字的属性，那么做以下步骤：

* 首先将这一属性排序

* 然后，计算相邻矩阵的平均值

* 把每一个平均值都试一下当作分类的标准，计算gini impurity，选取最小的那个。

**rank data**

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

SVM容忍outliers

SVM的大体思想是：将低维数据移向高维，并通过一条现来分开不同类别。

但是SVM并不是真正的移动数据，只是看作是高维的

核函数


### K近邻( KNN)

投票机制，离我最近的k个邻居都是什么类别，我们选择投票最高的邻居的类别


### K均值(K-mean)

选择一个合适的k

随机选择三个点，作为initial cluster的中心

那么遍历所有点，计算这个点离哪个cluster center最近，归类为这个cluster

我们现在有了一个cluster的结果，对于每个cluster，重新计算均值，计算center

重复上述步骤，直到center不发生变化

如何选择k值呢

有一个拐点的值，正好的最好的k


### 朴素贝叶斯( Naive Bayes)

朴素贝叶斯常常用于文本分类情感分析

**为什么叫朴素贝叶斯呢？**

因为假设所有的变量都是独立的，这种假设因此有了naive bayes

多项式朴素贝叶斯通常用于分类问题

使用词袋的方法

high bias low variance，准确率高泛化能力不强

### Apriori



### EM模型



### AdaBoost

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
 

   

### XGBoost

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


