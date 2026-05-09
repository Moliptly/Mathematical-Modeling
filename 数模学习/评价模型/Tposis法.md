
## 概念
## 用法
- 理想解：可能的最优方案
- 负理想解：可能的最劣方案
- 将可选方案与上述两解做比较，距离理想解最近，同时距离负理想解最远的方案即为==最优方案==
## 步骤
- 将原始矩阵==正向化==：所有指标转化为极大型指标
	- 极小型指标转化为极大型：$x^{'} = max-x$
	- 中间型指标转化为极大型：$x^{'}=1-\frac{|x_i-x_{best}|}{M},\ M=max\{|x_i-x_{best}|\}$
	- 区间型指标转化为极大型：
	$$ x^{'}=
		\begin{cases} 
		1-\frac{a-x_i}{M},\ x_i < a\\
		1,\ a \le x_i \le b\\
		1-\frac{x_i-b}{M},\ x_i > b
		\end{cases}
		\\\ M = max\{a-x_{min}, x_{max} - b\}
	$$
- 将正向化矩阵==标准化==：所有指标去量纲，保证数据排序不变
	- $Z_{ij}=\frac{x_{ij}}{\sqrt{\sum_{i=1}^nx_{ij}^2}} \ 其中\ X=\begin{bmatrix}x_{11}&\cdots&x_{1m}\\\vdots&\ddots&\vdots\\x_{n1}&\cdots&x_{nm}\end{bmatrix}\\, \\Z=\begin{bmatrix}z_{11}&\cdots&z_{1m}\\\vdots&\ddots&\vdots\\z_{n1}&\cdots&z_{nm}\end{bmatrix}$
- 计算得分：计算每个数据的权重
	- $$\text{定义最大值}\mathbb{Z}^+=(\mathbb{Z}_1^+,\mathbb{Z}_2^+,...,\mathbb{Z}_m^+)=(max\{z_{11},z_{21},...,z_{n1}\},max\{z_{12},z_{22},...,z_{n2}\},...,max\{z_{1m},z_{2m},...,z_{nm}\})\\Z=\begin{bmatrix}z_{11}&\cdots&z_{1m}\\\vdots&\ddots&\vdots\\z_{n1}&\cdots&z_{nm}\end{bmatrix}$$
	- $$\text{定义最小值}Z^-=(Z_1^-,Z_2^-,...,Z_m^-)=(min\{z_{11},z_{21},...,z_{n1}\},\quad min\{z_{12},z_{22},...,z_{n2}\},...,min\{z_{1m},z_{2m},...,z_{nm}\})$$
	- $得分：S_{i}=\frac{D_{i}^{-}} {{D_{i}^{+}+D_{i}^{-}}} \\ 其中D_{i}^{+}=\sqrt{\sum_{j=1}^{m}\left (Z_{j}^{+}-z_{ij}\right)^{2}}\\, \\ D_{i}^{-}=\sqrt{\sum_{j=1}^{m}\left (Z_{j}^{-}-z_{ij}\right)^{2}}$ 
- ==归一化得分==：$\widetilde{S_\mathrm{i}}=\frac{S_\mathrm{i}}{\sum_{i=1}^nS_\mathrm{i}}(\times 100\%)其中D_i^+为方案离最优解距离$ 
## 代码

``` python



```


