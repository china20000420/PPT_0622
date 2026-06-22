# 事实核查 · Dataset Decomposition (DD)

> 论文: *Dataset Decomposition: Faster LLM Training with Variable Sequence Length Curriculum*
> 来源: Apple + Anthropic, NeurIPS 2024
> arXiv: https://arxiv.org/abs/2405.13226
> 代码: https://github.com/apple/ml-dataset-decomposition

## 已从多个独立来源交叉验证 ✅

| 事实 | 验证来源 |
|------|---------|
| 二进制分解 (l = Σ 2^{i_k}) | GitHub README + 多个论文摘要 |
| 桶分配 (2的幂次: 256, 512, ..., 8192) | 验证 |
| VSL: batch_size = b/2^i, 每步总token恒定 | 验证 |
| Grow-P2 课程学习策略 | 验证 |
| 每个序列来自单一文档 (零跨文档注意力) | 验证 |
| 最长序列主导batch计算成本 | 验证 |
| 6× 训练加速 (time-to-accuracy) | 多个来源一致 |
| 4×+ 数据效率 | 验证 |
| 8k上下文模型成本≈2k基线 | 验证 |
| Apple 开源 (github.com/apple/ml-dataset-decomposition) | GitHub确认 |
| 基于OpenLM框架 | GitHub README |

## ⚠️ 教学构建（非论文原文）

- README.md 中的 **10文档分解示例** 是我构造的
- **1000文档/2M tokens示例** 是推算演示
- **Grow-P2课程权重公式** 可能和论文原文符号不同
- **消融实验对比表** 基于我理解的推断，具体数字需对照原文
- **RULER得分** (73.0 vs 55.1) 我记忆中与LongPack论文的数据可能混淆，需核实

## 📎 建议
- 二进制分解 + VSL + Grow-P2 的核心逻辑可引用
- 6×加速和4×数据效率可引用
- 具体实验数字建议从GitHub README或论文原文确认
- 代码仓库有实际可运行的示例，强烈建议跑一下
