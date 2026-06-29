---
title: "QuantImmuBench — 新抗原免疫原性预测工具部署与定量基准评测"
excerpt: "把 16 个免疫原性 / 提呈预测工具统一部署到 HPC，在 ELISpot 定量基准上 apples-to-apples 横评。核心发现：现有工具普遍只能弱相关（全局 Spearman ρ < 0.4），「反应强弱定量」仍是开放问题。<br/><img src='/images/quantimmu-ceiling.png' width='480'>"
collection: portfolio
date: 2026-06-28
header:
  teaser: quantimmu-ceiling.png
---

癌症个性化新抗原疫苗研究中的一个工程化基准评测台。最终目标是超越当前「免疫原性有 / 无」的二分类，去估计 T 细胞反应的**强弱程度（magnitude）**。作为第一步，本项目把一批现有免疫原性 / 提呈预测工具统一部署到 HPC 环境，在共享的 ELISpot 定量基准上跑通，并给出带统计不确定性的横向对比。

## 做了什么

- **部署 16 个已授权工具**进 ELISpot 基准做 apples-to-apples 横评（DeepImmuno、PredIG、pTuneos、IMPROVE、NeoTImmuML、PRIME、ImmuneApp、deepHLApan、BigMHC、CNNeo、IEDB Calis、MHCflurry、Repitope、TSCAPE，外加 HLAthena 作 presentation proxy 单列）。
- 每个工具收集四类交付信息：输入模板 / 运行参数 / 输出格式与含义 / 工具简介与边界。
- 三套统计口径互为印证：**per-patient Fisher-Z 加权 Spearman**（主指标，计入患者间差异）、全局 max-聚合 Spearman（对照）、AUC（SFC > 0）。所有数字带 bootstrap 置信区间。

## 关键结论

评测集为 ELISpot 基准 DS2（Braun 2025 *Nature*，肾癌 ccRCC，101 条肽 / 9 位患者）。

- **唯二统计显著的工具是 PRIME（per-patient ρ = +0.279，95% CI [0.050, 0.481]）与 IMPROVE（ρ = +0.250，CI [0.021, 0.455]）** —— 仅这两个的置信区间整体排除 0。
- 全局口径下，**IMPROVE（ρ = 0.252，p = 0.011）与 PredIG（ρ = 0.201，p = 0.044）** 达显著；IMPROVE 是唯一在两套口径下都显著的工具。
- **天花板：所有工具的全局 Spearman 都 ρ < 0.4。** 现有工具对反应强弱的排序能力普遍很弱 —— 「定量（magnitude）预测」仍是一个开放问题，这正是后续方法创新（QuantImmune）的立项空间。

<img src="/images/quantimmu-perpatient.png" width="600" alt="17 条目 per-patient Fisher-Z Spearman + 95% CI">

## 工程要点

诚实分级、不夸大：NeoTImmuML 为自训复刻版（官方不发权重，标★非官方）、pTuneos 仅跑 Pre&RecNeo 子模型、IMPROVE 为 Expression 特征降级、HLAthena 仅作提呈代理（AUC ≈ 0.51 近随机，印证提呈 ≠ 免疫原性）。

## 代码

代码与逐工具文档：[github.com/legacccY/quantimmu-bench](https://github.com/legacccY/quantimmu-bench)
（仓库当前为私有 —— 涉及 netMHCpan / TSCAPE / BigMHC 等第三方工具的许可红线，开放需先取得相应书面同意；可应需提供。）
