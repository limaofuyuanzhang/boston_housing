# 知识点总结

## numpy相关统计方法

```python
# TODO: Minimum price of the data
#目标：计算价值的最小值
minimum_price = np.min(prices)

# TODO: Maximum price of the data
#目标：计算价值的最大值
maximum_price = np.max(prices)

# TODO: Mean price of the data
#目标：计算价值的平均值
mean_price = np.mean(prices)


# TODO: Median price of the data
#目标：计算价值的中值
median_price = np.median(prices)

# TODO: Standard deviation of prices of the data
#目标：计算价值的标准差
std_price = np.std(prices)
```

这里要注意有一个小点值得注意，我们查看numpy的接口文档会发现只能找到amin,amax方法，找不到min和max方法，实际上min是还是调用amin，当只有一个输入数组时可以直接使用min，max和amax同理，在numpy的`__ini__.py`文件中可以看到下面这句代码
```
from .fromnumeric import amax as max, amin as min, round_ as round
```

具体区别可参考[numpy max vs amax vs maximum](http://stackoverflow.com/questions/33569668/numpy-max-vs-amax-vs-maximum)

**有用的资源**
[Numpy Quickstart tutorial](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html)
[Numpy 接口文档](https://docs.scipy.org/doc/numpy/reference/routines.html)

## 决定系数

决定系数R<sup>2</sup>=SSR/SST，SSR为回归平方和，即预测量与平均值之差的平方和，SST为总平方各，即实际值与平均值之差的平方和。

R<sup>2</sup>的数值范围从0至1，表示**目标变量**的预测值和实际值之间的相关程度平方的百分比。一个模型的R<sup>2</sup> 值为0还不如直接用**平均值**来预测效果好；而一个R<sup>2</sup> 值为1的模型则可以对目标变量进行完美的预测。从0至1之间的数值，则表示该模型中目标变量中有百分之多少能够用**特征**来解释。模型也可能出现负值的R<sup>2</sup>，这种情况下模型所做预测有时会比直接计算目标变量的平均值差很多。

计算R2方法
```
score = r2_score(y_true,y_predict)
```

## 偏差和方差

在模型预测中，模型可能出现的误差来自两个主要来源，即：因模型无法表示基本数据的复杂度而造成的偏差（bias），或者因模型对训练它所用的有限数据过度敏感而造成的方差（variance）

偏差表示预测值与真实值的误差，偏差越大，表示数据预测越不准确
方差表示预测值与期望值的误差，方差越大，表示数据分布越分散

偏差、方差和模型的复杂度之间的关系如下:

![](/14907888423241.jpg)

**有用的资源**
[Understanding the Bias-Variance Tradeoff](http://scott.fortmann-roe.com/docs/BiasVariance.html)
[偏差和方差有什么区别？](https://www.zhihu.com/question/20448464)

## 数据分割与重排

数据分割代码
```python
X_train, X_test, y_train, y_test = train_test_split(features, prices, test_size=0.2, random_state=42)
```

**将数据集按一定比例分为训练用的数据集和测试用的数据集对学习算法有什么好处？**
1.测试集可以用来评估模型的性能，并且这些数据都是真实，评估出来的结果是比较可靠的。如果没有数据进行测试，会使得我们无法无法判断模型的效果。
2.可以用来检查模型是否过度拟合，如果该模型训练评分很高，而测试评分很低，则该模型可能是过度拟合了。

**有用的资源**
[Why Data Scientists Split Data into Train and Test](http://info.salford-systems.com/blog/bid/337783/Why-Data-Scientists-Split-Data-into-Train-and-Test)


## 网格搜索和交叉验证

**什么是网格搜索法？如何用它来优化学习算法？**
在一些算法中，涉及到一些参数的选择，例如算法A,需要x,y两个参数，我们先给出x,y的参数列表，通过遍历参数列表中可能(x,y)组合，每次遍历我们都进行一次训练，并且使用我们预先设计好的评分方法对得到的模型进行评分，当遍历完所有组合后我们就可以得到评分最高的模型。

通过使用网格搜索法来得到学习算法的最优参数组合，从而得到最优的学习算法。

**什么是K折交叉验证法（k-fold cross-validation）？优化模型时，使用这种方法对网格搜索有什么好处？网格搜索是如何结合交叉验证来完成对最佳参数组合的选择的？**
K折交叉验证法就将数据平均分成K份，然后取第i(i的值为[1,k])份，作为测试集，剩余的除第i份以外的数据作为训练集训练模型，再使用第i份数据对模型进行评分，这样的过程进行K次后，计算这K次评分的平均值作为该模型的最终评分。

优化模型时，使用这种方法可以最大程度地利用已有的数据来反映数据的特征。

在使用网格搜索来寻找最佳参数组合过程中，使用交叉验证来计算每个参数组合下训练模型的评分，并且得到该最佳参数组合的模型（GridSearchCV中可以通过cv_results中的best_estimator_值直接得到最佳组合的模型，并直接使用该模型进行预测）。

**有用的资源**
[网格搜索算法与K折交叉验证](https://zhuanlan.zhihu.com/p/25637642)

# 小结

P1其实知识点并不多，只是内容比较零散，看得时候会有种不是很系统的感觉。

学习过程中发现自己的一个问题，容易死抠一个点不放，这种情况下可以考虑多看几个不同的材料来帮助自己理解。

