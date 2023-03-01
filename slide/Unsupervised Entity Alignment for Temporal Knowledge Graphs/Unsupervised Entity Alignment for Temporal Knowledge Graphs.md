# Unsupervised Entity Alignment for Temporal Knowledge Graphs

## 1、综述：时态知识图谱的补全方法

### 1.1、基于符号逻辑的方法

特点：基于符号逻辑的方法可以从已有的知识图谱出发，结合本体中的规则，推理出新的实体间关系；同时，还可以对演化后的知识图谱进行逻辑一致性检查，使得推理结果具备透明、可靠及可解释性强等特点。为了表达时态的知识，这一类方法通常需要引入时态算子来提升本体的表达能力，而表达能力的提升通常会导致如下两种局限性。

#### 1.1.1、以领域为中心的方法

在查询语言的表达能力上要强于以时间为中心的方法

#### 1.1.2、以时间为中心的方法

以时间为中心的方法在本体的表达能力上要强于以领域为中心的方法

### 1.2、基于知识表示学习的方法

特点：基于知识表示学习的方法将研究对象的语义信息表示为低维稠密的实值向量。在低维向量空间中能够高效地计算实体和关系的语义关系，显著地提高推理性能。但是，此类方法的推理过程不透明，推理结果的可解释性低。

第一类是基于平移距离模型的方法

第二类是基于矩阵分解模型的方法

第三类是基于神经网络模型的方法

## 2、目前研究

现有的大部分编码器：

1.基于KGE的编码器：EntityAlignment编码器。使用KG嵌入模型（例如TransE）学习实体嵌入。

2.基于GNN的编码器：基于GNN的模型展示了其卓越的性能。这是由于GNN具有强大的建模能力，能够利用各向异性散射机制捕获图形结构。然而，它们无法对TKG中的时间信息进行建模。

### 2.2、需要解决的问题

TKG的关系三元组中的时间信息是自然对齐的

1）“日本”和种子位于不同的连接组件中，其中GNN不可能将对齐信息传播到“日本”的嵌入中；

使用无学习编码器对时间间隔进行建模，而不是简单地将时间间隔视为关系。具体来说，我们从编码器中提取伪种子来训练基于GNN的EA模型。

2）“日本”与种子不共享共同的时间戳和关系类型，因此，种子信息无法通过共享的时间或关系嵌入到达“日本”。

## 3、创新

开发了独立于种子对齐的编码器。这是为了在将项目和关系信息编码为特征向量后将其融合。

（i）使用新型无标签编码器DualEncoder将时间和关系信息分别编码到嵌入中；

（ii）融合这两种信息并将其转换为对齐，使用一种新型的基于图匹配的解码器GM-decoder，该解码器能够在有或无监督的情况下对TKG执行EA，因为它具有有效捕获时间信息的能力。

1.编码时间信息：一些实体可能不包含足够的时间信息，这会导致较差的对齐结果。为了解决这个问题，我们通过聚集邻居的信息，从他们的邻居中推断出时间特征，这可能与足够的时间事实有联系

2.编码关系信息：使用GNN来传播种子对齐提供的信息

3.融合信息和解码：

第一𝑯𝑟是一个密集矩阵，而𝑯𝑡实际上是稀疏的。由于存储稀疏矩阵的常用格式，将一个稀疏矩阵与一个密集矩阵连接起来需要消耗内存。第二𝑯𝑟从关系编码器获取𝑯𝑡从Temporal Encoder获得。因此，不需要对它们进行同等考虑。为了解决上述两个问题，我们引入了一个权重𝛼:𝑷=𝛼·𝑯𝑡𝑠(𝑯𝑡𝑡)𝑇+𝑯𝑟𝑠(𝑯𝑟𝑡)𝑇, 哪里𝛼是平衡两个特征对对齐结果的影响的权重。

## 参考文献

[时态知识图谱补全的方法及其进展](http://www.infocomm-journal.com/bdr/article/2021/2096-0271/2096-0271-7-3-00030.shtml)