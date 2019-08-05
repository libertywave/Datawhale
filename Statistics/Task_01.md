> [TOC]

#### 一、统计学基本知识

统计学是在数据分析的基础上，研究测定、收集、整理、归纳和分析反映数据数据，以便给出正确消息的科学。

**平均数**：集中趋势的最常用测度值，确定一组数据的均衡点，适用于数值型数据，不能用于分类数据和顺序数据。

- 算术平均数：n个数据相加后除以n,记作 ![\bar{x}](https://wikimedia.org/api/rest_v1/media/math/render/svg/466e03e1c9533b4dab1b9949dad393883f385d80)
- 几何平均数：*n*个数据相乘后开 *n* 次方
- 调和平均数：*n*个数据的倒数取算术平均，再取倒数
- 平方平均数（均方根）：*n* 个数据的平方取算数平均，再开根号

**中位数**：处于序列中间位置的数

![\mathrm {Q} _{\frac {1}{2}}(x)={\begin{cases}x'_{\frac {n+1}{2}},&{\mbox{if }}n{\mbox{ is odd number.}}\\{\frac {1}{2}}(x'_{\frac {n}{2}}+x'_{{\frac {n}{2}}+1}),&{\mbox{if }}n{\mbox{ is even number.}}\end{cases}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0236b0544a51e8c4a225dad3e633c991759a47b8)

**众数**：一组数据中出现次数最多的数据值，主要用于分类数据，也可用于顺序数据和数值型数据

**四分位数**：把所有数值由小到大排列并分成四等份，处于三个分割点位置的数值就是四分位数

- 第一四分位数：又称较小四分位数，等于该样本中所有数值由小到大排列后第25%的数字。

- 第二四分位数：又称中位数，等于该样本中所有数值由小到大排列后第50%的数字。

- 第三四分位数：又称较大四分位数，等于该样本中所有数值由小到大排列后第75%的数字。

  第三四分位数与第一四分位数的差距又称四分位距（IQR）。

**方差**：设X为服从分布F的随机变量， 如果E[X]是随机变数*X*的期望值（平均数*μ*=E[*X*]）
随机变量X或者分布F的方差为：

![\operatorname{Var}(X) = \operatorname{E}\left[(X - \mu)^2 \right]](https://wikimedia.org/api/rest_v1/media/math/render/svg/06e01a0d2205e0db3118b14c3f6f06cfc5addc52)

这个定义涵盖了连续、离散、或两者都有的随机变量。方差亦可当作是随机变量与自己本身的协方差(或协方差)：

![\operatorname{Var}(X) = \operatorname{Cov}(X, X)](https://wikimedia.org/api/rest_v1/media/math/render/svg/922856b96a7eeb183632901851974b84d3053586)

方差典型的标记有Var(*X*)，![\scriptstyle\sigma_X^2](https://wikimedia.org/api/rest_v1/media/math/render/svg/e0d458a02ef16c04d94f4188fc90817c9945e4c2),　或是![\sigma^{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/53a5c55e536acf250c1d3e0f754be5692b843ef5)，也可记为"平方的期望减掉期望的平方"。

#### 二、二项分布

​	定义：是n个独立的是/非试验中成功的次数的离散概率分布，其中每次试验的成功概率为p。这样的单次成功/失败试验又称为伯努利试验。实际上，当n = 1时，二项分布就是伯努利分布。二项分布是显著性差异的二项试验的基础。

**概率质量函数：**一般地，如果随机变量![{\mathit {X}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8c2a3b4525e74184898cd922c3487c294367930b)服从参数为![{\mathit {n}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f8379ab758c42519867fc601fa155ea3175a05b3)和![{\mathit {p}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1f74855490ee91798c08fc6e61c81a2ff6aa6a5b)的二项分布，我们记![X\sim b(n,p)](https://wikimedia.org/api/rest_v1/media/math/render/svg/5dd4762022c5a1e62425892a9c897f97e1e1e949)或![X\sim B(n,p)](https://wikimedia.org/api/rest_v1/media/math/render/svg/d561b3cbc6297b3cd2313ac82686b3e553613d4d)。n次试验中正好得到*k*次成功的概率由概率密度函数]给出：

![{\displaystyle f(k;n,p)=\Pr(X=k)={n \choose k}p^{k}(1-p)^{n-k}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8d166dfa3dfbfbf2e2672144bf2d9f25a1c77503)

对于*k* = 0, 1, 2, ..., *n*，其中![{n \choose k}={\frac {n!}{k!(n-k)!}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/420bf080448b0b64ddd2eaeaa6a9c2cb8fd6923b)

是二项式系数这就是二项分布的名称的由来），又记为*C*(*n*, *k*)，*n**C**k*，或*n**C**k*。我们希望有*k*次成功(*p**k*)和*n* − *k*次失败(1 − *p*)*n* − *k*。然而，*k*次成功可以在*n*次试验的任何地方出现，而把*k*次成功分布在*n*次试验中共有C(*n*, *k*)个不同的方法。

**累积分布函数**：可以表示为：

![{\displaystyle F(x;n,p)=\Pr(X\leq x)=\sum _{i=0}^{\lfloor x\rfloor }{n \choose i}p^{i}(1-p)^{n-i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/da49806ae1476696ecfe7dc23efd83da315f766d)

其中![\scriptstyle \lfloor x\rfloor \,](https://wikimedia.org/api/rest_v1/media/math/render/svg/be005bf5bb3ea1b9176216152d5442e37d88a875)是小于或等于*x*的最大整数。

**期望：**如果*X* ~ *B*(*n*, *p*)（也就是说，*X*是服从二项分布的随机变量），那么*X*的期望值为

![\operatorname {E} [X]=np](https://wikimedia.org/api/rest_v1/media/math/render/svg/8a847aa9a0c1fc2751c00a6b9cb4be55e784e88a)

方差为

![\operatorname {Var} [X]=np(1-p).](https://wikimedia.org/api/rest_v1/media/math/render/svg/aa57bb99dc27f5bcee3d3e63bff1952994b3bb70)

#### 三、泊松分布

若![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)服从参数为![\lambda ](https://wikimedia.org/api/rest_v1/media/math/render/svg/b43d0ea3c9c025af1be9128e62a18fa74bedda2a)的泊松分布，记为![X \sim \pi(\lambda)](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8d05295235f7c13ecbdfefaa93bdabde3bbd944)，或记为![X \sim P(\lambda)](https://wikimedia.org/api/rest_v1/media/math/render/svg/d8837e24e776d86081e4921599c7c0c31f4483e3)

概率质量函数：**![P(X=k)=\frac{e^{-\lambda}\lambda^k}{k!}](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f4eb63a8e6d46d0f7c642426ca59531507c5a9e)

服从泊松分布的随机变量，其数学期望与方差相等，同为参数![\lambda](https://wikimedia.org/api/rest_v1/media/math/render/svg/b43d0ea3c9c025af1be9128e62a18fa74bedda2a)![{\displaystyle E(X)=V(X)=\lambda }](https://wikimedia.org/api/rest_v1/media/math/render/svg/b94d1b82df275fd71a88f418f348cacf5b64b6b6)

泊松分布来源（泊松小数定律）：

在二项分布的伯努利试验中，如果试验次数n很大，二项分布的概率p很小，且乘积λ= *np*比较适中，则事件出现的次数的概率可以用泊松分布来逼近。事实上，二项分布可以看作泊松分布在离散时间上的对应物。

证明如下。首先，回顾*e*的定义：

![\lim_{n\to\infty}\left(1-{\lambda \over n}\right)^n=e^{-\lambda},](https://wikimedia.org/api/rest_v1/media/math/render/svg/163448ac05b0672de07336945cf8c0e8fbcbf508)

二项分布的定义：

![{\displaystyle P(X=k)={n \choose k}p^{k}(1-p)^{n-k}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/94cdf18e27c5641888c81d31d97dd447dbaa72fe)

如果令![p = \lambda/n](https://wikimedia.org/api/rest_v1/media/math/render/svg/138086d0e6e280e1f03522ff76266e2b730b200d), ![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)趋于无穷时![P](https://wikimedia.org/api/rest_v1/media/math/render/svg/b4dc73bf40314945ff376bd363916a738548d40a)的极限：

![ \begin{align}  \lim_{n\to\infty} P(X=k)&=\lim_{n\to\infty}{n \choose k} p^k (1-p)^{n-k} \\  &=\lim_{n\to\infty}{n! \over (n-k)!k!} \left({\lambda \over n}\right)^k \left(1-{\lambda\over n}\right)^{n-k}\\ &=\lim_{n\to\infty} \underbrace{\left[\frac{n!}{n^k\left(n-k\right)!}\right]}_F \left(\frac{\lambda^k}{k!}\right) \underbrace{\left(1-\frac{\lambda}{n}\right)^n}_{\to\exp\left(-\lambda\right)} \underbrace{\left(1-\frac{\lambda}{n}\right)^{-k}}_{\to 1} \\ &= \lim_{n\to\infty} \underbrace{\left[ \left(1-\frac{1}{n}\right)\left(1-\frac{2}{n}\right) \ldots \left(1-\frac{k-1}{n}\right)  \right]}_{\to 1} \left(\frac{\lambda^k}{k!}\right) \underbrace{\left(1-\frac{\lambda}{n}\right)^n}_{\to\exp\left(-\lambda\right)} \underbrace{\left(1-\frac{\lambda}{n}\right)^{-k}}_{\to 1}      \\ &= \left(\frac{\lambda^k}{k!}\right)\exp\left(-\lambda\right) \end{align} ](https://wikimedia.org/api/rest_v1/media/math/render/svg/7f3305a795ed8b2ce32f86487f67b44ecd5f69cb)

#### 四、大数定律

​	样本数量越多，则其算术平均值就有越高的概率接近期望值

- 弱大数定律：也称辛钦定理，陈述为：样本均值依概率收敛于期望值

![{\displaystyle {\overline {X}}_{n}\ {\xrightarrow {P}}\ \mu \quad {\textrm {as}}\quad n\to \infty }](https://wikimedia.org/api/rest_v1/media/math/render/svg/c4eea1b3848e0c3512a0a06b092cf028d881e1e0)

​		也就是说对于任意正数 *ε*,

![{\displaystyle \lim _{n\to \infty }P\left(\,|{\overline {X}}_{n}-\mu |>\varepsilon \,\right)=0}](https://wikimedia.org/api/rest_v1/media/math/render/svg/b1dc8f599ffcdee479238a9ce8089fa605419ecc)

- 强大数定律：样本均值以概率1收敛于期望值

![{\displaystyle {\overline {X}}_{n}\ {\xrightarrow {\text{a.s.}}}\ \mu \quad {\textrm {as}}\quad n\to \infty }](https://wikimedia.org/api/rest_v1/media/math/render/svg/79105e7ae05a468724afe18420794bb2aa1cb42e)

​		即

![{\displaystyle P\left(\lim _{n\to \infty }{\overline {X}}_{n}=\mu \right)=1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1f754216a1516dd0afe8c78169fbdf1bb7f4d417)

- 切比雪夫定理的特殊情况

  设![{\displaystyle a_{1},\ a_{2},\ \dots \ ,\ a_{n},\ \dots }](https://wikimedia.org/api/rest_v1/media/math/render/svg/48696257ac29b856f045ffe5e600afb736eabd20)为相互独立的随机变量，其数学期望为：![{\displaystyle \operatorname {E} (a_{i})=\mu \quad (i=1,\ 2,\ \dots )}](https://wikimedia.org/api/rest_v1/media/math/render/svg/396b674aefcd43cd57e86421386a0f5e95d2354c)，方差为：![{\displaystyle \operatorname {Var} (a_{i})=\sigma ^{2}\quad (i=1,\ 2,\ \dots )}](https://wikimedia.org/api/rest_v1/media/math/render/svg/88a4de3a1d70fbc6f17d328eb69d9605641c8069)

  则序列![{\overline {a}}={\frac {1}{n}}\sum _{i=1}^{n}a_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/eacae1df43555dbc1fe92af96bded999c86b53c4)依概率收敛于![\mu ](https://wikimedia.org/api/rest_v1/media/math/render/svg/9fd47b2a39f7a7856952afec1f1db72c67af6161)（即收敛于此数列的数学期望![E(a_{i})](https://wikimedia.org/api/rest_v1/media/math/render/svg/7e2efbc7bd2209a4901adcedd04950711a879e72)）。换言之，在定理条件下，当}![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)无限变大时，![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)个随机变量的算术平均将变成一个常数。

- 伯努利大数定律

  设在![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)次独立重复伯努利试验中，
  事件![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)发生的次数为![n_{x}](https://wikimedia.org/api/rest_v1/media/math/render/svg/2a811d9f1857b131167bd4238be8eb175ae3e85d)。
  事件![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)在每次试验中发生的总体概率为![p](https://wikimedia.org/api/rest_v1/media/math/render/svg/81eac1e205430d1f40810df36a0edffdc367af36)。
  ![{\displaystyle {\frac {n_{x}}{n}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/2bf806e16edad35b4611581be04cf138de89fa52)代表样本发生事件![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)的频率。

  大数定律可用概率极限值定义: 则对任意正数![\varepsilon >0](https://wikimedia.org/api/rest_v1/media/math/render/svg/e04ec3670b50384a3ce48aca42e7cc5131a06b12)，下式成立：

  ![\lim _{n\to \infty }{P{\left\{\left|{\frac {n_{x}}{n}}-p\right|<\varepsilon \right\}}}=1](https://wikimedia.org/api/rest_v1/media/math/render/svg/b488a8ff684a74e3db311682dbc2cca72d3df26f)

  定理表明事件发生的频率依概率收敛于事件的总体概率。
  定理以严格的数学形式表达了频率的稳定性。
  就是说当![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)很大时，事件发生的频率于总体概率有较大偏差的可能性很小。

#### 五、正态分布

​	若随机变量![X](https://wikimedia.org/api/rest_v1/media/math/render/svg/68baa052181f707c662844a465bfeeb135e82bab)服从一个位置参数为![\mu ](https://wikimedia.org/api/rest_v1/media/math/render/svg/9fd47b2a39f7a7856952afec1f1db72c67af6161)、尺度参数为![\sigma ](https://wikimedia.org/api/rest_v1/media/math/render/svg/59f59b7c3e6fdb1d0365a494b81fb9a696138c36)的正态分布，记为：

![X \sim N(\mu,\sigma^2),](https://wikimedia.org/api/rest_v1/media/math/render/svg/2d941a7a56caf1f30802497907657ab6213f539d)

​	**概率密度函数**为：

![f(x) = {1 \over \sigma\sqrt{2\pi} }\,e^{- {{(x-\mu )^2 \over 2\sigma^2}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a4145febbfa700e8711b7bc87fa1dbf9ec37f906)

正态分布的数学期望]值或期望值![\mu ](https://wikimedia.org/api/rest_v1/media/math/render/svg/9fd47b2a39f7a7856952afec1f1db72c67af6161)等于位置参数，决定了分布的位置；其方差![\sigma^2](https://wikimedia.org/api/rest_v1/media/math/render/svg/53a5c55e536acf250c1d3e0f754be5692b843ef5)的开平方或标准差![\sigma ](https://wikimedia.org/api/rest_v1/media/math/render/svg/59f59b7c3e6fdb1d0365a494b81fb9a696138c36)等于尺度参数，决定了分布的幅度。正态分布的概率密度函数曲线呈钟形，因此人们又经常称之为**钟形曲线**。

标准正态分布期望为0，标准差为1：

![f(x) = \frac{1}{\sqrt{2\pi}} \, \exp\left(-\frac{x^2}{2} \right)](https://wikimedia.org/api/rest_v1/media/math/render/svg/3a348e5fe756ab8d67267b7fa8734ac1ec2ba548)

**累积分布函数**为：

![ F(x;\mu,\sigma) = \frac{1}{\sigma\sqrt{2\pi}} \int_{-\infty}^x  \exp  \left( -\frac{(t - \mu)^2}{2\sigma^2} \ \right)\, dt. ](https://wikimedia.org/api/rest_v1/media/math/render/svg/4ba40a6beb0e386c70b5961d623e8fbd29aca084)