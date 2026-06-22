# 大模型预训练数据拼接策略

> LLM Pretraining Data Packing Strategies — 四篇顶会论文精讲

## 项目结构

```
├── 数据拼接策略_终稿.pptx    ← 30页终稿PPT
├── 演讲稿.md                ← 逐页演讲稿（约13000字）
├── make_ppt.py              ← PPT生成脚本
│
├── 01_随机拼接_基线/         ← 基线方法
│   └── README.md
│
├── 02_BestFit_Packing/       ← Amazon, ICML 2024
│   ├── README.md
│   ├── paper.pdf
│   ├── FIGURES_INDEX.md
│   └── figures/  (22张)
│
├── 03_InContext_Pretraining/ ← Meta, ICLR 2024
│   ├── README.md
│   ├── paper.pdf
│   ├── FIGURES_INDEX.md
│   └── figures/  (10张)
│
└── 04_Dataset_Decomposition/ ← Apple, NeurIPS 2024
    ├── README.md
    ├── paper.pdf
    ├── FIGURES_INDEX.md
    └── figures/  (23张)
```

## 四篇论文

| # | 方法 | 论文 | 会议 |
|---|------|------|------|
| ① | 随机拼接 (Concat-and-Chunk) | GPT-3 / LLaMA 等 | — |
| ② | Best-Fit Packing | Ding, Wang et al. | ICML 2024 |
| ③ | In-Context Pretraining (ICLM) | Shi, Min et al. | ICLR 2024 |
| ④ | Dataset Decomposition (DD) | Pouransari, Li et al. | NeurIPS 2024 |

## 使用方法

1. 打开 `数据拼接策略_终稿.pptx` 查看完整PPT
2. 对照 `演讲稿.md` 逐页讲解
3. 各论文子目录含完整论文PDF和提取的图表
