---
title: LS和LMMSE信道估计
tags: 信道估计
mathjax: true
abbrlink: f63a5608
date: 2019-03-18 19:47:35
---

## System Model
---
$$ Y=XH+N $$ 其中 $Y\in \mathbb{C}^{m \times 1}$ 表示接收符号；$H\in \mathbb{C}^{n \times 1}$ 表示信道冲击响应；$X\in \mathbb{C}^{m \times n}$ 表示发送符号，即导频，其元素 $x_{ij}$ 服从$CN(0,\sigma_x^2)$；$N\in \mathbb{C}^{m \times 1}$ 表示叠加在信道上的加性高斯白噪声，其元素 $n_{ij}$ 服从$CN(0,\sigma_n^2)$；$H$ 和 $N$ 相互独立。

问题：
+ 根据接收符号 $Y$ 和导频 $X$ 估计 $H$，其估计值用 $\hat{H}$ 表示。
    
## LS Estimation
---
> LS 即 **最小二乘**（Least Square)，主要思想在于使理论值尽量逼近观测值，其追求在方程求解、拟合和建模中的 **误差平方和** 最小；二乘即为平方的意思

用 $\hat{Y}$ 表示发送符号 $X$ 经过估计信道 $\hat{H}$ 后的输出信号，即 $\hat{Y}=X\hat{H}$， 则有
$$ J=(\hat{Y}-Y)^H(\hat{Y}-Y)=(X\hat{H}-Y)^H(X\hat{H}-Y) \tag{1}$$在 LS 的估计准则下，需要找到使得 $J$ 最小的 $\hat{H}$，即 $$\begin{aligned}
\frac{\partial J}{\partial{\hat{H}}} &=  \frac{\partial}{\partial{\hat{H}}}(X\hat{H}-Y)^H(X\hat{H}-Y) \\ &=\frac{\partial}{\partial{\hat{H}}}(\hat{H}^HX^HX\hat{H}-\hat{H}^HX^HY-Y^HX\hat{H}+Y^HY) \\ &= (X^HX+XX^H)\hat{H}-X^HY - X^HY \\ &= 2\hat{H}XX^H-2YX^H
\end{aligned}\tag{2}$$
令 $\frac{\partial J}{\partial{\hat{H}}}=0$,则可得到 $H$ 的估计值为
$$\hat{H} = YX^{-1} \tag{3}$$

## LMMSE Estimation
---  
> 定义 $X$ 为 $n \times 1$ 隐随机向量，$Y$ 为 $m \times 1$ 随机向量（测量值或观测值），用 $\hat{X}(Y)$ 表示对 $X$ 的估计值，那么估计的误差可以表示为 $e=\hat{x}-x$，因此 **均方误差** （Mean Squared Error, MSE）为 $$MSE = tr\left\lbrace E\left\lbrace(\hat{X}-X)(\hat{X}-X)^H\right\rbrace\right\rbrace = E\left\lbrace (\hat{X}-X)^H(\hat{X}-X)\right\rbrace$$ MMSE（最小均方误差，Minimum Mean Square Error）的估计准则是寻找使得 MSE 最小的 $\hat{X}$，即$$\hat{X}(Y)=argmin_{\hat{X}} MSE$$LMMSE（线性最小均方误差，Linear Minimum Square Error）的估计准则把估计量构造成观测量的线性函数，同时要求估计量的 MSE 最小，即
$$\hat{X}(Y)=argmin_{\hat{X}} MSE \quad s.t. \hat{X}=WY+B$$

根据 LMMSE 的估计准则，先将 $\hat{H}$ 构造成 $Y$ 的线性函数
$$\hat{H}=WY+B \tag{4}\label{XEstimate}$$那么 MSE 可以写作
$$\begin{aligned}
MSE &= E\left\lbrace(\hat{H}-H)^H(\hat{H}-H)\right\rbrace \\
&= E\left\lbrace(WY+B-H)^H(WY+B-H)\right\rbrace \\
&= E\left\lbrace Y^HW^HWY+B^HWY+Y^HW^HB+B^HB-Y^HW^HH \\ -B^HH-H^HWY-H^HB+H^HH\right\rbrace
\end{aligned}\tag{5}$$将 MSE 分别对 $W$ 和 $B$ 求偏导
+ 对 $B$ 求偏导
$$
\begin{aligned}
\frac{\partial{MSE}}{\partial{B}}&=\frac{\partial{E\left\lbrace(\hat{H}-H)^H(\hat{H}-H)\right\rbrace}}{\partial{B}}\\&=E\left\lbrace\frac{\partial{\left\lbrace(\hat{H}-H)^H(\hat{H}-H)\right\rbrace}}{\partial{B}}\right\rbrace\\&=E\left\lbrace2WY+2B-2H\right\rbrace\\&=2W\overline{Y}+2B-2\overline{H}
\end{aligned}\tag{6}\label{BWeifen}
$$令 $\frac{\partial{MSE}}{\partial{B}}=0$，可得到 $$B=\overline{H}-W\overline{Y}\tag{7}\label{B}$$
+ 对 $W$ 求偏导，并将 $B$ 代入到表达式中
$$
\begin{aligned}
\frac{\partial{MSE}}{\partial{W}}&=\frac{\partial{E\left\lbrace(\hat{H}-H)^H(\hat{H}-H)\right\rbrace}}{\partial{W}}\\&=E\left\lbrace\frac{\partial{\left\lbrace(\hat{H}-H)^H(\hat{H}-H)\right\rbrace}}{\partial{W}}\right\rbrace\\&=E\left\lbrace 2WYY^H+2BY^H-2HY^H \right\rbrace \\&\overset{\eqref{B}}{=}E\left\lbrace 2W(YY^H-\overline{Y}Y^H)+2(\overline{H}-X)Y^H \right\rbrace \\
&=  2W\left\lbrace E(YY^H)-E(\overline{Y}Y^H) \right\rbrace +2\overline{H}E(Y^H)-2E(HY^H) \\
&= 2WC_{Y}-2C_{HY} 
\end{aligned} \tag{8}
$$令 $\frac{\partial{MSE}}{\partial{W}}=0$，可得到
$$W=C_{HY}C_{Y}^{-1} \tag{9}\label{W}$$
 
根据系统模型，在公式 $\eqref{BWeifen}$ - $\eqref{W}$ 中: 
$$\begin{aligned}
\overline{Y}&=E{Y}=A\overline{X}\\
C_Y&=AC_HA^T+C_N \\
C_{HY}&=C_XA^H
\end{aligned} \tag{10}
$$ $C_Y$ 表示 $Y$ 的自协方差矩阵，$C_{HY}$ 表示 $H$ 和 $Y$ 的互协方差矩阵。至此，可得到 $H$ 的估计值
$$
\begin{aligned}
 \hat{H}&=C_{HY}C_Y^{-1}(Y-\overline{Y})+\overline{H} \\
 &=C_HA^H(AC_HA^H+C_Z)^{-1}(Y-A\overline{H})+\overline{H}
\end{aligned} \tag{11}
$$



