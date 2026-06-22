# In-Context Pretraining (ICLM) 图表索引

> 论文: *In-Context Pretraining: Language Modeling Beyond Document Boundaries* (Meta, ICLR 2024)
> arXiv: https://arxiv.org/abs/2310.10638
> ar5iv HTML: https://ar5iv.labs.arxiv.org/html/2310.10638
> 所有图片已下载到 `figures/` 目录，来源为 ar5iv (arxiv HTML5 渲染)，与论文原文一致

## 图表清单

| 文件 | 论文编号 | 内容描述 | 建议放PPT |
|------|---------|---------|:--:|
| `intro.png` | **Figure 1** | ICLM 总览图: 标准预训练 vs ICLM (相关文档放同一上下文) | ✅ 必放 |
| `main.png` | **Figure 2** | ICLM 两步流程: (1) 文档图构建 (2) 图遍历生成输入上下文 | ✅ 必放 |
| `ppl.png` | **Figure 3** | 语言模型困惑度: ICLM vs 基线 (Wikipedia/Arxiv/Books, 多模型规模) | ✅ |
| `evolution.png` | Figure 4 | 训练过程中ICLM效果的演化 | 可选 |
| `k.png` | Figure 5 | k (最近邻数量) 的消融实验 | 可选 |

## PDF原文

已下载完整PDF: `paper.pdf` (1.0MB)

## 图Figure 1详解 (ICLM总览)

标准预训练: 随机文档 → 上下文窗口内文档主题无关
ICLM: 相关文档排序后 → 同一个上下文窗口内文档语义相关
例: "FIFA sets prize $42m" 的前文是 "World Cup never >$10M before 2022"
  → 模型可以推理出 "the highest so far"

## 图Figure 2详解 (两步流程)

Step 1 - 找相关文档 (Finding Related Documents):
  - Contriever编码文档为向量
  - FAISS大规模ANN检索找top-k邻居
  - 构建文档图 (节点=文档, 边=互为最近邻)
  
Step 2 - 图遍历 (Graph Traversal):
  - 贪婪算法近似最大旅行商问题
  - 从最小度数节点出发, 走到最相似未访问邻居
  - 路径切分为固定长度上下文窗口

## Table 1: ICL性能 (7个分类数据集, 32-shot)

ICLM在所有数据集上超越baseline, +8%平均

## Table 2: 阅读理解 (6个数据集, 2-shot)

ICLM在所有6个数据集上超越baseline, +15%平均
