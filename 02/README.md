# 2.1 经验误差与过拟合
* 错误率
* 精度 accuracy = 1 - 错误率
* 过拟合 overfitting (学过了)
* 欠拟合 underfitting (未学好)
# 2.2 评估方法
* 使用 测试集 testing set
  * 真实分布采样
  * 与训练集互斥
* 留出法 hold-out
  * 数据集D, 训练集S, 测试集T
  * D = S 并 T
  * S 交 T = 空
  * S,T 数据分布一致性
  * S:T = 3:2/5:4 (通常 训练集:测试集 8:2)
* 交叉验证法 cross validation
  * D = D1 并 D2 并 ... Dk
  * D1 交 D2 交 ... Dk = 空
  * 选k-1个子集merge作training，剩下的一个做test
  * 获得k组结果，取ave
  * k通常10
* 留一法 Leave-One-Out
  * 特殊的交叉验证法，k=样本数量
  * 缺点，样本量大时，训练成本大
* 自助法 bootstrapping
  * 样本数量m
  * 随机挑选m次 生成训练集S (有重复样本)
  * D \ S => T
* 不同数据量选择策略
  * 数据量较小，自助法
  * 数据量足够，留出法和交叉验证法
* 调参 parameter tuning
  * 基于算力和性能, 范围+步长
  * 把训练数据分为，训练集／验证集 validation set

# 2.3 性能度量
* 分类任务
  * 错误率
  * 精度
* 泛化能力的评估
* 均方误差
  * SUM(i=1, i<=m, (f(xi)-yi) ** 2) / m
* 错误率：E(f;D) = COUNT(i=1, i<=m, f(xi)!=yi) / m
* 精度：Acc(f;D) = COUNT(i=1, i<=m, f(xi)==yi) / m = 1 - E(f;D)
* 四种情形
  * TP: True Positive, 真正类，将正类预测为正类的数量
  * TN: True Negative，真负类，将负类预测为负类的数量
  * FP: False Positive，假正类，将负类预测为正类的数量
  * FN: False Negaitve，假负类，将正类预测为负类的数量
  * TP + TN + FP + FN = 样本总数
* 查准率,查全率
  * 查准率 precision = TP / TP + FP 
  * 查全率 recall = TP / TP + FN
  * 互相矛盾
  * PR曲线
    * (面积越大，性能约优)
    * 度量值，平衡点BEP，P=R时
    * F1 = (1 + b**2) * P * R / (b**2 * P + R)
      * b=1，标准
      * b>1，查全率影响更大（逃犯）
      * b<1，查准率影响更大（商品推荐）
  * 多个二分类
    * 多头任务
    * 2种方法
      * 先ave.P/R, 再F1
      * 先ave.TP/FP/TN/FN, 再F1
* ROC, AUC
  * ROC曲线
    * 受试者工作特征
    * x: 假正例例率，y:真正例率, (0, 1) 理想模型
    * 对角线，随机猜测
    * 左上越好，右下越差
  * AUC
    * ROC的量化指标
    * ROC曲线面积
    * 面积越大，算法越好
* 错误代价
  * 不同错误的代价不同 (权重)
  * 最小化错误次数(均等代价) => 最小化总体代价(非均等代价)
  * 二分类代价矩阵
    * cost(i,i) = cost(j,j) = 0
    * cost(i, j), cost(j, i) 非零
  * E(f;D;cost) = ( COUNT(i in D+, f(xi)!= yi) * cost(0,1) + COUNT(i in D-, f(xi)!= yi) * cost(1,0) ) / m
  * 代价曲线
    * 非均等代价下，ROC曲线不能反映总体期望代价
    * P(+)cost = p * cost(0,1) / (p * cost(0, 1) + (1 - p) * cost(1, 0) )

# 2.4 比较检验
* 疑问
  * 比较的是 => 泛化性能
  * 实验评估的是测试集上的性能
  * 不同/相同 测试集实验结果可能都不同
  * 测试集上 A比B好，A的泛化是否比B好？
* 假设检验
  * 根据测试错误率 估推出 泛化错误率
* 交叉验证t检验
  * 基于k折交叉验证
  * 两个算法A和B
* McNemar检验
  * 留出法
  * 两个算法A和B

# 2.5 偏差与方差
* 解释 算法泛化性能的 一种工具
* 偏差-方差窘境
* 拟合能力弱时，偏差主导错误率
* 拟合能力弱时，方差主动错误率