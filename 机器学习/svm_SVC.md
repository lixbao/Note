# SVM算法

> 概要：SVM算法（support vector machine，支持向量机），是一种二分类算法。
>
> + 支持向量：支持或支撑平面上把两类类别划分开的超平面的向量点
> + 机：一个算法
>
> SVM模型的种类
>
> - **svm.LinearSVC** Linear Support Vector Classification.
> - **svm.LinearSVR** Linear Support Vector Regression.
> - **svm.NuSVC** Nu-Support Vector Classification.
> - **svm.NuSVR** Nu Support Vector Regression.
> - **svm.OneClassSVM** Unsupervised Outlier Detection.
> - **svm.SVC** C-Support Vector Classification.
> - **svm.SVR** Epsilon-Support Vector Regression.
>
> 可以很简单的解释为三者的关系
>
> - SVM=Support Vector Machine 是支持向量
> - SVC=Support Vector Classification就是支持向量机用于分类，
> - SVR=Support Vector Regression.就是支持向量机用于回归分析
>
> 语言：python
>
> 领域：机器学习



## **支持向量机之SVC算法（svm.SVC）** 

> svm.SVC：C-Support Vector Classification即svm用于分类

```python
sklearn.svm.SVC(
	C=1.0, 
	kernel='rbf', 
	degree=3, 
	gamma='auto', 
	coef0=0.0, 
	shrinking=True, 
	probability=False, 
	tol=0.001, 
	cache_size=200, 
	class_weight=None, 
	verbose=False, 
	max_iter=-1, 
	decision_function_shape=None,
	random_state=None
	)
```

### 参数：

关键参数：

- **C**：float类型，默认值是1.0
  参数表明算法要训练数据点需要作出多少调整适应，C小，SVM对数据点调整少，多半取平均，仅用少量的数据点和可用变量，C值增大，会使学习过程遵循大部分的可用训练数据点并牵涉较多变量。C值过大，有过拟合问题，C值过小，预测粗糙，不准确。
  
- **kernel** ：str类型，默认是rbf，核函数，可选参数有
  
  - linear：线性核函数
  
    ​							$$k(x,x_i)=x \cdot xi$$
  
  - poly：多项式核函数
  
    ​							$$k(x,x_i)=(x \cdot xi+1)^d$$
  
  - rbf：径像核函数/高斯核
  
    ​							$$k(x,x_i)=exp({-||x-xi||^2 \over \delta^2})$$
  
  - sigmoid：sigmoid核函数（实现多层神经网络）
  
    ​							$$k(x,x_i)=tanh(\eta <x,x_i>+\theta)$$
  
  - precomputed：核矩阵表示自己提前计算好核函数矩阵，这时候算法内部就不再用核函数去计算核矩阵，而是直接用你给的核矩阵。
  
- **degree** ：int 类型，默认为3
  多项式poly函数的阶数n，选择其他核函数时会被忽略。
  
- **gamma** ：float 类型，默认为auto代表其值为样本特征数的倒数，即1/n_features
  核函数系数，只对rbf，poly，sigmod有效。
  
- **coef0** ：float 类型，默认为0.0
  核函数的常数项，对于poly和 sigmoid代表其中的参数c



其他参数：

- **probability** ：bool类型，默认为False
  是否采用概率估计，需要在训练fit()模型时加上这个参数，之后才能用相关的方法：predict_proba和predict_log_proba
- **shrinking** ：bool类型，默认为True
  是否采用shrinking heuristic()启发式收缩方式
- **tol** ：float类型，默认为1e-3
  svm停止训练的误差值大小
- **cache_size** ：float类型，默认为200(MB)
  指定训练所需要的内存，以MB为单位
- **class_weight** ：字典类型或’balance’字符串，默认为None
  类别的权重，字典形式传递。设置第几类的参数C为weight*C(C-SVC中的C)
- **verbose** ：bool类型，默认为False
  是否启用详细输出。 此设置利用libsvm中的每个进程运行时设置，如果启用，可能无法在多线程上下文中正常工作。一般情况都设为False，不用管它。
- **max_iter** ：int类型， 默认为-1
  最大迭代次数。-1为无限制。
- **decision_function_shape** ：str类型，默认为’ovr’
  ovo’, ‘ovr’, default=’ovr’，是否将形状（n_samples，n_classes）的one-rest-rest（‘ovr’）决策函数作为所有其他分类器返回，或者返回具有形状的libsvm的原始one-vs-one（‘ovo’）决策函数（n_samples） ，n_classes *（n_classes - 1）/ 2）。
  通俗解释：ovr即为one v rest 一个类别分别与其他类别进行划分，ovo为one v one，类别两两之间划分，用二分类的方法模拟多分类的结果
- **random_state** ：int类型，默认为None
  伪随机数发生器的种子,在混洗数据时用于概率估计。
  





### **SVM预测模型的通用步骤**

- 选择使用的SVM类
- 用数据训练模型
- 检查验证误差并作为基准线
- 为SVM参数尝试不同的值
- 检查验证误差是否改进
- 再次使用最优参数的数据来训练模型







示例代码





```python
start = time.time()
clf = svm.SVC(kernel='sigmoid')
scores = cross_val_score(clf, X, Y, cv=10, scoring='accuracy')
print '用时%f 秒！' % (time.time() - start)
print 'svm10重交叉验证评分：', scores
print 'svm10重交叉验证平均分：', scores.mean()
```
