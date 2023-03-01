
# 1、数据库
## 1.1、GRAILQA
### 1.1.1、介绍
GrailQA是一个新的大规模、高质量的KBQA数据集，其==知识库是Freebase==。它主要用来评估KBQA模型的泛化能力，支持三个泛化级别的评估，文章贡献：
1.迄今最大最全面的KBQA评估数据集（2021.4），该数据集以FREEBASE为基础（最大的公开知识库之一，包含86个域、2038个类、6265个关系和超过4500万个实体）。
2.基于novel bert的KBQA模型，对于复杂的问题，如果以生硬的方式列举候选逻辑形式，搜索空间仍然很大。
3.更智能引导的搜索是一个有前途的未来方向，它可以有效地生成最有前途的候选人，并删除不太有前途的候选人:
 i.i.d setting（独立同分布）: training distribution over questions is the same as the test distribution.
> 大多数现有的研究(隐含地)集中在i.i.d .设置上，也就是说，==假设训练分布代表真实的用户分布==，测试时的问题将从相同的分布中抽取。虽然这是机器学习的标准，但对于大规模知识库的KBQA来说，这可能是个问题。
> 首先，由于==广泛的覆盖面和组合爆炸，很难收集足够的训练数据来涵盖用户可能提出的所有问题。==
> 第二，即使努力做到这一点，例如，通过迭代地将所有用户问题注释到已部署的KBQA系统，也可能损害用户体验，因为在==每次迭代中，系统会在未被现有训练数据覆盖的新的非分布问题上失败。==

composition setting: generalize to novel compositions of the seen constructs.

