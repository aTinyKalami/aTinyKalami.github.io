
# DeepSeek V3.2 及核心技术DSA（DeepSeek Sparse Attention，细粒度稀疏注意力机制）

//【时间】20251008

//【地点】马来西亚，吉隆坡

//【事件】学习DeepSeek模型相关内容的笔记

//【分类】AI学习

------------------------------------------------------------------------------------------------------------------------------

## DeepSeek V3.2-Exp (推出时间：2025年9月29日）

### 1. 原始代码与论文
1.[**DeepSeek V3.2官方API文档介绍 on DeepSeek**](https://api-docs.deepseek.com/news/news250929)；

2.[**DeepSeek V3.2官方代码仓 on Github**](https://github.com/deepseek-ai/DeepSeek-V3.2-Exp)，含完整PDF论文。

3.[**DeepSeek V3.2官方论文 on Github**](https://github.com/deepseek-ai/DeepSeek-V3.2-Exp/blob/main/DeepSeek_V3_2.pdf);

4.[**DeepSeek V3.2 介绍 on Huggingface**](https://huggingface.co/deepseek-ai/DeepSeek-V3.2-Exp);

### 2. 关键技术（DSA）

DeepSeek-V3.2-Exp是一个实验性（Experimental）的版本，是DeepSeek模型迈向新一代架构的中间步骤。

DeepSeek-V3.2-Exp在V3.1-Terminus的基础上引入了 **DSA：DeepSeek Sparse Attention (DeepSeek 细粒度稀疏注意力 机制， fine-grained sparse attention)**

#### 2.1 方案：细粒度稀疏注意力机制(DSA)

【DSA原理图】

![DSA原理图](https://pica.zhimg.com/v2-95639b472024f757b50062f4bd8d51de_1440w.jpg)

DSA 的设计包括两个关键组件：

**组件1.Lightning Indexer**

这是一个高效的索引计算模块。对于每个查询token hth_tht，它会与之前的所有历史token H(s) 计算索引分数 I(t,s)，用于判断哪些历史token 需要被关注。

Lightning Indexer 支持 FP8 实现，进一步提升计算效率。

**组件2.细粒度Top-k选择机制**

对于每个查询 token，仅选择 index 分数排名前 k 的 key-value 对参与注意力计算。这样显著减少了参与注意力的 token 数量，将复杂度从 O(L²）降低到O(Lk)，其中 k≪L。L为历史查询的Token的总长度，k为选择靠前排名的数量。

其余技术创新、训练实现等详细内容可参见：[**分析文章**](https://zhuanlan.zhihu.com/p/1956280388277245345)。

#### 2.2 效果：推理价格大幅下降

在几乎不影响模型输出效果的前提下，DSA实现了长文本训练和推理效率的大幅提升。在各领域的公开评测集上，DeepSeek-V3.2-Exp 的表现与 V3.1-Terminus 基本持平。

【DeepSeek-V3.2与V3.1推理成本对比图】

![DeepSeek-V3.2与V3.1推理成本对比图](https://api-docs.deepseek.com/zh-cn/img/v3_2_cost_compare.webp)

**复杂度降低**：主注意力由O(L²)降为O(Lk)。

**Lightning Indexer** :仍为O(L²)，但因其头数少、FP8 实现，计算量远小于原主注意力。

**实测结果（H800 GPU 集群）**：在 128K 上下文中，V3.2-Exp 的推理 token 成本显著低于 V3.1，prefill 和 decode 阶段均有明显收益。

V3.2的效率大幅提升对应带来调价，模型Token API调用的成本降低：

![DeepSeek-V3.2价格变化图](https://api-docs.deepseek.com/zh-cn/img/v3_2_price_zh.webp)

-------------------------------------------------------------------------------------------------------------------------------------------------

（本文完）




