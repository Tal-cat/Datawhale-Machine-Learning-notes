# Datawhale-Machine-Learning-notes    
2024.5 

![1359141715867507_ pic](https://github.com/Tal-cat/Datawhale-Machine-Learning-notes/assets/60603537/b526c28c-8345-4241-9c8e-f58f97bf5c6b)

第一章 绪论    
1. 主要是概念的各种介绍，大致清楚，以下值得记录。
2. 经典定义：利用经验改善系统自身的性能。【1997】
3. 目前主要研究：智能数据分析的理论和方法。   
4. 没有免费的午餐定理（No Free Lunch Theorem, NFL）：在f均匀分布时，任何2个算法的期望都相同。
5. 但是！并不意味着选择算法无用，因为在特定情境（数据）下，f并非均匀分布，因此机器学习有一番用武之地。
6. 发展历史：推理期→知识期→学习期【符号主义（表示能力强导致复杂，成也萧何败萧何，程野不是唱二人转那个么？）→连接主义→统计学习→连接主义（算力提升）】。

第二章 模型评估与选择   
1. 基础评估：accuracy, specificity, recall/precision, F1-score (Fβ-score), PR, ROC, AUC-ROC。曲线完全包裹代表性能更好。    
2. 模型训练方法：（1）留出法hold-out：最简单。（2）Cross-validation: 常用，5-fold, 10-fold。特殊一点是留一法。（3）自助法Bootstraping:感觉强化学习用的多些，放回的抽样，可能改变数据分布。
3. 调参及训练过程：其实在书上画了个图。data分成training set和test set, training set分成实际training set和validation set。用实际training set调参，根据validation set表现确定hyperparameters。然后才是真正的训练：用大的training set训练然后test set评估。
4. ROC曲线画法：将threshold分别设置为每个样本的数值，然后计算画图。
5. 代价曲线cost curve:多个线段交集下的面积，下次画一个。
6. 比较检验：假设检验，交叉验证t检验，McNemar检验，Friedman检验，Nemenyi后续检验。其中Friedman检验的图很实用，下次画一个。
7.泛化误差：偏差+方差+噪声。偏差反映拟合能力，方差反应数据变化的影响。噪声反应问题难度。    
8. 偏差-方差窘境。

第三章 线性模型    
3,2 线性回归【数学部分南瓜书很详细，不赘述】     
1. 针对离线属性，如果存在“序”，可转化为连续值【如大中小对应1,0.5,0】；不存在“序”时转换为k维向量，避免将无序属性连续化，引入序【如篮球足球对应（1,0）和（0,1）】。
2. 可解释性好，w大小代表贡献度。   
3. 使用最小二乘法最小化MSE，即：找到一条直线，使得样本到直线的距离最小。
4. 求解最小参数argmin的过程称为参数估计。
5. 多元线性回归时，满秩有唯一解，不满秩时有多个解，就像3个x只有2个公式一样，此时根据偏好（如需要某个数最小）或加入正则化限制，以获取答案。   
6. 广义线性模型：y=g^(-1)(w^Tx+b)。期中g(.)称之为联系函数。

3.3 对数几率回归（大名鼎鼎的Logistic Regression/逻辑回归【被批判，不是】）    
1. 用于分类任务，将不连续且不可微的[0,1]二分类转化为单调+任意阶可导的函数。
2. 属于Sigmoid函数的一种。
3. 命名由来：ln(y/1-y)=e^(w^Tx+b)，其中y/1-y反应相对可能性，称为几率（odds）。log+odds→取名logit→形容词化logistic→被【错误翻译成了逻辑啊啊啊啊】。
4. 优点：（1）；无需实现预测数据分布（2）可得到近似概率；（3）现有许多optimization方法。    
5. MLE思想：max(P(真是+)P(预测是+)+P(真是-)P(预测是-))。
6. LR的MLE高阶连续可导，但是不用MSE，原因有二：（1）多元时MSE计算，w=(x^Tx)^(-1)x^Ty，(x^Tx)^(-1)不一定存在；（2）GD为迭代算法，方便计算机实现。
7. MLE求解使用GD（一阶导数）或者Newton Method（二阶导数）。注意GD常用，Newton计算麻烦且不一定都能用二阶。

3.4 线性判别分析（LDA）    
1. 线性方法，不严格时也叫做”Fisher“判别分析。但是严格来说，LDA假设了各类样本的协方差矩阵相同且满秩。
2. 思想：给定训练集，投影到直线，同类聚集，不同类远离。新样本投影后根据位置进行分类。属于监督（需要label）降维技术。
3. 计算：不同类距离最大，同类距离最小。综合上就是：最大化广义瑞利商【不同类距离/同类距离】。
4. w=S_w^(-1)(u_0-u_1)。计算是通常对S_w进行奇异值分解，S_w=UsigmaV^T, S_w^(-1)=Vsigma^(-1)U^T。    
5. 具体计算挺好玩，笔记在书上，建议参考周志华老师课程。 
6. 多分类LDA和二分类类似。W的闭式解S_w^(-1)S_b的d'个非零广义特征值所对应的特征向量组成的矩阵，d‘<=N-1。

第四章 决策树（Decision Tree）   
4.1 基本流程    
1. 策略为divide and conquer，分治法。
2. 过程为递归，以下3种情况导致递归返回：（1）当前节点的样本属于同一类别（不用再分了）；（2）当前属性空了，或者里面样本属性取值相同（没发分了），使用比例最多的类别；（3）样本空了（比如没有白色西瓜），使用父节点的分布。
     
4.2 划分选择
1. 使用信息增益（ID3）/增益率（C4.5）/基尼指数（CART），性能差不多，有一个用的就行。    
2. 信息熵Entrophy越小，纯度越高。表示还需要多少信息可以划分出来。    
3. 信息增益对可取值数目多的属性有偏好，喜欢分的干净（如编号、电话号）。
4. 增益率对可取值数目少的属性有偏好。
5. C4.5采用启发式方法，先选出信息增益高于平均的属性，在从中选择增益率最高的。
6. 基尼指数反映了从数据集抽2个样本，类别不一样的概率，越小越好。
       
4.3 剪枝pruning
1. 很重要，防止overfitting，性能提升的保障。
2. 预剪枝：边算边决定是否分支，训练时间↓，测试时间↓。过拟合风险↓，欠拟合风险↑。
3. 后剪枝：先完整树，再剪。训练时间↑，测试时间↓。过拟合风险↓，欠拟合风险基本不变。
      
4.4 连续与缺失值
1. 连续属性划分后，后面的节点还可以接着用，分类变量则不行。
2. 基本思路：样本赋权，权重划分。
3. 解决2个问题：（1）如何在属性值缺失时划分属性选择；（2）给定划分属性，如样本缺失该属性值，如何划分。
4. 针对（1）用没有缺失值的样本算。针对（2）使用比例，同时进入多个分支。     
4.5 多变量决策树：根据对属性的线性组合划分。

第五章 神经网络    
1. 定义：神经网络是有具有适应性的简单单元组成的广泛并行连接的网络。课程中指机器学习和神经网络2学科的交叉部分。
2. 神经元模型：突触。
3. 感知机（perceptron）只有输入层和输出层，只有输出层一层使用激活函数处理，只有一层功能神经元。
4. 感知机对于线性可分问题才会收敛，否则会震荡。
5. 多层前馈神经网络（Multi-layer feedforward neural network）：每层神经元与下一层全互联，神经元之间不存在同层连接，也不存在跨层连接。其中隐层和输出层为功能神经元。
6. 误差逆传播（error BackPropagation）：计算见书和视频，很清晰，使用链式法则（chain rule）。
7. 万有逼近性（Universal approximation）：只需一个包含足够多神经元的hidden layer，多层前馈神经网络就能以任意精度逼近任意复杂度的连续函数。原因：因为黑箱，存疑，故在数学上推到此条作为基础。
8. 避免over-fitting：（1）early stopping：1）若训练误差连续a轮小于b，则停止训练；2）若训练集误差降低但验证集误差升高，则停止训练，同时返回具有最小验证集误差的连接权和阈值。（2）正则化regularization：误差函数中增加一个描述网络复杂度的函数。这样会偏好比较小的连接权，使得输出比较光滑。
9. 跳出局部最小的方法（启发式，缺乏理论保障）：（1）以多组不用参数初始化NN，取误差最小的做最终参数；（2）模拟退火：每一步都以一定概率接受比当前解最差的结果，接受“次优解”的概略随时间减低，从而保证算法稳定（可能也会跳出全局最小）；（3）SGD。     