zero-shot setting: model should also handle novel schema items or even entire domains not covered by the limited training data, which also includes compositions of novel constructs.
![请添加图片描述](https://img-blog.csdnimg.cn/0024f3f9ce4440b58c58b19690f1089d.png)

参考文章：
[Beyond I.I.D.: Three Levels of Generalization for Question Answering on Knowledge Bases](https://arxiv.org/abs/2011.07743v3)
(1)对遵循训练分布的问题进行归纳
(2)对训练中看到的模式项目的新组合进行组合概括(标记为蓝色)
(3)对看不见的模式项目甚至域进行零样本概括(标记为红色)。
### 1.1.2、 GrailQA数据集中问题的详细格式
![请添加图片描述](https://img-blog.csdnimg.cn/5f1034076c7145e1a84eb729ff6b7807.png)
### 1.1.3、 数据统计
![请添加图片描述](https://img-blog.csdnimg.cn/9c767d272f0f4df5bdea831b399dd655.png)

参考链接：[KBQA 常用的问答数据集之 GrailQA](https://blog.csdn.net/lft_happiness/article/details/123051686)
## 1.2、WEBQSP
参考链接：[KBQA 常用的问答数据集之 WebQSP](https://blog.csdn.net/lft_happiness/article/details/123049158)
  WebQSP(WebQuestionsSP) 目标==知识库是Freebase==。该数据集可用在Question Answering，Semantic Parsing和Entity Linking任务。创建并共享了迄今为止最大的语义解析标签数据集，以推进问题回答的研究(2016.8),为4,737个问题提供SPARQL查询。其余18.5%的原始webquestions被标记为“不可回答的”。这是由于许多因素，包括使用更严格的“可回答的”评估，即通过SPARQL而不是通过从文本描述中返回或提取信息来回答问题。
![请添加图片描述](https://img-blog.csdnimg.cn/b4dcef90bab347dc85ebe8deccf63f58.png)
参考文章：[The Value of Semantic Parse Labeling forKnowledge Base Question Answering](https://aclanthology.org/P16-2033.pdf)
### 1.2.1、具体方法
1.UI首先使用实体链接系统显示问题中检测到的实体(Yangand Chang, 2015)，并要求用户选择问题中的一个实体作为可能导致答案的主题实体。
2.如果实体链接系统返回的候选实体没有一个是正确的，用户也可以建议新的实体。一旦选择了实体，系统就会请求用户选择Freebase谓词来表示答案和这个主题实体之间的关系。
3.最后，添加额外的过滤器来进一步约束答案。我们的UI设计的一个关键优势是注释器在每个阶段只需要关注一个特定的子任务。标签器所做的所有选择都被用来自动构建一个连贯的语义解析。
4.评估是基于Freebase的，这比之前工作中使用的知识库要大得多
## 1.3 Freebase
参考链接：[Freebase 的基本概念](https://zhuanlan.zhihu.com/p/354058339)
### 1.3.1、基础介绍
Freebase 的数据被存储在图数据结构中。一个图由边连接的结点组成。在 Freebase 中，结点使用 /type/object 定义，边使用 /type/link 定义。通过以图的形式存储数据，Freebase 可以快速遍历主题（topic）之间的任意连接，并轻松添加新的模式（schema），而无需改变数据的结构。
Freebase 有超过 3900 万个关于真实世界的实体，例如人、地点和事物。由于 Freebase 的数据由图表示，这些主题对应图中的结点。然而，不是每个结点都是主题。CVT 就是这样一个例子，它不是主题但是结点。
![请添加图片描述](https://img-blog.csdnimg.cn/6b2864192fa14b22a905eaa0e48b4324.png)

![请添加图片描述](https://img-blog.csdnimg.cn/8bf510e75c844f1daf23a61ab803e868.png)
![请添加图片描述](https://img-blog.csdnimg.cn/ab9d11dbf33a412ea40132b2094cce2a.png)
# 2、RNG-KBQA: Generation Augmented Iterative Ranking forKnowledge Base Question Answering
## 2.1、GRAILQA缺点
基于生成的方法(例如，==seq-to-seq解析器==不足以有效地处理这种实际的泛化场景，因为生成不可见的KB模式项非常困难。
基于排名的方法，它首先使用==预定义的规则生成==一组候选逻辑表单，然后根据问题得分最高的一个已经显示出了一些成功(Gu等人，2021年)。但是，它存在覆盖问题，因为由于知识库的规模，用尽所有规则来覆盖所需的逻辑形式通常是不切实际的
## 2.2、做法
文章任务：任务是获得可以在知识库中执行的逻辑形式，以产生最终答案
### 2.2.1、先排序，产生排行榜
使用一个排序器从通过搜索图获得的候选逻辑表单池中选择一组相关的逻辑表单。所选的逻辑形式==不需要包含正确的逻辑形式==，但语义上是一致的，并与问题的潜在意图一致。
具体代码操作：排序器是一个基于bert的(Devlin等人，2019年)双编码器(将输入问题-候选对象对)，受训以最大化基本真理逻辑形式的分数，同时最小化不正确候选对象的分数。这样的训练模式允许从整个区域内候选人之间的对比中学习
**自举对否定逻辑形式进行抽样。也就是说，先用随机样本训练秩器数个周期来温暖启动它，然后选择对模型产生混淆的伪逻辑形式作为负样本进一步训练模型。发现，与使用随机负样本相比，这种先进的负抽样策略可以使排名器获得更好的性能。**
![请添加图片描述](https://img-blog.csdnimg.cn/edb53793393f42c9ad07ef553f81d31b.png)
### 2.2.2、生成器：补充缺失的结构或约束进一步优化首选候选项
引入一个生成器，它将消耗问题和排名前k的候选者，以组成最终的逻辑表单。我们的方法的核心思想是排名器和生成器之间的相互作用:排名器向生成器提供KB模式项的基本成分，生成器通过补充缺失的结构或约束进一步优化首选候选项，从而允许覆盖更广泛的逻辑表单空间。
![请添加图片描述](https://img-blog.csdnimg.cn/8749001b716c4e82bbb2b919e35a961b.png)
生成模型以排序器返回的问题和排名靠前的候选人为条件

具体代码操作：生成器是一个基于t5 (rafel et al.，2020)的序列到序列（seq-to-seq）模型，它融合了在前k个候选对象中发现的语义和结构成分，以组成最终的逻辑形式。为了实现这一点，我们向生成器==输入问题，然后是前k个候选问题的线性序列==，这使得它可以提炼出一种精致的逻辑形式，通过==补充缺失的部分或丢弃不相关的部分，而不需要学习底层动态，有意地反映问题的意图==。
通过连接问题和由分号分隔的排序器返回的前k个候选项(即[x;ct1;…;ctk])来构造输入。**利用教师强迫的方法训练该模型生成具有交叉熵目标的自回归逻辑形式。在推理中，使用波束搜索来解码k个目标逻辑形式。来构造前k个逻辑表单候选者**
[Teacher forcing是什么？](https://www.cnblogs.com/dangui/p/14690919.html)
> 生成器需要专注于纠正或补充现有的逻辑形式，而不是学习逻辑形式的生成规则和正确性
> 执行增强推理我们使用无语法约束的，T5生成模型，它不能保证生成的逻辑表单的语法正确性和可执行性。因此，使用了执行增强差分过程，使用束搜索解码top-k个逻辑表单，然后==执行每个逻辑表单，直到找到一个产生有效(非空)答案的表单。如果前k个逻辑表单中没有一个是有效的，将返回使用该排序器获得的排名最高的候选表单作为最终的逻辑表单，保证该逻辑表单是可执行的==。

**需要生成器的原因**：排序器基于自己的算法（在知识库中查询两跳内可到达的路径。接下来，我们写出对应于每个路径的ans-表达式）没有囊括所有的生成选项，需要生成器的补充

### 2.2.3、实体关系消歧
![请添加图片描述](https://img-blog.csdnimg.cn/f01be4cf05d0498880456351633bf719.png)
运行实体分解为排序的示例。混淆的实体(红色)和正确的实体(绿色)都符合问题的表面形式。为了区分它们，训练了一个实体消歧模型，该模型遵循与逻辑形式排序相同的架构，但通过连接问题和关系来构造输入。
# 3、代码
## 3.1、ENTITY DETECTION
实体检测，每个提及最多10个实体，这将在下一步中进一步消除歧义消除实体歧义(实体链接)，提供了预训练秩模型:sh scripts/run_disamb.sh predict
这将把预测结果(以每次提到的选定实体索引的形式)写入misc/grail_entity_link.json
## 3.2、ENTITY DISAMBIGUATING
消除实体歧义(实体链接)，提供了经过预先训练的排名者模型，run_disamb.sh
## 3.3、ENUM CANDIDATE
列举逻辑表格候选人
把枚举的候选人写入outputs/grail_candidate_ranking.jsonline。
## 3.4、RUN RANKER
这将把预测候选日志(每个示例中每个候选的日志)写入misc/grail_candidates_logits.bin，
并将预测结果(以原始GrailQA预测格式)写入misc/grail_ranker_results.txt
通过python grail_evaluate.py 来评估排名结果
## 3.5、RUN GENERATOR
运行生成器,读取候选项并使用日志来选择前k个候选项，并将生成模型输入写入output/grail_gen.json,
生成top-k解码的逻辑表单，存储在misc/grail_ _topk_generators .json中
## 3.6、FINAL INFERENCE
最后推论步骤
有了解码的前k个预测，将沿着前k个列表，一个一个地执行逻辑表单，直到我们找到一个返回有效答案的逻辑表单。
预测结果(原始GrailQA预测格式)到misc/grail__final_results.txt。
然后可以使用官方的GrailQA评估脚本来运行评估
