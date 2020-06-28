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

举个![](https://static001.geekbang.org/resource/image/f2/82/f23f88a18b1227398c2ab3ef445d5382.png)

### 随机森林( Random Forest )

### 逻辑回归( Logistic Regression)

### 支持向量机( SVM)

### K近邻( KNN)

### K均值(K-mean)

### 朴素贝叶斯( Naive Bayes)

### Apriori

### EM模型

### AdaBoost

### XGBoost

### LightGBM



## 经典问题

### 样本不均衡的处理方法

### K-means的聚类流程

### LR的原理

### 1TB的数据排序


