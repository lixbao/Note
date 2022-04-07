# 交叉验证

> 一般机器学习对数据的处理就是分割数据集为训练集和测试集，用训练集去训练模型，用测试集去测试模型的性能或是否过拟合。但这样做是有一点不可靠，万一不好的模型对测试集过拟合，这样我们就错误认为模型是好的。要解决这个问题要怎么办呢？答案是使用交叉验证。



```python
sklearn.model_selection.cross_val_score(
    estimator, 
    X, 
    y=None, 
    groups=None, 
    scoring=None, 
    cv='warn', 
    n_jobs=None, 
    verbose=0, 
    fit_params=None, 
    pre_dispatch='2*n_jobs',
    error_score='raise-deprecating'
)
```



### 参数：

[官方解释（English）](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html#sklearn.model_selection.cross_val_score)

- **estimator**： 需要使用交叉验证的算法
- **X**： 输入样本数据
- **y**： 样本标签
- **groups**： 将数据集分割为训练/测试集时使用的样本的组标签
- **cv**： 交叉验证折数或可迭代的次数
  - None：使用默认的 5 折交叉验证
  - int：指定折数
  - 生成（训练，测试）拆分为索引数组的迭代
  - [CV splitter](https://scikit-learn.org/stable/glossary.html#term-CV-splitter)
- **n_jobs**： 同时工作的cpu个数（-1代表所有）
- **verbose**： int, default=0 详细程度
- **fit_params**： 传递给估计器（验证算法）的拟合方法的参数
- **pre_dispatch**： 控制并行执行期间调度的作业数量。减少这个数量对于避免在CPU发送更多作业时CPU内存消耗的扩大是有用的。
  - Null：在这种情况下，所有的工作立即创建并产生。将其用于轻量级和快速运行的作业，以避免由于按需产生作业而导致延迟
  - int：给出所产生的总工作的确切数量
  - str：给出一个表达式作为n_jobs的函数，如2 * n_jobs
- **error_score**： 如果在估计器拟合中发生错误，要分配给该分数的值

- **scoring**： **str or callable, default=None**

  | Scoring（得分）                | Function（函数）                                             | Comment（注解）                                              |
  | :----------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
  | **Classification（分类）**     |                                                              |                                                              |
  | ‘accuracy’                     | [`metrics.accuracy_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.accuracy_score.html#sklearn.metrics.accuracy_score) |                                                              |
  | ‘average_precision’            | [`metrics.average_precision_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.average_precision_score.html#sklearn.metrics.average_precision_score) |                                                              |
  | ‘f1’                           | [`metrics.f1_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.f1_score.html#sklearn.metrics.f1_score) | for binary targets（用于二进制目标）                         |
  | ‘f1_micro’                     | [`metrics.f1_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.f1_score.html#sklearn.metrics.f1_score) | micro-averaged（微平均）                                     |
  | ‘f1_macro’                     | [`metrics.f1_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.f1_score.html#sklearn.metrics.f1_score) | macro-averaged（微平均）                                     |
  | ‘f1_weighted’                  | [`metrics.f1_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.f1_score.html#sklearn.metrics.f1_score) | weighted average（加权平均）                                 |
  | ‘f1_samples’                   | [`metrics.f1_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.f1_score.html#sklearn.metrics.f1_score) | by multilabel sample（通过 multilabel 样本）                 |
  | ‘neg_log_loss’                 | [`metrics.log_loss`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.log_loss.html#sklearn.metrics.log_loss) | requires `predict_proba` support（需要 `predict_proba` 支持） |
  | ‘precision’ etc.               | [`metrics.precision_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.precision_score.html#sklearn.metrics.precision_score) | suffixes apply as with ‘f1’（后缀适用于 ‘f1’）               |
  | ‘recall’ etc.                  | [`metrics.recall_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.recall_score.html#sklearn.metrics.recall_score) | suffixes apply as with ‘f1’（后缀适用于 ‘f1’）               |
  | ‘roc_auc’                      | [`metrics.roc_auc_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.roc_auc_score.html#sklearn.metrics.roc_auc_score) |                                                              |
  | **Clustering（聚类）**         |                                                              |                                                              |
  | ‘adjusted_mutual_info_score’   | [`metrics.adjusted_mutual_info_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.adjusted_mutual_info_score.html#sklearn.metrics.adjusted_mutual_info_score) |                                                              |
  | ‘adjusted_rand_score’          | [`metrics.adjusted_rand_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html#sklearn.metrics.adjusted_rand_score) |                                                              |
  | ‘completeness_score’           | [`metrics.completeness_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.completeness_score.html#sklearn.metrics.completeness_score) |                                                              |
  | ‘fowlkes_mallows_score’        | [`metrics.fowlkes_mallows_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.fowlkes_mallows_score.html#sklearn.metrics.fowlkes_mallows_score) |                                                              |
  | ‘homogeneity_score’            | [`metrics.homogeneity_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.homogeneity_score.html#sklearn.metrics.homogeneity_score) |                                                              |
  | ‘mutual_info_score’            | [`metrics.mutual_info_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.mutual_info_score.html#sklearn.metrics.mutual_info_score) |                                                              |
  | ‘normalized_mutual_info_score’ | [`metrics.normalized_mutual_info_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.normalized_mutual_info_score.html#sklearn.metrics.normalized_mutual_info_score) |                                                              |
  | ‘v_measure_score’              | [`metrics.v_measure_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.v_measure_score.html#sklearn.metrics.v_measure_score) |                                                              |
  | **Regression（回归）**         |                                                              |                                                              |
  | ‘explained_variance’           | [`metrics.explained_variance_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.explained_variance_score.html#sklearn.metrics.explained_variance_score) |                                                              |
  | ‘neg_mean_absolute_error’      | [`metrics.mean_absolute_error`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.mean_absolute_error.html#sklearn.metrics.mean_absolute_error) |                                                              |
  | ‘neg_mean_squared_error’       | [`metrics.mean_squared_error`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.mean_squared_error.html#sklearn.metrics.mean_squared_error) |                                                              |
  | ‘neg_mean_squared_log_error’   | [`metrics.mean_squared_log_error`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html#sklearn.metrics.mean_squared_log_error) |                                                              |
  | ‘neg_median_absolute_error’    | [`metrics.median_absolute_error`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.median_absolute_error.html#sklearn.metrics.median_absolute_error) |                                                              |
  | ‘r2’                           | [`metrics.r2_score`](http://sklearn.apachecn.org/cn/stable/modules/generated/sklearn.metrics.r2_score.html#sklearn.metrics.r2_score) |                                                              |






### 交叉验证的思路：

1. 先使用train_test_split将数据集分为训练集和测试集。
2. 将训练集在分为n份（自己设置），用n-1份去训练模型，用1份去预测模型。
3. 得到n个结果去平均值
4. 再用测试集去预测最终结果





### 示例代码

```python
# -----------------朴素贝叶斯模型训练-----------
import numpy
X=numpy.array([
    [0, 0, 2, 0, 0, 3, 0, 0, 0, 0, 0, 1],
    [0, 0, 0, 1, 0, 0, 2, 0, 0, 1, 1, 1],
    [0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0],
    [0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1]
])
Y=numpy.array([1, 1, 1, 1, 0, 0])
from sklearn.naive_bayes import MultinomialNB
clf = MultinomialNB(alpha=1.0, class_prior=None, fit_prior=True)
clf.fit(X,Y)
param=X[0:6]
param=[[0, 0, 3, 3, 2, 2, 0, 0, 0, 0, 0, 1]]
numpy.set_printoptions(suppress=True,precision=4)
print clf.predict_proba(param),clf.predict(param)
# ------------------end-----------------
#output
>>>[[0.36 0.64]] [1]


import time
from sklearn.model_selection import cross_val_score
# -----------------朴素贝叶斯交叉验证-----------
start = time.time()
clf = MultinomialNB()
scores = cross_val_score(clf, X, Y, cv=3, scoring='accuracy')
print '用时%f 秒！' % (time.time() - start)
print '朴素贝叶斯n重交叉验证评分：', scores
print '朴素贝叶斯n重交叉验证平均分：', scores.mean()
# ------------------end-----------------
#output
>>>用时0.004000 秒！
>>>朴素贝叶斯3重交叉验证评分： [0.3333 0.5    0.    ]
>>>朴素贝叶斯3重交叉验证平均分： 0.27777777777777773
```

