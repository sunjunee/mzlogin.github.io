---
layout: post
title: 广义线性模型
categories: [机器学习]
description: 广义线性模型
keywords: 机器学习, 线性模型, Logistic回归, 岭回归, Lasso回归
---
所谓广义线性模型，如西瓜书所说，就是形式上是线性的，但是呢，实际上不一定是线性的方程。
一般的，线性模型是：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\hat{y}(w,x)=w^{T}X%2Bb" style="border:none;" />

更一般地，考虑单调可微函数g()，令：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\hat{y}(w,x)=g^{-1}(w^{T}X%2Bb)" style="border:none;" />

这样得到的模型称为“广义线性模型”，其中的函数g称为联系函数。

当g(x) = ln(x)时，称为对数线性回归。

下面所谈到的coef\_的是w，interception\_的是w0

## 1.1 一般的线性模型

下面是一系列的回归方法，输出结果理论上应该是输入的线性组合。用数学形式表示如下：

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\hat{y}(w,x)=w_{0} %2B w_{1}x_{1}%2B...%2Bw_{p}x_{p}" style="border:none;" />

其中w是因子，w_0是截距

### 1.1.1 最小二乘回归
最小二乘回归通过最小化观测值和预测值之间差值的平方，来找到最佳的权重w:

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\min_{w}||Xw-\hat{y}||_{2}^{2}" style="border:none;" />

<img src="http://scikit-learn.org/stable/_images/sphx_glr_plot_ols_0011.png" style="border:none;width: 320.0px; height: 240.0px;"/>

<a class="reference internal" href="http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression" title="sklearn.linear_model.LinearRegression"><code class="xref py py-class docutils literal"><span class="pre">LinearRegression</span></code></a>函数示例如下：


```python
from sklearn import linear_model
reg = linear_model.LinearRegression()
reg.fit ([[0, 0], [1, 1], [2, 2]], [0, 1, 2])

reg.coef_
```




    array([ 0.5,  0.5])



然而，普通最小二乘法的系数估计依赖于模型项的独立性。 当相关项和设计矩阵的列具有近似的线性相关性时，设计矩阵变得接近奇异，因此最小二乘估计对观测响应中的随机误差高度敏感，产生大的方差。 例如，当没有实验设计收集数据时，就会出现这种多重共线性的情况。

### 1.1.2 岭回归(Ridge Regression)
岭回归通过引入惩罚项，解决了最小二乘回归中的问题。惩罚因子是w的模的平方：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\min_{w}||Xw-\hat{y} ||_{2}^{2}%2B\alpha||w||_{2}^{2}" style="border:none;" />

这里的的a>=0是一个控制收缩性（shrinkage）的变量，越大则收缩越强，得到的w就对共线性（collinearity）越鲁棒

<img src="http://scikit-learn.org/stable/_images/sphx_glr_plot_ridge_path_0011.png" style="border:none;width: 480.0px; height: 360.0px;"/>




```python
from sklearn import linear_model
reg = linear_model.Ridge (alpha = .5)
reg.fit ([[0, 0], [0, 0], [1, 1]], [0, .1, 1])

print("coef_:", reg.coef_)

print("intercept_:", reg.intercept_)
```

    coef_: [ 0.34545455  0.34545455]
    intercept_: 0.136363636364


### 1.1.3 拉索（Lasso）回归

套索是估计**稀疏系数**的线性模型。 在某些情况下它是有用的，因为它倾向于选择具有较少参数值的解决方案，从而有效地减少给定解决方案所依赖的变量的数量。 出于这个原因，套索及其变体是压缩感测领域的基础。 在某些情况下，它可以恢复非零权值的精确集合。

其数学形式中，添加了L1正则化，目标函数如下：


<img src="http://chart.googleapis.com/chart?cht=tx&chl=\min_{w}||Xw-\hat{y} ||_{2}^{2}/2n%2B\alpha||w||_{1}" style="border:none;" />

它和岭回归十分相似，但是使用的是权重的1范数，即绝对值之和，而岭回归用的是二范数的平方，即我们通常说的距离的平方。


```python
from sklearn import linear_model
reg = linear_model.Lasso(alpha = 0.1)
reg.fit([[0, 0], [1, 1]], [0, 1])

reg.predict([[1, 1]])
```




    array([ 0.8])



### 1.1.4 多任务(Lasso)拉索

多任务针对的是稀疏系数下的多个回归结合的问题：y的是一个矩阵，大小是(n_samples, n_tasks)；也就是说，对于所有的回归问题而言，其所对应的特征是一样的，这些问题成为任务。


<img src="http://scikit-learn.org/stable/_images/sphx_glr_plot_multi_task_lasso_support_0011.png" style="border:none;width: 600.0px"/>


上图比较和多套索和单个套索的效果，多套索得到的W基本都是非0的。


其数学形式如下：

<img src="http://scikit-learn.org/stable/_images/math/fb8862ba8a0bbd86df74a2d45e115042abaaf337.png" style="border:none;width: 300.0px;"/>


其中：


