# ③ In-Context Pretraining (ICLM) — Meta AI, ICLR 2024

> **论文**: *In-Context Pretraining: Language Modeling Beyond Document Boundaries*
> **作者**: Weijia Shi, Sewon Min, Maria Lomeli, Chunting Zhou, Margaret Li, Xi Victoria Lin, Noah A. Smith, Luke Zettlemoyer, Scott Yih, Mike Lewis
> **机构**: Meta AI / FAIR + University of Washington + Allen Institute for AI
> **会议**: ICLR 2024
> **地址**: https://arxiv.org/abs/2310.10638
> **代码**: https://github.com/swj0419/in-context-pretraining

---

## 1. 核心思路

Best-Fit Packing 消灭了不必要截断，但语义噪声仍存——同一训练序列中的文档是随机相遇的，跨文档注意力缺乏预测信号。

ICLM 的思路：让语义相关的文档出现在同一上下文窗口，跨文档注意力从噪声变为预测信号。仅修改预处理阶段的文档排序，模型架构和训练目标完全不变。

---

## 2. 算法流程

### Step 1 — 找相关文档（大规模语义检索）

**文档嵌入**：使用 Contriever（Izacard et al., 2022）将每篇文档编码为 768 维密集向量。

```
E(d) = (1/|d|) · Σ_{t=1}^{|d|} h_t^(last)   ∈ R^768
```

Contriever 基于无监督对比学习训练——同一文档的两个随机片段作为正样本对，不同文档的片段作为负样本对，用 InfoNCE 损失优化。不需要人工标注。

**相似度计算**：两文档的余弦相似度。

```
sim(d_i, d_j) = (E_i · E_j) / (‖E_i‖₂ · ‖E_j‖₂)
```

**最近邻搜索**：每文档取 top-k 最相似文档。

```
N(d_i) = top-k_{j≠i} sim(d_i, d_j),   k=10
```

**FAISS 加速**（Johnson et al., 2019）：IVF（Inverted File Index）将嵌入空间分割为 Voronoi 单元，搜索限制在最近单元。PQ（Product Quantization）将 768 维向量分段压缩为 8-bit 编码。235M 文档，50M/批次，8 GPU，约 6 小时完成。对比暴力搜索需数周。

### Step 2 — 图遍历组织上下文

**文档图构建**：节点 = 全部文档。边满足双向条件。

```
(d_i, d_j) ∈ L  ⇔  d_j ∈ N(d_i)  or  d_i ∈ N(d_j)
边权重: w(d_i, d_j) = sim(d_i, d_j)
```

双向条件保证图的连通性——避免"高流行度文档单向连接大多数文档，但文档间互不连通"的问题。

**贪心图遍历 — Max-TSP 近似**：

目标：在 G 上找覆盖每个文档恰好一次的哈密顿路径，最大化相邻文档相似度之和。

```
max_P  Σ_{t=1}^{N-1}  sim(d_{π_t}, d_{π_{t+1}})
```

这是 NP-Hard 的最大旅行商问题。N=235M，精确算法不可行。论文采用**双层贪心近似**：

**外层循环** — 选链段起点：每次从未访问节点中选**度数最小**的节点。度数低 = 相似文档少。优先处理避免最后孤立——最小度数优先启发式（Min-Degree-First）。

**内层循环** — 扩展链段：从当前节点出发，在未访问邻居中选**余弦相似度最高**的节点作为下一步。重复直到当前连通分量全部访问（死胡同）。返回外层选新起点。

**复杂度**：每条边在检查"是否未访问"时最多被访问一次。|L| ≈ N·k/2 ≈ 1.2B 条边。总操作 O(N·k) = O(N)，k=10 常数。20 CPU × 12h（235M 文档）。

**路径 → 上下文窗口**：沿路径累积文档 token 数达到 L 时切出窗口。同一窗口内文档语义相关。batch 内从路径不同区段取窗口保证多样性。

---

## 3. 关键设计决策

### k 值选择（论文消融）

| k | 效果 |
|---|------|
| 1 | 图太稀疏，大量孤立节点，路径频繁断裂 |
| 5 | 中等密度，仍有较多断裂 |
| **10** | **最优平衡：连通性好，语义多样性高** |
| 20 | 图太稠密，热门文档主导遍历 |

### 语义去重（附带收益）

检索阶段发现近似重复文档对产生极高余弦相似度（sim > 0.98）。这些文档在图中自然聚成密集子图，可在构建图过程中直接识别并移除。消融确认：不做语义去重导致 ICLM 效果显著退化。

### 计算开销

| 阶段 | 资源 | 耗时 |
|------|------|------|
| Contriever 编码 | ~ GPU | ~ 几百 GPU-h |
| FAISS 检索 | 32 GPU | ~ 6h |
| 图遍历 | 20 CPU | ~ 12h |
| 训练（7B） | 128 A100 | ~ 9d |

预处理占总训练成本 < 2%。

---

## 4. 实验结果（300B tokens CommonCrawl，7B，L=8192）

| 任务类别 | 提升 | 数据集/说明 |
|---------|------|-------------|
| In-Context Learning | +8% | 7 个分类数据集 (32-shot)，全部优于 Standard |
| 阅读理解 | +15% | SQuADv2, RACE, BoolQ, DROP, HotpotQA 等 6 ds（四种方法最高）|
| 忠实度 (Faithfulness) | +16% | NQ-Swap, MemoTrap |
| 检索增强 | +9% | Wikipedia 外部知识 QA |
| 长上下文推理 | +5% | 长上下文基准 |
| 困惑度 | 全部更低 | Wikipedia/Arxiv/Books, 0.3B–7B |

**扩展性**：数据 100M → 10B tokens，ICLM 优势 +4% → +11%，单调递增无饱和。

**消融**：k=10 最优；BM25 替代 Contriever → 效果下降（短文档和代码尤甚）；不去重 → 显著退化。

---

## 5. 方法定位

**回应随机拼接"缺陷二（噪声）"**：语义排序将跨文档注意力从噪声变信号。

**与 Best-Fit 正交**：Best-Fit 决定空间安排（哪些 chunk 放一起），ICLM 决定时间顺序（文档排列先后）。可组合——先装箱后排序。

**适合需跨文档推理的下游任务**：RAG、Few-shot ICL、多跳推理。局限：需 GPU 集群做检索；依赖 Contriever 嵌入质量；非英语语料可能需要重训嵌入模型。
