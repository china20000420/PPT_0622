# 事实核查 · In-Context Pretraining (ICLM)

> 论文: *In-Context Pretraining: Language Modeling Beyond Document Boundaries*
> 来源: Meta AI / FAIR + UW + AI2, ICLR 2024
> arXiv: https://arxiv.org/abs/2310.10638
> 代码: https://github.com/swj0419/in-context-pretraining

## 已从多个独立来源交叉验证 ✅

| 事实 | 验证来源 |
|------|---------|
| Contriever 做文档嵌入 | FAQ论文总结 + ar5iv HTML |
| FAISS IVF-PQ 做大规模ANN搜索 | 同上 |
| 文档图构建 (top-k邻居 → 无向图) | 多个来源一致 |
| 贪心图遍历 (最小度起点 → 最大相似度邻居) | 验证 |
| 建模为最大旅行商问题(Max-TSP) | 验证 |
| +8% In-Context Learning (8 datasets) | 多个来源一致 |
| +15% Reading Comprehension (6-8 datasets) | 验证 |
| +16% Faithfulness to Context | 验证 |
| +9% Retrieval Augmentation | 验证 |
| +5% Long-Context Reasoning | 验证 |
| 附带语义去重收益 | 验证 |
| 300B tokens, 7B model, CommonCrawl | 验证 |

## ⚠️ 教学构建（非论文原文）

- README.md 中的 **10文档语义排序示例**（d₁...d₁₀）是我构造的教学案例
- **1000文档示例** 是推算演示
- **伪代码** 逻辑基于论文描述，但格式非原文Algorithm 1逐字
- **图结构示意** 是我基于k=2构建的简化示例
- **Batch采样策略描述** 基于我理解的推断

## 📎 建议
- 核心数据和算法可引用
- 文档排序示例请标注"教学示例"
- 建议去原文看看 Figure 2 (文档图可视化) 和实际的实验表格
