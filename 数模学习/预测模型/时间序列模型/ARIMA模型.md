## 理论部分
- 时间序列平稳性
	- 数据均值固定、方差存在为常数、协方差与只与间隔 s 有关与 t 无关
	- 默认的平稳为弱平稳性
- 白噪声序列（扰动项）
	- 数据均值为 0、方差存在为常数、协方差为 0
	- 为平稳序列
- 差分方程
	- 将某个时间序列变量表示为该变量的滞后项、时间、其他时间序列变量的函数方程
	- 三类：自回归 AR (p)模型、移动平均 MA (q)模型、自回归移动平均 ARMA (p, q) 模型
	- $y_t=\alpha_0+\alpha_1y_{t-1}+\alpha_2y_{t-2}+\cdots+\alpha_py_{t-p}+\varepsilon_t$ (自回归 AR$( p)$模型) $y_t=\varepsilon_t+\beta_1\varepsilon_{t-1}+\beta_2\varepsilon_{t-2}+\cdots+\beta_q\varepsilon_{t-q}$ (移动平均 MA $(q)$ 模型)
      $y_t=\alpha_0+\sum_{i=1}^p\alpha_iy_{t-i}+\varepsilon_t+\sum_{i=1}^q\beta_i\varepsilon_{t-i}$ (自回归移动平均 ARMA $(p,q)$ 模型)
      $y_t=a+by_{t-1}+cz_t+dz_{t-1}+\varepsilon_t$ 
      $y_t=a+by_{t-1}+ct+\varepsilon_t$
      注：$\varepsilon_t$ 没有特殊说明一般默认为白噪声序列。
    - 差分方程的齐次部分：只包含该变量自身和它的滞后项的式子
    - 差分方程齐次部分转化为齐次方程求解，解的模长反应 $y_t$ 的平稳性
- 滞后算子L
	- 定义： $L^iy_t=y_{t-i}$
	- 性质：$$\begin{aligned}(1)&LC=C\quad(C\text{为常数})\\(2)&(L^i+L^j)y_t=y_{t-i}+y_{t-j}\\(3)&L^iL^jy_t=y_{t-i-j}\end{aligned}$$
	- $d\text{阶差分}:\Delta^dy_t=(1-L)^dy_t$
	- $$\begin{aligned}
&季节差分( m\text{为周期)}: 
&&\Delta y_t-\Delta y_{t-m}=(y_t-y_{t-1})-(y_{t-m}-y_{t-m-1})  \\
&&=& (1-L-L^m+L^{m+1})y_t  \\
&&=& (1-L)(1-L^m)y_t 
\end{aligned}$$
- AR（p）模型
	- 定义：自回归，将自己的滞后项视为变量，曲线受到自身历史因素较大，均值相对平稳
	- 扰动项：1. 独立同分布 2. 异方差 3. 自相关
	- AR 模型一定是平稳的，不平稳转化为平稳
	- AR（p）模型平稳条件
		- 差分方程齐次部分转化为特征方程求解（$y_t=x^t$）
		- 解的模长均小于 1，$y_t$ 平稳，模型平稳
		- k 个解模长为 1，$y_t$ 为 k 阶单位根过程，k 阶差分转化为平稳
		- 存在解大于 1，$y_t$ 为爆炸过程，呈指数增长，一般不出现
	- 快速判别平稳性：
		- $对于AR( p)$模型而言：
		- $y_t=\alpha_0+\alpha_1y_{t-1}+\alpha_2y_{t-2}+\cdots+\alpha_py_{t-p}+\varepsilon_t$
		- $\{y_t\}$ 为单位根的的充要条件：
		- $\alpha_1+\alpha_2+\cdots+\alpha_p=1$
		- $\{y_t\}$ 平稳的充分条件：
		- $|\alpha_1|+|\alpha_2|+\cdotp\cdotp\cdotp+|\alpha_p|<1$
		- $\{y_t\}$ 平稳的必要条件：
		- $\alpha_1+\alpha_2+\cdots+\alpha_p<1$
- MA (q)模型
	- 定义：扰动项随时间变化
	- MA 一定是平稳的
	- MA（1）可以转化为 AR（无穷），也可以逆转化
	- 将 AR（无穷）转化为 MA（1）以减少参数

- ARMA（p, q）模型
	- 定义：AR 与 MA 过程结合 $\Leftrightarrow(1-\sum_{i=1}^p\alpha_iL^i)y_t~=~\alpha_0+(1+\sum_{i=1}^q\beta_iL^i)\varepsilon_t$
	
