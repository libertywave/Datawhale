### 一、线性回归

在统计学中，线性回归（英语：linear regression）是利用称为线性回归方程的最小二乘函数对一个或多个自变量和因变量之间关系进行建模的一种回归分析。这种函数是一个或多个称为回归系数的模型参数的线性组合。只有一个自变量的情况称为简单回归，大于一个自变量情况的叫做多元回归（multivariate linear regression）。

在线性回归中，数据使用线性预测函数来建模，并且未知的模型参数也是通过数据来估计。这些模型被叫做线性模型。最常用的线性回归建模是给定X值的y的条件均值是X的仿射函数。不太一般的情况，线性回归模型可以是一个中位数或一些其他的给定X的条件下y的条件分布的分位数作为X的线性函数表示。像所有形式的回归分析一样，线性回归也把焦点放在给定X值的y的条件概率分布，而不是X和y的联合概率分布（多元分析领域）。

线性回归是回归分析中第一种经过严格研究并在实际应用中广泛使用的类型。这是因为**线性依赖于其未知参数的模型比非线性依赖于其未知参数的模型更容易拟合，而且产生的估计的统计特性也更容易确定**。

线性回归有很多实际用途。分为以下两大类：

1. 如果目标是预测或者映射，线性回归可以用来对观测数据集的和X的值拟合出一个预测模型。当完成这样一个模型以后，对于一个新增的X值，在没有给定与它相配对的y的情况下，可以用这个拟合过的模型预测出一个y值。
2. 给定一个变量y和一些变量![X_1](https://wikimedia.org/api/rest_v1/media/math/render/svg/f70b2694445a5901b24338a2e7a7e58f02a72a32),...,![X_p](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f7e8d9eb9672d0455d39519ae4eb49adfa19331),这些变量有可能与y相关，线性回归分析可以用来量化y与![X_j](https://wikimedia.org/api/rest_v1/media/math/render/svg/ca3cb1ef7c9f25e85e1957e4eb58a72fa16a0066)之间相关性的强度，评估出与y不相关的![X_j](https://wikimedia.org/api/rest_v1/media/math/render/svg/ca3cb1ef7c9f25e85e1957e4eb58a72fa16a0066)，并识别出哪些![X_j](https://wikimedia.org/api/rest_v1/media/math/render/svg/ca3cb1ef7c9f25e85e1957e4eb58a72fa16a0066)的子集包含了关于y的冗余信息。

**理论模型**：

![ Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \ldots + \beta_p X_{ip} + \varepsilon_i, \qquad i = 1, \ldots, n ](https://wikimedia.org/api/rest_v1/media/math/render/svg/495382d7c370d1ba0c0fa3d2f9dff042eb45b45d)

**数据和估计：**

区分随机变量和这些变量的观测值是很重要的。通常来说，观测值或数据（以小写字母表记）包括了n个值 ![(y_{i},x_{{i1}},\ldots ,x_{{ip}}),\,i=1,\ldots ,n](https://wikimedia.org/api/rest_v1/media/math/render/svg/0a30b9a4734d68bad66bf929a7d3081c06218b38)

我们有![p+1](https://wikimedia.org/api/rest_v1/media/math/render/svg/5885ec01d3b5670fd5f88847f32da2b3dd62f60c)个参数 ![\beta _{0},\ldots ,\beta _{p}](https://wikimedia.org/api/rest_v1/media/math/render/svg/65dcd586c7f6b2a9accef1596ff7c7adc84b9da9)需要决定，为了估计这些参数，使用矩阵表记是很有用的。

![Y=X\beta +\varepsilon \,](https://wikimedia.org/api/rest_v1/media/math/render/svg/5c51e87ed1c90e6a1e7dddc75f4182a71f27fd8e)

其中*Y*是一个包括了观测值![Y_{1},\ldots ,Y_{n}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1df095625cb0644ba7ed0c6a0cb2812fa210bd61)的列向量，![\varepsilon ](https://wikimedia.org/api/rest_v1/media/math/render/svg/a30c89172e5b88edbd45d3e2772c7f5e562e5173)包括了未观测的随机成分![\varepsilon _{1},\ldots ,\varepsilon _{n}](https://wikimedia.org/api/rest_v1/media/math/render/svg/4214c1658ff50164552613a0004e19aef73eafab)以及回归量的观测值矩阵![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)：

![X={\begin{pmatrix}1&x_{{11}}&\cdots &x_{{1p}}\\1&x_{{21}}&\cdots &x_{{2p}}\\\vdots &\vdots &\ddots &\vdots \\1&x_{{n1}}&\cdots &x_{{np}}\end{pmatrix}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/4aa8207c358a1afbb78fd14e011c9cce13b33ee2)

*X*通常包括一个常数项。

如果*X*列之间存在线性相关，那么参数向量![\beta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/7ed48a5e36207156fb792fa79d29925d2f7901e8)就不能以最小二乘法估计除非![\beta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/7ed48a5e36207156fb792fa79d29925d2f7901e8)被限制，比如要求它的一些元素之和为0。

**古典假设：**

- **样本**是在**总体**之中**随机抽取**出来的。
- 因变量Y在实直线上是**连续的**，
- 残差项是**独立**且**相同**分布的，也就是说，残差是独立随机的，且服从**高斯分布**。

这些假设意味着**残差项不依赖自变量的值**，所以![\varepsilon _{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/00e1b6ad3cbad4af49bf21a3ad2dc379ff045079)和自变量X（预测变量）之间是相互独立的。

在这些假设下，建立一个显式线性回归作为条件预期模型的**简单线性回归**，可以表示为：

![{\mbox{E}}(Y_{i}\mid X_{i}=x_{i})=\alpha +\beta x_{i}\,](https://wikimedia.org/api/rest_v1/media/math/render/svg/eef97104d6238e82f4d3ff355cbb2a207a1f9cdb)

**最小二乘法**

回归分析的最初目的是估计模型的参数以便达到对数据的最佳拟合。在决定一个最佳拟合的不同标准之中，最小二乘法是非常优越的。这种估计可以表示为：

![{\hat  \beta }=(X^{T}X)^{{-1}}X^{T}y\,](https://wikimedia.org/api/rest_v1/media/math/render/svg/7f7aa3188855bd778f3131dcd8673a9cac60adec)

### 二、卡方分布

**卡方分布**（**chi-square distribution**], **χ²-distribution**，或写作**χ²分布**）是概率论与统计学中常用的一种概率分布。**k个独立的标准正态分布变量的平方和服从自由度为k的卡方分布**。卡方分布是一种特殊的伽玛分布，是统计推断中应用最为广泛的概率分布之一，例如假设检验和置信区间的计算。。

数学定义：

若*k*个随机变量![Z_{1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/cea9e950915c77b3dcf9d4d54101820f538bc077)，...，![Z_{k}](https://wikimedia.org/api/rest_v1/media/math/render/svg/29a05237076c50ce9cf9a75c02ff57abefac0de4)是相互独立，符合标准正态分布的随机变量（数学期望为0、方差为1），则随机变量Z的平方和

![X=\sum _{{i=1}}^{k}Z_{i}^{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5248fb0830a7e66ab3075b6a694d962b1468b5f7)

被称为服从自由度为 k 的卡方分布，记作

![X\ \sim \ \chi ^{2}(k)\,](https://wikimedia.org/api/rest_v1/media/math/render/svg/7674f8f53c0f1a36bf753fba2ae52fe8160d9f5c)

![\ X\ \sim \ \chi _{k}^{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1d984cc4b04e09948ab6334deb9d2f54050bc64b)

**卡方分布表**

p-value = 1- p_CDF.

χ2越大，p-value越小，则可信度越高。通常用p=0.05作为阈值，即95%的可信度

常用的χ2与p-value表如下:

| 自由度k \ P value （概率） | 0.95  | 0.90 | 0.80 | 0.70 | 0.50 | 0.30  | 0.20  | 0.10  | 0.05  | 0.01  | 0.001 |
| :------------------------: | ----- | ---- | ---- | ---- | ---- | ----- | ----- | ----- | ----- | ----- | ----- |
|             1              | 0.004 | 0.02 | 0.06 | 0.15 | 0.46 | 1.07  | 1.64  | 2.71  | 3.84  | 6.64  | 10.83 |
|             2              | 0.10  | 0.21 | 0.45 | 0.71 | 1.39 | 2.41  | 3.22  | 4.60  | 5.99  | 9.21  | 13.82 |
|             3              | 0.35  | 0.58 | 1.01 | 1.42 | 2.37 | 3.66  | 4.64  | 6.25  | 7.82  | 11.34 | 16.27 |
|             4              | 0.71  | 1.06 | 1.65 | 2.20 | 3.36 | 4.88  | 5.99  | 7.78  | 9.49  | 13.28 | 18.47 |
|             5              | 1.14  | 1.61 | 2.34 | 3.00 | 4.35 | 6.06  | 7.29  | 9.24  | 11.07 | 15.09 | 20.52 |
|             6              | 1.63  | 2.20 | 3.07 | 3.83 | 5.35 | 7.23  | 8.56  | 10.64 | 12.59 | 16.81 | 22.46 |
|             7              | 2.17  | 2.83 | 3.82 | 4.67 | 6.35 | 8.38  | 9.80  | 12.02 | 14.07 | 18.48 | 24.32 |
|             8              | 2.73  | 3.49 | 4.59 | 5.53 | 7.34 | 9.52  | 11.03 | 13.36 | 15.51 | 20.09 | 26.12 |
|             9              | 3.32  | 4.17 | 5.38 | 6.39 | 8.34 | 10.66 | 12.24 | 14.68 | 16.92 | 21.67 | 27.88 |
|             10             | 3.94  | 4.86 | 6.18 | 7.27 | 9.34 | 11.78 | 13.44 | 15.99 | 18.31 | 23.21 | 29.59 |

### 三、方差分析

随着增加个体显著性检验的次数，偶然因素导致差别的可能性也会增加（并非均值真的存在差别）。而方差分析则是同时考虑所有样本，排除了错误累积的概率，从而**避免拒绝一个真实的原假设**

方差分析（analysis ofvariance,ANOVA）:通过检验各总体的均值是否相等来判断分类型自变量对数值型因变量是否有显著影响。 
例：分析四个行业之间的服务质量是否有显著差异，即判断“行业”对”投诉次数“是否有显著影响。 
上述问题可转换为：检验四个行业被投诉次数的均值是否相等。 
在方差分析中，所要检验的对象称为**因素或因子**。因素的不同表现称为**水平或处理**。每个因子水平下得到的样本数据称为**观测值**。 

在上例中，行业是要检验的对象，称为因素或因子；零售业、旅游业等行业的具体表现，称为水平或处理；在每个行业下得到的样本数据（被投诉次数）称为观察值。由于只涉及行业一个因素，因此称为单因素4水平的试验。

**方差分析的基本思想和原理**：

为分析分类型自变量对数值型因变量的影响，需要从数据误差来源分析。 
(1) 图形描述 
(2) 误差分解 
思想：通过对数据误差来源的分析来判断不同总体的均值是否相等，进而分析自变量对因变量是否有显著影响。 
组内误差：同一总体下，观测值的差异，反映了一个样本内部数据的离散程度。只含随机误差。 
组间误差：不同总体之间的差异，反映了不同样本间的离散程度。是随机误差和系统误差的总和。 
总平方和：反映全部数据误差大小的平方和，反映了全部观察值的离散状况。 
总平方和（SST）=组内平方和（SSE）+组间平方和（SSA） 
组内平方和也称误差平方和或残差平方和 
组间平方和也称因素平方和 
(3) 误差分析 

以上例为例，如果不同行业对投诉次数没有影响，那么在组间误差中只包含随机误差，而没有组内误差，这时组间误差与组内误差经过平均后的数值就会接近1:1，反之，组间误差与组内误差的比值会大于1，当比值达到一定程度时，因素的不同水平之间即存在显著差异。

**方差分析中的基本假定：**

- 每个总体都应服从正态分布。例：每个行业被投诉的次数必须服从正态分布。 
- 各个总体的方差σ²必须相同。各组观察值是从具有相同方差的正态总体中抽取的。 
- 观察值是独立的。

**单因素方差分析：**

1. 提出假设
2. 构造检验的统计量
   1. 计算各样本的均值
   2. 计算全部观测值的总均值
   3. 计算各误差平方和
   4. 计算统计量
3. 统计决策

> 参考文章：
>
> [方差分析]: https://blog.csdn.net/troubleisafriend/article/details/48140497	"方差分析"

