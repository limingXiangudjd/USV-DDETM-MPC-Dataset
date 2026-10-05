# USV Parameter-Separated D-DETM-MPC — Experiment Dataset / 实验数据集

> Dataset for: *"A Parameter-Separated Dynamic Event-Triggered Predictive Control Architecture for
> Unmanned Surface Vehicles: Quantifying the Continuous-to-Discrete Safeguard Loss and Its
> Reset-Free Recovery"* (Li Mingxiang, Dalian Maritime University)
>
> **DATA ONLY — 本仓库只包含实验数据，不含任何代码。** 实现代码（MATLAB/Simulink 模型与脚本）
> 在私有仓库 [USV-Gain-Separated-DET-MPC](https://github.com/limingXiangudjd/USV-Gain-Separated-DET-MPC)，
> 查看权限需向作者申请：**limingxiang@dlmu.edu.cn**。
> **This repository hosts experiment data only.** The implementation code (MATLAB/Simulink models
> and scripts) lives in the private repository above. Viewing that code requires the author's
> authorization (email limingxiang@dlmu.edu.cn with your purpose; access is granted as a
> repository collaborator).
>
> **Code usage restriction**: the private code repository carries NO open-source license and all
> rights are reserved. Without the author's prior written permission, the code may not be used
> to publish academic papers or theses, nor for other academic experiments or benchmarks.
>
> **代码使用限制（中文）**：上述私有代码仓库未授予任何开源许可证，保留全部权利。
> **未经作者本人书面许可，不得使用该代码发表学术论文、学位论文或会议文章，
> 亦不得将其用于其他学术实验、评测或教学；如需引用/复现/扩展，请先邮件获得书面许可。**

## 1. 数据集内容 / What is included

论文全部 **13 组实验**（覆盖论文每一张表格与数值图）× **每组建多次 ≥3 轮独立复现**
（互相隔离的 MATLAB R2024b 会话、不同日期运行），共 **94 个数据文件**。
All 13 experiment groups of the paper (every table and quantitative figure), each provided in at
least **three independent reproduction rounds** — 94 data files in total.

三轮关键标量**逐位一致（bit-identical, max|Δ| = 0）**，蒙特卡洛与多种子的逐样本 CSV
**跨轮字节级相同**；论文表值经 182 项自动核对全部通过（182/182 PASS）。
Key scalars are **bit-identical across rounds** (max|Δ| = 0); per-sample CSVs of the Monte-Carlo
and multi-seed campaigns are byte-identical; 182/182 paper-vs-data checks pass.

**Reproducibility statement / 可复现性声明**: these three rounds were produced by isolated
MATLAB R2024b sessions on different dates, re-running the same archived scripts and seed
protocols. The per-group cross-round verification records (19/19 groups, max|Δ| = 0) are listed
in `README_REPRODUCTION.md`. The experiment source code itself is NOT in this repository — it is
kept in the private repository above and requires the author's consent to view; reproducing the
numbers from code therefore also requires that authorization.

## 2. 文件命名 / Naming convention

`NN_实验名_Vk.mat`：NN = 论文章节顺序编号；Vk = 复现轮次（V1/V2/V3/V4）。

| 组 | 实验 | 论文位置 | 轮次 | 关键数值（各轮一致） |
|---|---|---|---|---|
| 01 | 消融 7 案例（含静态 ETM） | Table 4 | V1/V2/V3（staticETM: V1/V3） | C1 6.30°/7.48 Hz/62.6%/526.81 N … C7 6.60°/7.44 Hz/62.8%/528.28 N |
| 02 | ζ 内变量轨迹（4239 步逐步） | §5.2, Fig 2–3 | V1/V2/V3 | min ζ = −0.3133, max = 1.9310, 12 zero-crossings |
| 03 | 采样周期独立性（Ts 三档 + 逐步 ζ/J_dev/σ 轨迹） | Table 6 | V1/V2/V3 | RMSEψ 1.33 / 2.24 / 6.88° |
| 04 | 反事实 reset 2×2 因子 | Table 3 | V2/V3/V4 | V0 37.38%/7.48 Hz … V3 61.87%/12.37 Hz |
| 05 | Exp1 蒙特卡洛 300 次（逐样本 CSV） | Table 5 Group 1 | V1/V2/V3 | 2.27 / 2.26 / 2.28°, QP 不可行 0/300 |
| 06 | Group 2 物理约束摄动 200 次 | Table 5 Group 2 | V1/V2/V3 | σ=20%: 2.31±0.14°, RMSE_y 0.073±0.035 m |
| 07 | D-DETM 参数 343 点扫描 | §5.6 | V1/V2/V3 | 343/343 valid，零发散 |
| 08 | 多种子稳健性 10 seeds × 2000 次（逐样本 CSV） | §5.7, Fig 6 | V1/V2/V3 | per-seed P95 ∈ [1.3320, 1.4834] |
| 09 | β_ξ 传播代理（Bootstrap P95） | §5.7, Fig 10 | V1/V2/V3 | P95 = 1.5137 [1.366, 1.721] |
| 10 | 推力 PSD 窄带四频带（全谱） | Fig 8 | V1/V2/V3 | 0.5–10 Hz **+10.03 dB** |
| 11 | 抖振抑制（vs 准 ST-SMC） | §5.7 | V1/V2/V3 | std 3.0268 → 1.5360 N = **−49.3%** |
| 12 | UKF β 对比 / 协方差条件数 / κ 逐步序列 | 附录 D | V1/V2/V3/V4（κ: V2/V3/V4） | κ̄ 133.6 / 133.6, TOST PASS |
| 13 | WAM-V 跨平台三跑（顺序会话协议，含时间序列） | Table 7 | V1/V2/V3 | P: 0.1711 / 0.1065 / 82.3498 |
| 13b | WAM-V 独立会话实现（论文 Table 7 脚注） | Table 7 脚注 | V1/V2/V3 | N2 1.3858/0.9037/88.7012; P 0.1726/0.1009/82.5392 |

轮次例外说明（详见 `README_REPRODUCTION.md`）：04 组无 V1（该实验于第二轮才加入）；
12 组 κ 序列为 V2/V3/V4；06 组 V1 取第一轮合规口径文件。

## 3. 完整性校验 / Integrity verification

```bash
md5sum -c manifest_paperdata.md5      # 95/95 OK
```

`README_REPRODUCTION.md` 内含：组×轮次×论文位置矩阵、生成脚本映射、三轮一致性逐组核对记录
（19/19 组 max|Δ|=0）与复现协议（随机种子流、WAM-V 两种会话协议）。

## 4. 复现说明 / Reproduction notes

- 确定性实验（消融/ζ/Ts/2×2/WAM-V 单跑等）：独立会话重跑即得逐位相同结果；
- 统计实验（Exp1/MC 类）：使用 `-batch` 启动随机流（Mersenne Twister seed 0）+ 传感器噪声块
  显式种子（run_idx/+1000/+2000），按此协议跨会话确定；
- WAM-V 主表（Table 7）采用**同会话顺序执行**协议（Run1→Run2→Run3 共享连续随机流）；
  13b 组为独立会话下的另一合法实现（论文脚注值）；
- 生成代码与 Simulink 模型不在本仓库：见顶部的私有代码仓库申请说明。

## 5. 引用 / Citation

如使用本数据集，请引用上述论文并注明数据集仓库。商用或再分发请联系作者。
If you use this dataset, please cite the paper above and this repository. Contact the author for
commercial use or redistribution.

© 2026 Li Mingxiang, Dalian Maritime University. All rights reserved.