- ACF 自相关系数
	- 使用前提：时间序列平稳
	- 自相关系数反应了平稳序列中，间隔 S 期的两个时间点之间的相关系数
	- $\text{自相关系数: }\rho_s=\frac{cov\left(x_i,x_{i-s}\right)}{\sqrt{Var\left(x_i\right)}\sqrt{Var\left(x_{i-s}\right)}}=\frac{\gamma_s}{\gamma_0}\quad\text{注意:}\quad\gamma_0=Cov\left(x_i,x_i\right)=Var\left(x_i\right)$
	- $\text{样本自相关系数: }r_s=\widehat{\rho}_s=\frac{\sum_{t=s+1}^T\left(x_t-\overline{x}\right)\left(x_{t-s}-\overline{x}\right)}{\sum_{i=1}^T(x_i-\overline{x})^2}$
	- $\left.\text{如果}\{\varepsilon_t\}\text{是白噪声序列,那么}\rho_s=\left\{\begin{array}{ll}1,&s=0\\0,&s\neq0\end{array}\right.\right.$
- PACF 偏自相关系数
	- AR (p) ==ACF：逐渐衰减，即拖尾  PACF：P 阶后截尾==
	- MA (q) A ==CF：Q 阶后截尾        PACF：逐渐衰减，即拖尾==
	- ARMA (p, q) ==ACF：逐渐衰减，即拖尾   PACF：逐渐衰减，即拖尾==
	- 当ACF、PACF图都是拖尾或者截尾时，说明方法失效
- ARMA 模型识别以及阶数确定
	- 做样本自相关系数与偏自相关系数图
	- 确定 $H_0:\rho_s =0$ 的相关系数的置信区间
	- 在置信区间内的相关系数视为 0，否则视为异于 0
	- 确定截尾与拖尾，来确定模型以及阶数
- ARMA 模型估计
	- 极大似然估计，用计算机编程求解
- 模型规模选择
	- 过拟合问题：参数越多，拟合效果越好，复杂度越高，折中处理
	- AIC 准则：AIC = 2 (模型中参数的个数)一 2 ln (模型的极大似然函数值)
	- BIC 准则：BIC=ln (n)(模型中参数的个数)-2 ln (模型的极大似然函数值)
	- AIC，BIC 越小越好，且 BIC 选择的模型复杂度更低
- 检验模型是否识别完全
	- 对残差进行白噪声检验
		- 残差是白噪声，则模型可以接受
		- 残差不是白噪声，则模型识别不完全
	- Q 检验
		- $H_0{:}\rho_1=\rho_2=\cdots=\rho_s=0,\quad H_1{:}\rho_i(i=1,2,\cdots,s)至少有一个不为0$ 
		- $\\在H_{0}$ 成立的条件下，统计量 $Q=T(T+2)\sum_{k=1}^s\frac{r_k^2}{T-k}$~$\chi_{s-n}^2$
		 - 软件会给我们算出 $p$ 值，$p$ 值小于 0.05 则拒绝原假设，此时模型没有识别完全，需要修正
		 注：
		 $(1)T$ 表示样本个数
		 $(2)n$ 表示模型中未知参数的个数 (例如 ARMA ($p,q)$ 模型中，$n=p+q+1)$
		 $(3)s$ 根据样本量的大小一般可以取 8，16,24 等 (SPSS 软件取的是 18)

- ARIMA (p, d, q)模型
	- 定义：含有差分的 ARMA 模型，$y_t^{\prime}=\alpha_0+\sum_{i=1}^p\alpha_iy_{t-i}^{\prime}+\varepsilon_t+\sum_{i=1}^q\beta_i\varepsilon_{t-i}$，其中 $y_t^{\prime}=\Delta^dy_t=(1-L)^dy_t$ 即为 $(1-\sum_{i=1}^p\alpha_iL^i)(1-L)^dy_t=\alpha_0+(1+\sum_{i=1}^q\beta_iL^i)\varepsilon_t$
	- 差分可以让模型平稳
- SARIMA（p, d, q）(P, D, Q)_m 季节性的 ARIMA 模型
	- $\begin{array}{ccc}\text{SARIMA}&\underbrace{(p,d,q)}&\underbrace{(P,D,Q)_m}\\&\uparrow &\uparrow\\&\text{非季节部分}&\text{季节部分}\end{array}$
	- $(1-\sum_{i=1}^p\phi_iL^i)(1-\sum_{i=1}^p\Phi_iL^{mi})(1-L)^d(1-L^m)^Dy_t~=~\alpha_0+(1+\sum_{i=1}^q\theta_iL^i)(1+\sum_{i=1}^q\Theta_iL^{mi})\varepsilon_t$


## 实战部分
- 见[SPSS实践部分](SPSS实践部分.md)