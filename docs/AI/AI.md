# AI

## 二分分类

记号，输入是一个向量x，($nx\times 1$维)，输出y=0或1.

sigmoid函数: $y = \dfrac{1}{1 + e^{-z}}$

$z = w^Tx + b$，x是单个样本。

一个经典的损失函数: $L(\hat{y},y) = -(y\log\hat{y} + (1-y)\log(1-\hat{y}))$

成本函数：$J(w,b)=\dfrac{1}{m}\sum\limits_{i=1}^mL(\hat{y}^{(i)},y^{(i)})$

先初始化 J w b,再求出J对每个w分量的偏导，根据学习系数$\alpha$求出下一个w，同样的对b