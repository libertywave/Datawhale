#### 一、中心极限定理

​	在适当的条件下，大量相互独立随机变量的均值经适当标准化后收敛于正态分布（维基百科）

> 每次从这些总体中随机抽取 n 个抽样，一共抽 m 次。 然后把这 m 组抽样分别求出平均值。 这些平均值的分布接近正态分布
>
> **样本的平均值约等于总体的平均值。不管总体是什么分布，任意一个总体的样本平均值都会围绕在总体的整体平均值周围，并且呈正态分布。**

```python
# 模拟掷骰子10000次
# 理论平均值为3.5
import numpy as np 
random_data = np.random.randint(1, 7, 10000)
print random_data.mean() # 打印平均值
print random_data.std()  # 打印标准差

# 随机抽取十个样本
sample1 = []
for i in range(0, 10):
    sample1.append(random_data[int(np.random.random() * len(random_data))])
 
print sample1 # 打印出来
print(sample1.mean())

# 抽取1000组，每次50个样本
samples = []
samples_mean = []
samples_std = []
 
for i in range(0, 1000):
    sample = []
    for j in range(0, 50):
        sample.append(random_data[int(np.random.random() * len(random_data))])
    sample_np = np.array(sample)
    samples_mean.append(sample_np.mean())
    samples_std.append(sample_np.std())
    samples.append(sample_np)
 
samples_mean_np = np.array(samples_mean)
samples_std_np = np.array(samples_std)
 
print samples_mean_np
```

用途：

**1）在没有办法得到总体全部数据的情况下，我们可以用样本来估计总体**

**2）根据总体的平均值和标准差，判断某个样本是否属于总体**

#### 二、置信区间和置信水平

考虑一个一维随机变量![{\displaystyle {\cal {X}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/665536ed9698be58cd8bacaf30a527aa0be1649a)服从分布![{\displaystyle {\cal {F}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8b2f53754385cb648fe2b298deaded2924b57670)，又假设![\theta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e5ab2664b422d53eb0c7df3b87e1360d75ad9af)是![{\displaystyle {\cal {F}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8b2f53754385cb648fe2b298deaded2924b57670)的参数之一。假设我们的数据采集计划将要独立地抽样![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)次，得到一个随机样本![{\displaystyle \{X_{1},\ldots ,X_{n}\}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/c3f57ea988a26a3da89f2d0080ba01e35cbcd0d3)，注意这里所有的![X_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/af4a0955af42beb5f85aa05fb8c07abedc13990d)都是随机的，我们是在讨论一个尚未被观测的数据集。如果存在**统计量**![{\displaystyle X=\{X_{1},\ldots ,X_{n}\}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/dcbf39e54811783de5bfd141d419c65a4845dde2)的一个函数，且不得依赖于任何未知参数![{\displaystyle u(X_{1},\ldots ,X_{n}),v(X_{1},\ldots ,X_{n})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/836470a4de4b5d59afa0925850fd248ebd75057e)满足![{\displaystyle u(X_{1},\ldots ,X_{n})<v(X_{1},\ldots ,X_{n})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/b78239ec4c7891ee9768dcabf5f0f1d7835fe2f1)使得：

![{\displaystyle \mathbb {P} \left(\theta \in \left(u(X_{1},\ldots ,X_{n}),v(X_{1},\ldots ,X_{n})\right)\right)=1-\alpha }](https://wikimedia.org/api/rest_v1/media/math/render/svg/3085fef2fc2500357e32ff14d08d63d9eaf5f236)

则称![{\displaystyle \left(u(X_{1},\ldots ,X_{n}),v(X_{1},\ldots ,X_{n})\right)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bac4afba99cd571f36b00af22af63354f9d34c8f)为一个用于估计参数![\theta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e5ab2664b422d53eb0c7df3b87e1360d75ad9af)的![1-\alpha ](https://wikimedia.org/api/rest_v1/media/math/render/svg/9afa7876fb8b4fb8c4d8039ebed6cd1cbc4781cd)置信区间，其中的![1-\alpha ](https://wikimedia.org/api/rest_v1/media/math/render/svg/9afa7876fb8b4fb8c4d8039ebed6cd1cbc4781cd)称为**置信水平**。

![微信图片_20190805182020](C:\Users\Vodka\Desktop\微信图片_20190805182020.jpg)