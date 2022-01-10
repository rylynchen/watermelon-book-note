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
  * ROC : 受试者工作特征