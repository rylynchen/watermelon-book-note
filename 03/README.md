# 3.1 基本形式
* 预测函数
	f(x) = w1*x1 + w2*x2 + ... + wd*xd + b
		 = wT * x + b

# 3.2 线性回归
* 试图学习
  * f(xi) = w * xi + b , 使得 f(xi) = yi
  * 如何确定w和b
  * 均方误差(欧几里得距离，欧氏距离)最小化
  ```
  min E(w, b) = min SUM( i=1, i<=m, (f(xi) - yi) ** 2 )
              = min SUM( i=1, i<=m, (yi - w * xi -b) ** 2)
  ```
  * 最小二乘 : 基于均方误差求解的方法
  * 对w进行求导
  ```
  dE(w,b)/dw = dSUM(i=1, i<=m, (y**2 - 2ywx + 2wbx + w**2 * x**2 - 2by + b**2 ) ) / dw
             = SUM(i=1, i<=m, (-2yx + 2bx + 2w * x**2) )
             = 2( w * SUM(i=1, i<=m, x**2 ) - SUM(i=1, i<=m, (y - b) * x ) )
  ```
  * 对b进行求导
  ```
  dE(w,b)/db = dSUM(i=1, i<=m, (y**2 - 2ywx + 2wbx + w**2 * x**2 - 2by + b**2 ) ) / db
             = SUM(i=1, i<=m, (2wx - 2y + 2b) )
			 = SUM(i=1, i<=m, 2(b - y + wx ) )
			 = 2( SUM(i=1, i<=m, b) - SUM(i=1, i<=m, y) + SUM(i=1, i<=m, wx) )
			 = 2(mb - SUM(i=1, i<=m, (y - wx) ) )
  ```
  * 令dE(w,b)/db = 0, 得b最优解  
  ```
   0 = 2(mb - SUM(i=1, i<=m, (y - wx) ) )
  mb = SUM(i=1, i<=m, (y - wx) )
   b = SUM(i=1, i<=m, (y - wx) ) / m
   b = SUM(i=1, i<=m, y) / m - w * SUM(i=1, i<=m, x) / m
   # 由于 SUM(i=1, i<=m, y)/m = AVE(y), SUM(i=1, i<=m, x)/m = AVE(x)
   b = AVE(y) - w * AVE(x)
  ```
  * 令dE(w,b)/dw = 0, 得w最优解
  ```
														0 = 2( w * SUM(i=1, i<=m, x**2 ) - SUM(i=1, i<=m, (y - b) * x ) )
								w * SUM(i=1, i<=m, x**2 ) = SUM(i=1, i<=m, (y - b) * x )
								w * SUM(i=1, i<=m, x**2 ) = SUM(i=1, i<=m, yx) - SUM(i=1, i<=m, bx)
								# 代入 b = AVE(y) - w * AVE(x)
								w * SUM(i=1, i<=m, x**2 ) = SUM(i=1, i<=m, yx) - SUM(i=1, i<=m, ( AVE(y) - w * AVE(x) ) * x )
								w * SUM(i=1, i<=m, x**2 ) = SUM(i=1, i<=m, yx) - AVE(y) * SUM(i=1, i<=m, x) + w * AVE(x) * SUM(i=1, i<=m, x)
  w( SUM(i=1, i<=m, x**2 ) - AVE(x) * SUM(i=1, i<=m, x) ) = SUM(i=1, i<=m, yx) - AVE(y) * SUM(i=1, i<=m, x)
                                                        w = [ SUM(i=1, i<=m, yx) - AVE(y) * SUM(i=1, i<=m, x) ] / [ SUM(i=1, i<=m, x**2 ) - AVE(x) * SUM(i=1, i<=m, x) ]
								# 由于 AVE(y) * SUM(i=1, i<=m, x) = (1/m) * SUM(i=1, i<=m, y) * SUM(i=1, i<=m, x) = AVE(x) * SUM(i=1, i<=m, y)
								# 由于 AVE(x) * SUM(i=1, i<=m, x) = (1/m) * SUM(i=1, i<=m, x) * SUM(i=1, i<=m, x) = (1/m) * [SUM(i=1, i<=m, x) ** 2]
								                        w = SUM(i=1, i<=m, y (x - AVE(x)) ) / { SUM(i=1, i<=m, x**2 ) - (1/m) * [SUM(i=1, i<=m, x) ** 2] }
  ````
  * 对数线性回归
  ```
  ln(y) = wT * x + b
      y = e ** (wT * x + b)
  ```
# 3.3 对数机率回归
* ln(y/(1-y)) = wT * x + b
	* y = 1 : 
	* y = 0

# 3.4 线性判别分析
* 同类样例投影尽可能近
* 异类样例投影尽可能远

# 3.5 多分类学习
* 拆为多个二分类任务
* 一对一(One vs One, OvO)
	* N(N-1)/2个训练器
	* C1+C2,C1+C3,C1+C4,...
* 一对其余(One vs Rest, OvR)
	* N个训练器
	* C1+C2C3C4, C2+C1C3C4, C3+C1C2C3, C4+C1C2C3
* 纠错输出码
	* 常用的MvM一种
	* 两步：编码，解码
	* 二元码：正/负类
	* 三元码：正/负/停用类
	* 任意两类别之间编码距离越远，纠错能力越强
# 3.6 类别不平衡问题
* 实际思考：实际中很多corner case数据就很少(比如：车道线训练中，红色车道线/潮汐车道线)
* 指分类任务中，不同类别的训练样本数目差别很大
* 通常y = wT * x + b的中
	* y>0.5为正例
	* 否则为反例
* 学习策略：再缩放(rescaling)
```
	[ y/(1 - y) ] * m反例数/m正例数
```
* 对反类欠采样:降低反例数目，使之与正例数一样
* 对正类过采样
* 正常采样，然后使用再缩放策略