<img src="http://scikit-learn.org/stable/_images/math/ab1b1502d6ca4bf16f01209e3b0a37593249271f.png" style="border:none;width: 150.0px;"/>


<img src="http://scikit-learn.org/stable/_images/math/95a01d8b63169b1769176434fe57ec5a02e43b0d.png" style="border:none;width: 160.0px;"/>

### 1.1.5 弹性网(Elastic Net)
弹性网回归，同时使用了L1和L2正则化。它能够和Lasso一样学习稀疏的模型，同时也能保持Ridge的正则化特性。
弹性网回归对于样本中，很多特征和其他特征相关的情况，十分有效。Lasso就像是从其中选择一个特征，但是弹性网则是全部保留下来。

其目标函数如下：


<img src="http://scikit-learn.org/stable/_images/math/fb8862ba8a0bbd86df74a2d45e115042abaaf337.png" style="border:none;width: 300.0px;"/>

### 1.1.6 多任务弹性网
类似于多任务套索，其目标函数如下：


<img src="http://scikit-learn.org/stable/_images/math/a0ef45ea26ee66211e7a17d2192da2a21e2ce714.png" style="border:none;width: 500.0px;"/>

### 1.1.7 最小角度回归
最小角度回归(LARS)针对的是高维数据，LARS类似于一个逐步的回归，在每一步，它会找出和结果最相关的预测变量。当多个预测变量具有相等的相关性时，不是沿着相同的预测变量继续，而是沿预测变量之间的等角方向进行。

优点：

* 当数据的维度远远大于样本点的数目时，LARS十分有效；
* 它产生的是一个分段线性的解路径，这在交叉验证或者微调模型时有效；

缺点：
* 对噪声敏感

### 1.1.8 LARS拉索
LARS lasso是基于LARS算法实现的Lasso，保持了LARS和Lasso的优点



```python
from sklearn import linear_model
reg = linear_model.LassoLars(alpha=.1)
reg.fit([[0, 0], [1, 1]], [0, 1])  

reg.coef_
```




    array([ 0.71715734,  0.        ])



### 1.1.9 贝叶斯回归
贝叶斯回归技术可以用来在估计过程中包括正则化参数：正则化参数不是硬性的，而是根据当前的数据进行调整。
这可以通过在模型的超参数中引入无信息的先验信息来实现。L2正则化用于在已知w一个高斯先验的情况下，找到w的一个最大后验估计，其系数不再手动设置，而是取λ^-1
为了得到一个概率模型，输出y认为是Xw附近的一个高斯分布：


<img src="http://scikit-learn.org/stable/_images/math/f478d5b226c02754ede31ce247d4d9870ff7c416.png" style="border:none;width: 200.0px;"/>

alpha是一个需要从数据中进行估计的随机变量。

其中我们有贝叶斯岭回归：
w的先验估计如下：


<img src="http://scikit-learn.org/stable/_images/math/ebf24e7f03ce757cbfab29367628715eb161fc38.png" style="border:none;width: 200.0px;"/>

α和λ都认为服从gamma分布。

### 1.1.10 Logistic回归
Logistic回归，更多地用于分类，而不是回归。Logistic回归在文献中也被称为对数回归，最大熵分类（MaxEnt）或对数线性分类器。在这里，单词实验的可能概率结果通过一个Logistic函数来描述。

在sklean中，Logistic回归的实现在<a class="reference internal" href="http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression" >LogisticRegression</a>类中，其实现，可以用于二进制，一对多或者多项Logistic回归，L1和L2都是可选。

例如，对于一个二分类问题，考虑L2正则化，Logistic回归的目标函数如下：


<img src="http://scikit-learn.org/stable/_images/math/760c999ccbc78b72d2a91186ba55ce37f0d2cf37.png" style="border:none;width: 450.0px;"/>

考虑L1正则化，则如下：


<img src="http://scikit-learn.org/stable/_images/math/6a0bcf21baaeb0c2b879ab74fe333c0aab0d6ae6.png" style="border:none;width: 450.0px;"/>

LogisticRegression中的求解器有多种，可以自由选择。


|Case|	Solver|
|--|--|
|L1 penalty	|“liblinear” or “saga”|
|Multinomial loss|	“lbfgs”, “sag”, “saga” or “newton-cg”|
|Very Large dataset (n_samples)|	“sag” or “saga”|


“saga”通常是最佳选择。

### 1.1.11 多项式回归

多项式回归类似线性回归，通过多项式来拟合样本，例如如下的多项式函数：


<img src="http://scikit-learn.org/stable/_images/math/1e1f74179df321954b823943c08d555a524e69f9.png" style="border:none;width: 500.0px;"/>


如果考虑：


<img src="http://scikit-learn.org/stable/_images/math/2c0a9947255ef61995c3f5e6319fc7fdfa3503c6.png" style="border:none;width: 200.0px;"/>


实际上，它还是一个线性模型：


<img src="http://scikit-learn.org/stable/_images/math/618623438e33ecf053d364a0b938f056cd758b34.png" style="border:none;width: 470.0px;"/>
