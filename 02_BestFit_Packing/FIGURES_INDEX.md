# Best-Fit Packing 图表索引

> 论文: *Fewer Truncations Improve Language Modeling* (Amazon, ICML 2024)
> arXiv: https://arxiv.org/abs/2404.10830
> ar5iv HTML: https://ar5iv.labs.arxiv.org/html/2404.10830
> 所有图片已下载到 `figures/` 目录，来源为 ar5iv (arxiv HTML5 渲染)，与论文原文一致

## 图表清单

| 文件 | 论文编号 | 内容描述 | 建议放PPT |
|------|---------|---------|:--:|
| `x1.png` | **Figure 1** | Best-fit Packing 两阶段流程总览图 (对比拼接基线) | ✅ 必放 |
| `concat_ex1.png` | Figure 2(a) | Undefined Names 示例 (代码截断导致变量未定义) | ✅ |
| `concat_ex2.png` | Figure 2(b) | Ungrounded Content 示例 (NL截断导致无依据生成) | ✅ |
| `concat_ex3.png` | Figure 2(c) | Missing Knowledge 示例 (知识因截断丢失) | ✅ |
| `toy_process_final_trimmed.png` | **Figure 3** | 截断损失分析: token位置m与期望loss增量 (理论分析) | 可选 |
| `truncation_nl_count.png` | Figure 4 | 自然语言数据: 文档截断率统计 | ✅ |
| `truncation_pl_count_2k_only.png` | Figure 5 | 代码数据: 文档截断率统计 (2k context) | 可选 |
| `x2.png` | 公式 | BFD装箱算法相关的数学公式 | 可选 |
| `x3.png` | 公式 | 理论分析公式 | 可选 |
| `x4.png` | 公式 | 相关公式 | 可选 |
| `x5.png` | 公式 | 相关公式 | 可选 |

## PDF原文

已下载完整PDF: `paper.pdf` (2.4MB)

## 图Figure 1详解 (Best-Fit Packing两阶段流程)

```
Top: 原始文档 (5个文档, 长度14/7/5/2/3 tokens)
Bottom-left: Best-fit Packing
  Step 1 (Segmentation): 长文档(blue, len>8)被切分为 ≤8 tokens的chunk
  Step 2 (Packing): chunks被BFD算法装入容量=8的序列中
  结果: 仅1个文档被截断 (必要截断, 因原始长度>L)
Bottom-right: Concatenation基线
  结果: 5个文档中3个被截断 (60%)
```

## 图Figure 2详解 (截断的三个危害)

- (a) Undefined Names: Python代码中 `DecoderModel` 和 `config` 的定义与使用被切到不同序列 → 模型学到病态模式
- (b) Ungrounded Content: `Monday morning` 的上下文在另一个序列 → 生成内容无依据
- (c) Missing Knowledge: ICML的location和conference name在不同序列 → 模型永远学不到完整事实
