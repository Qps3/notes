# 第一关

- [任务描述](https://www.educoder.net/tasks/p8fb3jnofs69#任务描述)
- 相关知识
  - [为什么要进行标准化](https://www.educoder.net/tasks/p8fb3jnofs69#为什么要进行标准化)
  - [Z-score标准化](https://www.educoder.net/tasks/p8fb3jnofs69#z-score标准化)
  - [Min-max标准化](https://www.educoder.net/tasks/p8fb3jnofs69#min-max标准化)
  - [MaxAbs标准化](https://www.educoder.net/tasks/p8fb3jnofs69#maxabs标准化)
- [编程要求](https://www.educoder.net/tasks/p8fb3jnofs69#编程要求)
- [测试说明](https://www.educoder.net/tasks/p8fb3jnofs69#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`对数据进行标准化。

#### 相关知识

为了完成本关任务，你需要掌握：1.为什么要进行标准化，2.`Z-score`标准化，3.`Min-max`标准化，4.`MaxAbs`标准化。

##### 为什么要进行标准化

对于大多数数据挖掘算法来说，数据集的标准化是基本要求。这是因为，如果特征不服从或者近似服从标准正态分布（即，零均值、单位标准差的正态分布）的话，算法的表现会大打折扣。实际上，我们经常忽略数据的分布形状，而仅仅做零均值、单位标准差的处理。在一个机器学习算法的目标函数里的很多元素所有特征都近似零均值，方差具有相同的阶。如果某个特征的方差的数量级大于其它的特征，那么，这个特征可能在目标函数中占主导地位，这使得模型不能从其它特征有效地学习。

##### Z-score标准化

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210092252862.png)

这种方法基于原始数据的均值`mean`和标准差`standard deviation`进行数据的标准化。

将特征`A`的原始值`x`使用`z-score`标准化到`x’`。

`z-score`标准化方法适用于特征`A`的最大值和最小值未知的情况，或有超出取值范围的离群数据的情况。

将数据按其特征(按列进行)减去其均值，然后除以其方差。最后得到的结果是，对每个特征/每列来说所有数据都聚集在`0`附近，方差值为`1`。

函数`scale`为数组形状的数据集的标准化提供了一个快捷实现：

<img src="https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210092258533.png" alt="image-20221009225818488" style="zoom:150%;" />

经过缩放后的数据具有零均值以及标准方差:

```
>>> X_scaled.mean(axis=0)array([ 0.,  0.,  0.])>>> X_scaled.std(axis=0)array([ 1.,  1.,  1.])
```

##### Min-max标准化

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210092251292.png)

`Min-max`标准化方法是对原始数据进行线性变换。设`minA`和`maxA`分别为特征`A`的最小值和最大值，将`A`的一个原始值`x`通过`min-max`标准化映射成在区间`[0,1]`中的值`x'`，其公式为：

*x*,=*x**m**a**x*−*x**m**i**n**x*−*x**m**i**n*

可以使用`MinMaxScaler`实现，以下是一个将简单的数据矩阵缩放到`[0, 1]`的例子:

```
X_train = np.array([[ 1., -1.,  2.],                    [ 2.,  0.,  0.],                    [ 0.,  1., -1.]])min_max_scaler = preprocessing.MinMaxScaler()X_train_minmax = min_max_scaler.fit_transform(X_train)>>> X_train_minmaxarray([[ 0.5       ,  0.        ,  1.        ],      [ 1.        ,  0.5       ,  0.33333333],      [ 0.        ,  1.        ,  0.        ]])
```

##### MaxAbs标准化

`MaxAbs`的工作原理与`Min-max`非常相似，但是它只通过除以每个特征的最大值将训练数据特征缩放至 `[-1, 1]` 范围内，这就意味着，训练数据应该是已经零中心化或者是稀疏数据。公式如下：

*x*,=*x**m**a**x**x*

可以使用`MaxAbsScale`实现，以下是使用上例中数据运用这个缩放器的例子:

```
X_train = np.array([[ 1., -1.,  2.],                    [ 2.,  0.,  0.],                    [ 0.,  1., -1.]])max_abs_scaler = preprocessing.MaxAbsScaler()X_train_maxabs = max_abs_scaler.fit_transform(X_train)>>> X_train_maxabsarray([[ 0.5, -1. ,  1. ],       [ 1. ,  0. ,  0. ],       [ 0. ,  1. , -0.5]])
```

#### 编程要求

在右侧编辑器`Begin-End`处补充`Python`代码，实现数据标准化方法，我们会使用实现好的方法对数据进行处理。

#### 测试说明

我们会调用你实现的方法对数据进行处理，如使用`Z-score方法`，则处理后数据**均值**为`0`**方差**为`1`，如使用`minmax`方法，则处理后数据**最小值**为`0`，**最大值**为`1`，如使用`maxabs`方法，则处理后数据**最大值**为`1`。我们会对结果进行检测，完全正确则视为通关。

------

开始你的任务吧，祝你成功！ 

[机器学习——特征工程——数据的标准化（Z-Score,Maxmin,MaxAbs,RobustScaler,Normalizer](https://blog.csdn.net/huangguohui_123/article/details/105813207)

[python Numpy中array详解](https://blog.csdn.net/github_36669230/article/details/78038756#:~:text=%E5%8F%91%E5%B8%83-,python%20Numpy%E4%B8%ADarray%E8%AF%A6%E8%A7%A3,-Lesley_%E9%A9%B0%E9%AA%8B)

[Numpy学习之——numpy.mean中axis参数用法](https://blog.csdn.net/c20081052/article/details/80208473)

[fit_transform,fit,transform区别和作用详解](https://blog.csdn.net/weixin_38278334/article/details/82971752)



# 第二关

- [任务描述](https://www.educoder.net/tasks/r2tfunzwyqxg#任务描述)
- 相关知识
  - [为什么要非线性转换](https://www.educoder.net/tasks/r2tfunzwyqxg#为什么要非线性转换)
  - [映射到均匀分布](https://www.educoder.net/tasks/r2tfunzwyqxg#映射到均匀分布)
  - [映射到高斯分布](https://www.educoder.net/tasks/r2tfunzwyqxg#映射到高斯分布)
- [编程要求](https://www.educoder.net/tasks/r2tfunzwyqxg#编程要求)
- [测试说明](https://www.educoder.net/tasks/r2tfunzwyqxg#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`对数据进行非线性转换。

#### 相关知识

为了完成本关任务，你需要掌握：1.为什么要非线性转换，2.映射到均匀分布，3.映射到高斯分布。

##### 为什么要非线性转换

在上一关中已经提到，对于大多数数据挖掘算法来说，如果特征不服从或者近似服从标准正态分布（即，零均值、单位标准差的正态分布）的话，算法的表现会大打折扣。非线性转换就是将我们的特征映射到均匀分布或者高斯分布(即正态分布)。

##### 映射到均匀分布

相比线性缩放，该方法不受异常值影响，它将数据映射到了零到一的均匀分布上，将最大的数映射为`1`，最小的数映射为`0`。其它的数按从小到大的顺序均匀分布在`0`到`1`之间，如有相同的数则取平均值，如数据为`np.array([[1],[2],[3],[4],[5]])`则经过转换为：`np.array([[0],[0.25],[0.5],[0.75],[1]])`，数据为`np.array([[1],[2],[9],[10],[2]])`则经过转换为：`np.array([[0],[0.375],[0.75],[1.0],[0.375]])`。第二个例子具体过程如下图：

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210092325222.png)

在`sklearn`中使用`QuantileTransformer`方法实现，用法如下：

```
from sklearn.preprocessing import QuantileTransformerimport numpy as npdata = np.array([[1],[2],[3],[4],[5]])quantile_transformer = QuantileTransformer(random_state=666)data = quantile_transformer.fit_transform(data)>>>dataarray([[0.  ],       [0.25],       [0.5 ],       [0.75],       [1.  ]])
```

##### 映射到高斯分布

映射到高斯分布是为了稳定方差，并最小化偏差。在最新版`sklearn 0.20.x`中`PowerTransformer`现在有两种映射方法，`Yeo-Johnson`映射，公式如下：

*x**i*(*λ*)=⎩⎪⎪⎪⎪⎪⎨⎪⎪⎪⎪⎪⎧[(*x**i*+1)*λ*−1],*i**f**λ*=0,*x**i*≥0ln(*x**i*)+1, if *λ*=0,*x**i*≥0−[(−*x**i*+1)2−*λ*−1]/(2−*λ*), if *λ*=2,*x**i*<0−ln(−*x**i*+1), if *λ*=2,*x**i*<0

`Box-Cox`映射，公式如下：

*x**i*(*λ*)={*λ**x**i**λ*−1,*i**f**λ*=0ln(*x**i*),*i**f**λ*=0

在`sklearn 0.20.x`中使用`PowerTransformer`方法实现，用法如下：

```
from sklearn.preprocessing import PowerTransformerimport numpy as npdata = np.array([[1],[2],[3],[4],[5]])pt = PowerTransformer(method='box-cox', standardize=False)data = pt.fit_transform(data)
```

学习平台使用的是`sklearn 0.19.x`，通过对`QuantileTransformer`设置参数`output_distribution='normal'`实现映射高斯分布，用法如下：

```
from sklearn.preprocessing import QuantileTransformerimport numpy as npdata = np.array([[1],[2],[3],[4],[5]])quantile_transformer = QuantileTransformer(output_distribution='normal',random_state=666)data = quantile_transformer.fit_transform(data)data = np.around(data,decimals=3)>>>dataarray([[-5.199],       [-0.674],       [ 0.   ],       [ 0.674],       [ 5.199]])
```

#### 编程要求

根据提示，在右侧编辑器`Begin-End`处补充`Python`代码，实现数据非线性转换方法，我们会使用实现好的方法对数据进行处理。

#### 测试说明

我们会调用你实现好的方法对数据进行处理，如输入数据为：

```
np.array([[1],[2],[3],[4],[5]])
```

映射到均匀分布,则处理后结果为:

```
np.array([[0.  ], [0.25],[0.5 ],[0.75],[1.  ]])
```

映射到高斯分布，则处理后结果为:

```
np.array([[-5.199],[-0.674],[ 0.   ],[ 0.674],[ 5.199]])
```

如处理后的数据符合要求，则视为通关。

------

开始你的任务吧，祝你成功！

# 第3关：归一化

- [任务描述](https://www.educoder.net/tasks/ku9inl4w83fp#任务描述)
- 相关知识
  - [为什么使用归一化](https://www.educoder.net/tasks/ku9inl4w83fp#为什么使用归一化)
  - [L1范式归一化](https://www.educoder.net/tasks/ku9inl4w83fp#l1范式归一化)
  - [L2范式归一化](https://www.educoder.net/tasks/ku9inl4w83fp#l2范式归一化)
- [编程要求](https://www.educoder.net/tasks/ku9inl4w83fp#编程要求)
- [测试说明](https://www.educoder.net/tasks/ku9inl4w83fp#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`对数据进行归一化。

#### 相关知识

为了完成本关任务，你需要掌握：1.为什么使用归一化，2.`L1`范式归一化，3.`L2`范式归一化。

##### 为什么使用归一化

归一化是缩放**单个样本**以具有**单位范数**的过程。归一化实质是一种线性变换，线性变换有很多良好的性质，这些性质决定了对数据改变后不会造成“失效”，反而能提高数据的表现，这些性质是归一化的前提。归一化能够**加快模型训练速度**，**统一特征量纲**，**避免数值太大**。值得注意的是，归一化是对每一个样本做转换，所以是**对数据的每一行进行变换**。而之前我们讲过的方法是对数据的每一列做变换。

##### L1范式归一化

`L1`范式定义如下：

∣∣*x*∣∣1=*s**u**m**i*=1*n*∣*x**i*∣

表示向量`x`中每个元素的绝对值之和。 `L1`范式归一化就是将样本中每个特征**除以**特征的`L1`范式。

在`sklearn`中使用`normalize`方法实现，用法如下：

```
from sklearn.preprocessing import normalizedata = np.array([[-1,0,1],                 [1,0,1],                 [1,2,3]])data = normalize(data,'l1')>>>dataarray([[-0.5  ,  0.   ,  0.5  ],       [ 0.5  ,  0.   ,  0.5  ],       [ 0.167,  0.333,  0.5  ]])
```

##### L2范式归一化

`L2`范式定义如下：

∣∣*x*∣∣2=*s**q**r**t**s**u**m**i*=1*n**x**i*2

表示向量元素的平方和再开平方根。 `L2`范式归一化就是将样本中每个特征**除以**特征的`L2`范式。

在`sklearn`中使用`normalize`方法实现，用法如下：

```
from sklearn.preprocessing import normalizedata = np.array([[-1,0,1],                 [1,0,1],                 [1,2,3]])data = normalize(data,'l2')>>>dataarray([[-0.707,  0.   ,  0.707],       [ 0.707,  0.   ,  0.707],       [ 0.267,  0.535,  0.802]])
```

#### 编程要求

根据提示，在右侧编辑器`Begin-End`处补充`Python`代码，实现数据归一化方法，我们会使用实现好的方法对数据进行处理。

#### 测试说明

我们会调用你实现的方法对数据进行处理，如数据为：

```
data = np.array([[-1,0,1],                 [1,0,1],                 [1,2,3]])
```

使用`L1`归一化则输出为：

```
array([[-0.5  ,  0.   ,  0.5  ],       [ 0.5  ,  0.   ,  0.5  ],       [ 0.167,  0.333,  0.5  ]])
```

使用`L2`归一化则输出为：

```
array([[-0.707,  0.   ,  0.707],       [ 0.707,  0.   ,  0.707],       [ 0.267,  0.535,  0.802]])
```

数据处理正确则视为通关。

------

开始你的任务吧，祝你成功！



# 第4关：离散值编码

- [任务描述](https://www.educoder.net/tasks/ste4x2u5pan7#任务描述)
- 相关知识
  - [LabelEncoder](https://www.educoder.net/tasks/ste4x2u5pan7#labelencoder)
  - [OneHotEncoder](https://www.educoder.net/tasks/ste4x2u5pan7#onehotencoder)
- [编程要求](https://www.educoder.net/tasks/ste4x2u5pan7#编程要求)
- [测试说明](https://www.educoder.net/tasks/ste4x2u5pan7#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`对标签进行`OneHot`编码。

#### 相关知识

为了完成本关任务，你需要掌握：1. `LabelEncoder`；2. `OneHotEncoder`。

##### LabelEncoder

在数据挖掘中，特征经常不是数值型的而是分类型的。举个例子，一个人可能有`["male", "female"]`，`["from Europe", "from US", "from Asia"]`，`["uses Firefox", "uses Chrome", "uses Safari", "uses Internet Explorer"]`等分类的特征。这些特征能够被有效地编码成整数，比如`["male", "from US", "uses Internet Explorer"]`可以被表示为`[0, 1, 3]`，`["female", "from Asia", "uses Chrome"]`表示为`[1, 2, 1]`。

在`sklearn`中，通过`LabelEncoder`来实现：

```
from sklearn.preprocessing import LabelEncoderlabel = ['male','female']int_label = LabelEncoder()label = int_label.fit_transform(label)>>>labelarray([1, 0])
```

##### OneHotEncoder

这种整数特征表示并不能在`sklearn`的估计器中直接使用，因为这样的连续输入，估计器会认为类别之间是有序的，但实际却是无序的。如将`male,female`，转换为`1,0`。`1`比`0`要大，机器就会把这个关系考虑进去，而`male,female`之间是没有这样的关系的。所以我们需要使用另外一种编码方式，`OneHot`编码。

在`sklearn`中通过`OneHotEncoder`来实现，使用方法如下：

```
import numpy as npfrom sklearn.preprocessing import OneHotEncoderlabel = np.array([1,0])label = np.array(label).reshape(len(label),1)#先将X组织成（sample，feature）的格式onehot_label = OneHotEncoder()label = onehot_label.fit_transform(label).toarray()>>>labelarray([[0., 1.],       [1., 0.]])
```

#### 编程要求

根据提示，在右侧编辑器`Begin-End`处补充代码，实现`OneHot`编码方法。

#### 测试说明

我们会调用你实现的方法对标签进行处理，如标签为：

```
label = ['male','female']
```

则经过`OneHot`编码后的数据为：

```
array([[0., 1.],       [1., 0.]])
```

------

开始你的任务吧，祝你成功！



# 第5关：生成多项式特征

- [任务描述](https://www.educoder.net/tasks/vtzs3wpx67u2#任务描述)
- 相关知识
  - [为什么需要多项式特征](https://www.educoder.net/tasks/vtzs3wpx67u2#为什么需要多项式特征)
  - [PolynomialFeatures](https://www.educoder.net/tasks/vtzs3wpx67u2#polynomialfeatures)
- [编程要求](https://www.educoder.net/tasks/vtzs3wpx67u2#编程要求)
- [测试说明](https://www.educoder.net/tasks/vtzs3wpx67u2#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`生成多项式特征。

#### 相关知识

为了完成本关任务，你需要掌握：1.为什么需要多项式特征；2.`PolynomialFeatures`。

##### 为什么需要多项式特征

在数据挖掘中，获取数据的代价经常是非常高昂的。所以有时就需要人为的制造一些特征，并且有的特征之间是有关联的。生成多项式特征可以轻松的为我们获取更多的数据，并获得特征的更高维度和互相间关系的项且引入了特征之间的非线性关系，可以有效的增加模型的复杂度。

##### PolynomialFeatures

在`sklearn`中通过`PolynomialFeatures`方法来生成多项式特征，使用方法如下：

```
import numpy as npfrom sklearn.preprocessing import PolynomialFeaturesdata = np.arange(6).reshape(3, 2)poly = PolynomialFeatures(2)#生成二项式特征data = poly.fit_transform(data)>>>dataarray([[ 1.,  0.,  1.,  0.,  0.,  1.],       [ 1.,  2.,  3.,  4.,  6.,  9.],       [ 1.,  4.,  5., 16., 20., 25.]])
```

特征转换情况如下：

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210131306239.png)

在一些情况下，只需要特征间的交互项，这可以通过设置 `interaction_only=True`来得到:

```
import numpy as npfrom sklearn.preprocessing import PolynomialFeaturesdata = np.arange(6).reshape(3, 2)poly = PolynomialFeatures(degree=2, interaction_only=True)#degree=n表示生成n项式特征，只需要特征之间交互data = poly.fit_transform(data)>>>dataarray([[ 1.,  0.,  1.,  0.],       [ 1.,  2.,  3.,  6.],       [ 1.,  4.,  5., 20.]])
```

特征转换情况如下：

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E6%80%BB%E7%B1%BB/java/202210131306248.png)

#### 编程要求

根据提示，在右侧编辑器`Begin-End`处补充代码，实现生成多项式特征方法。

#### 测试说明

我们会调用你实现的方法，将数据生成多项式特征，如数据为：

```
data = np.arange(6).reshape(3, 2)
```

则生成二项式特征为：

```
array([[ 1.,  0.,  1.,  0.,  0.,  1.],       [ 1.,  2.,  3.,  4.,  6.,  9.],       [ 1.,  4.,  5., 16., 20., 25.]])
```

生成二项式只交互特征为：

```
array([[ 1.,  0.,  1.,  0.],       [ 1.,  2.,  3.,  6.],       [ 1.,  4.,  5., 20.]])
```

处理正确则视为通关。

------

开始你的任务吧，祝你成功！



# 第6关：估算缺失值

- [任务描述](https://www.educoder.net/tasks/rc569weikfsg#任务描述)
- 相关知识
  - [为什么要估算缺失值](https://www.educoder.net/tasks/rc569weikfsg#为什么要估算缺失值)
  - [Imputer](https://www.educoder.net/tasks/rc569weikfsg#imputer)
- [编程要求](https://www.educoder.net/tasks/rc569weikfsg#编程要求)
- [测试说明](https://www.educoder.net/tasks/rc569weikfsg#测试说明)

------

#### 任务描述

本关任务：利用`sklearn`对数据估算缺失值。

#### 相关知识

为了完成本关任务，你需要掌握：1.为什么要估算缺失值，2.`Imputer`。

##### 为什么要估算缺失值

由于各种原因，真实世界中的许多数据集都包含缺失数据，这类数据经常被编码成空格、`NaNs`，或者是其他的占位符。但是这样的数据集并不能被`sklearn`学习算法兼容，因为大多的学习算法都默认假设数组中的元素都是数值，因而所有的元素都有自己的意义。 使用不完整的数据集的一个基本策略就是舍弃掉整行或整列包含缺失值的数据。但是这样就付出了舍弃可能有价值数据（即使是不完整的 ）的代价。 处理缺失数值的一个更好的策略就是从已有的数据推断出缺失的数值。

##### Imputer

`sklearn`中使用`Imputer`方法估算缺失值，使用方法如下：

```
from sklearn.preprocessing import Imputerdata = [[np.nan, 2], [6, np.nan], [7, 4],[np.nan,4]]imp = Imputer(missing_values='NaN', strategy='mean', axis=0)#缺失值为nan，沿着每一列，使用平均值来代替缺失值data = imp.fit_transform(data)>>>dataarray([[6.5       , 2.        ],       [6.        , 3.33333333],       [7.        , 4.        ],       [6.5       , 4.        ]])
```

其中`strategy`参数用来选择代替缺失值方法：

```
`mean`表示使用平均值代替缺失值`median`表示使用中位数代替缺失值`most_frequent`表示使用出现频率最多的值代替缺失值
```

`missing_values`参数表示何为缺失值：

```
`NaN`表示`np.nan`为缺失值`0`表示`0`为缺失值
```

#### 编程要求

根据提示，在右侧编辑器`Begin-End`处补充代码，实现估算缺失值方法。

#### 测试说明

我们会调用你实现的估算缺失值方法对数据进行处理，如输入数据为：

```
data = [[np.nan, 2], [6, np.nan], [7, 4],[np.nan,4]]
```

则用取平均值方法处理后数据为：

```
array([[6.5       , 2.        ],       [6.        , 3.33333333],       [7.        , 4.        ],       [6.5       , 4.        ]])
```

用取中位数方法处理后数据为：

```
array([[6.5, 2. ],       [6. , 4. ],       [7. , 4. ],       [6.5, 4. ]])
```

用出现频率最多的值代替缺失值方法处理后数据为：

```
array([[6., 2.],       [6., 4.],       [7., 4.],       [6., 4.]])
```

数据处理正确则视为通关。

------

开始你的任务吧，祝你成功！
