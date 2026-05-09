## 用途
- 降维算法
- 将多个指标转换为少数几个主成分，主成分是原始数据的线性组合，且主成分间互不相关
- 用于变量较多，且变量之间有相关性

## 思想
- $\begin{aligned}\text{假设有}n\text{ 个评价对象,}p\text{ 个评价指标,则可构成大小为}n\times p\text{ 的样本矩阵}x{:}\\x=\begin{bmatrix}x_{11}&x_{12}&\cdots&x_{1p}\\x_{21}&x_{22}&\cdots&x_{2p}\\\vdots&\vdots&\ddots&\vdots\\x_{n1}&x_{n2}&\cdots&x_{np}\end{bmatrix}=(x_1,x_2,\cdots,x_p)\end{aligned}$
- $\begin{gathered}\text{假设我们想找到新的一组变量}z_1,z_2,\cdots,z_m(m\leq p),\text{ 且它们满足:}\\\begin{cases}z_1=l_{11}x_1+l_{12}x_2+\cdots+l_{1p}x_p\\z_2=l_{21}x_1+l_{22}x_2+\cdots+l_{2p}x_p\\\vdots\\z_m=l_{m1}x_1+l_{m2}x_2+\cdots+l_{mp}x_p&\end{cases}\end{gathered}$
- 系数 $l_{ij}$ 的确定原则： $(1)z_i$ 与 $z_j(i\neq j;\:i,j=1,2,\cdots,m)$ 相互无关；
 $(2)z_1$ 是 $x_1,x_2,\cdots,x_p$ 的一切线性组合中方差最大者；
 $(2)z_2$ 是与 $z_1$ 不相关的 $x_1,x_2,\cdots,x_p$ 的所有线性组合中方差最大者；
 $(3)依次类推$ $z_m$ 是与 $z_1,z_2,\cdotp\cdotp\cdotp,z_{m-1}$ 不相关的 $x_1,x_2,\cdotp\cdotp\cdotp,x_p$ 的所有线性
 $(4)$ 新变量指标 $z_1,z_2,\cdots,z_m$ 分别称为原变量指标 $x_1,x_2,\cdots,x_p$ 的第一，
 第二，..., 第 $m$ 主成分。

## 步骤
- 构造==样本矩阵==： $\begin{aligned}\text{假设有}n\text{ 个评价对象,}p\text{ 个评价指标,则可构成大小为}n\times p\text{ 的样本矩阵}x{:}\\x=\begin{bmatrix}x_{11}&x_{12}&\cdots&x_{1p}\\x_{21}&x_{22}&\cdots&x_{2p}\\\vdots&\vdots&\ddots&\vdots\\x_{n1}&x_{n2}&\cdots&x_{np}\end{bmatrix}=(x_1,x_2,\cdots,x_p)\end{aligned}$
- ==标准化==去量纲：
	- $X_{ij}={\frac{x_{ij}-{\overline{x_{j}}}}{S_{j}}}$ 其中标准差 $S_j=\sqrt{\frac{\sum_{i=1}^n\left(x_{ij}-\overline{x_j}\right)}{n-1}}$，均值 $\overline{x_j}=\frac1n\sum_{i=1}^nx_{ij}$
	- ==标准化矩阵== $X=\begin{bmatrix}X_{11}&X_{12}&\cdots&X_{1p}\\X_{21}&X_{22}&\cdots&X_{2p}\\\vdots&\vdots&\ddots&\vdots\\X_{n1}&X_{n2}&\cdots&X_{np}\end{bmatrix}=(X_1,X_2,\cdots,X_p)$
- 计算协方差矩阵：
	- $r_{ij}=\frac{1}{n-1}\sum_{k=1}^{n}(X_{ki}-\overline{X}_{i})(X_{kj}-\overline{X}_{j})=\frac{1}{n-1}\sum_{k=1}^{n}X_{ki}X_{kj}$
	- ==协方差矩阵== ${{R}=\begin{bmatrix}r_{11}&r_{12}&\cdots&r_{1p}\\ r_{21}&r_{22}&\cdots&r_{2p}\\\vdots&\vdots&\ddots&\vdots\\r_{p1}&r_{p2}&\cdots&r_{pp}\end{bmatrix}}$
- 标准化与计算协方差矩阵的步骤可以合并，计算==样本相关系数矩阵==
	- $R=\frac{\sum_{k=1}^n(x_{ki}-\overline{x_i})(x_{kj}-\overline{x_j})}{\sqrt{\sum_{k=1}^n(x_{ki}-\overline{x_i})^2\sum_{k=1}^n(x_{kj}-\overline{x_j})^2}}$ 为半正定矩阵，对称阵，主对角元素全是 1
	- 函数corrcoef
- 计算 $R$ 的特征值、特征向量
	- 用 matlab 中函数 eig（R）
- 计算主成分贡献率、累计贡献率
	- $\text{贡献率 }=\frac{\lambda_i}{\sum_{k=1}^p\lambda_k}(i=1,2,\cdots,p)$
	- $\text{累计贡献率}=\frac{\sum_{k=1}^i\lambda_{\mathrm{k}}}{\sum_{k=1}^p\lambda_k}\mathrm{~(~}i=1,2,\cdots,p\mathrm{~)}$
- 写出主成分
	- 取累计贡献率超过 80%的特征值所对应的主成分
	- $\text{第}i\text{个主成分:}F_i=a_{1i}X_1+a_{2i}X_2+\cdots+a_{pi}X_p\quad(i=1,2,\cdots,m)$
- 根据系数分析主成分代表的意义
	- 对于某个主成分而言，指标前面的系数越大，代表该指标对于该主成分的影响越大
- 利用主成分的结果进行后续的分析
	- (1)主成分得分：可用于评价类模型吗？（千万别用！）
	- (2)主成分可用于聚类分析（方便画图）
	- (3)主成分可用于回归分析

## 实战经验
- 例题一
	- ![[1705991041713.png]]
	- ![[1705990930228.png]]
	- ![[1705990998897.png]]
	- 主成分分析的困难之处主要在于要能够给出==主成分的较好解释==，所提取的主成分中如有一个主成分解释不了，整个主成分分析也就失败了
- 例题二
	- ![[1705991507287.png]]
- 例题三
	- ![[1705993799503.png]]
## 缺点
- 降维会损失原始数据信息
- 指标可能有各种类型，没有正向化处理过程
- 主成分分析不可用于计算得分 

## 主成分分析作用
- 主成分聚类
	- Spss 操作后面再看
	- 聚类效果图（两个主成分才能作图）
- 主成分回归
	- 解决多重共线性问题

## 代码
```matlab
	
```