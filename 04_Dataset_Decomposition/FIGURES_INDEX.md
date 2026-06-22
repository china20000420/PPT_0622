# Dataset Decomposition (DD) 图表索引

> 论文: *Dataset Decomposition: Faster LLM Training with Variable Sequence Length Curriculum* (Apple, NeurIPS 2024)
> arXiv: https://arxiv.org/abs/2405.13226
> ar5iv HTML: https://ar5iv.labs.arxiv.org/html/2405.13226
> 所有图片已下载到 `figures/` 目录，来源为 ar5iv (arxiv HTML5 渲染)，与论文原文一致

## 图表清单

| 文件 | 论文编号 (推测) | 内容描述 | 建议放PPT |
|------|---------|---------|:--:|
| `x1.png` | Figure 1 | Concat-and-Chunk vs DD 对比示意图 | ✅ 必放 |
| `x2.png` | Figure 2 | 二进制分解 (Binary Decomposition) 示意 | ✅ 必放 |
| `x4.png` | Figure 3 | 训练效率曲线: Loss vs Training Tokens (DD 6× faster) | ✅ 必放 |
| `x5.png` | Figure 4 | VSL 训练课程权重/长度分布 | ✅ |
| `x7.png` | Figure 5 | 消融实验: 不同课程策略对比 (Grow-P2最优) | ✅ |
| `length_accuracy.png` | Figure | 序列长度 vs 准确率的trade-off分析 | ✅ |
| `evals_distributions.png` | Figure | 评估数据集上的序列长度分布 | 可选 |
| `x6.png` | Figure | 长上下文benchmark结果 | ✅ |
| `x3.png` | 公式 | 二进制分解公式 (LaTeX渲染) | 可选 |
| `x8.png` | Figure | 桶内token分布统计 | 可选 |
| `x9-x16.png` | 多种图表 | 补充实验结果/消融/扩展性分析 | 可选 |

## PDF原文

已下载完整PDF: `paper.pdf` (0.6MB)

## 核心图表详解

### Figure 1: Concat-and-Chunk vs DD

- 左侧: 传统concat-and-chunk → 跨文档注意力浪费 + 短文档碎片化
- 右侧: DD → 文档分解为2的幂次序列 → 各自入桶 → VSL训练

### Figure 2: 二进制分解

```
Document of length l = Σ 2^{i_k}
例如: l=2200 = 2048 + 128 + 16 + 8
                  D11    D7    D4   D3
```

### Figure 3/4: 训练效率

- X轴: Training tokens (steps × tokens-per-step)
- Y轴: Validation loss
- DD曲线比Concat-and-Chunk更快下降
- 相同loss, DD用1/6的tokens → 6× 加速

### Figure 5: 消融实验

| 配置 | 效果 |
|------|------|
| Baseline (Concat-and-Chunk) | 最差 |
| DD + Random | 略好 |
| DD + Linear | 中等 |
| DD + Grow-P2 | 最佳 |
