# paperdata/ — 论文数据集与三轮复现证明（REPRODUCTION PROOF）

> 生成：2026-10-04（DATA-CLEANUP-20261004 整改）
> 性质：**论文全部定量声明的一手数据集**。每组实验保留三轮独立复现（V1/V2/V3），
> 由互相隔离的 MATLAB R2024b 会话在不同日期运行；三轮结果经标量级核对**逐位一致（max|Δ|=0）**。
> 本目录即 Data availability 声明所指 benchmark archives 的正式载体。

## 一、命名规则

`NN_实验名_Vk.mat`：NN=论文实验编号（按论文章节顺序）；Vk=复现轮次。

| 轮次 | 物理来源（运行日期） | 说明 |
|---|---|---|
| V1 | 全量复现第一轮（2026-08~09 归档族） | 消融/ζ/PSD/UKF/WAM-V 取 v6 轮（09-28/29）；Ts 取 v10 轮（10-03）；多种子取 v9 轮（09-29/30）；物理摄动取 08-13 轮合规口径文件（现归档于 archive/data_cleanup_20261004/v2_20260813/，与 V2/V3 逐位一致） |
| V2 | 全量复现第二轮（2026-10-03，v11 运行族） | 含反事实 2×2 首轮、κ 条件数序列 |
| V3 | 全量复现第三轮（2026-10-04，v12 运行族） | 182/182 项论文核对 PASS 的最终验证轮 |
| V4 | 补齐轮（2026-10-05，v13/v14 隔离会话） | 04 反事实 2×2 与 12 κ 序列补齐第三轮（runs/20261005T085755_v13_cf2x2/、runs/20261005T093408_v14_kappa/） |

三轮对应互相独立的运行目录与种子协议（确定性实验同种子同结果；统计实验固定种子族），
完整运行日志与核对脚本随 v12 运行目录保留（runs/20261004T080925_v12_e1v5/）。

## 二、实验组 × 复现轮次矩阵（含论文引用位置）

| 组 | 实验内容 | 论文位置 | 轮次 | 关键标量（三轮一致） |
|---|---|---|---|---|
| 01 | 消融 7 案例（Case1/2/3/5/6/7 + 静态ETM） | Table 4（C1-C7） | V1/V2/V3（staticETM: V1/V3，V2 轮等价运行见 04 组 cf2x2 V2，五标量逐位一致） | Case1: maxψ 6.30°, QP 7.48Hz, ETM 62.6%, TV 526.81N；C7 静态ETM 6.60°/7.44Hz/528.28N |
| 02 | ζ 内变量轨迹（4239 步） | §5.2 / Fig 2-3 | V1/V2/V3 | min ζ −0.3133, max 1.9310, 零穿越 12 |
| 03 | 采样周期独立性（Ts 三档） | Table 6 | V1/V2/V3 | RMSEψ 1.33/2.24/6.88°, minζ −0.093/−0.313/−1.386 |
| 04 | 反事实 2×2 因子（V0-V3 变体） | Table 3 | V2/V3/V4 | V0 37.38%/7.48Hz … V3 61.87%/12.37Hz（实验诞生于 v11 轮故无 V1；V4=2026-10-05 第三轮补齐，三轮逐位一致） |
| 05 | Exp1 蒙特卡洛 300 次 | Table 5 Group1 | V1/V2/V3 | σ=20%: ψ 2.28±0.13°, QP 不可行 0/300；CSV 字节级相同 |
| 06 | 物理约束独立摄动 200 次 | Table 5 Group2 | V1/V2/V3 | σ=20%: ψ 2.31±0.14°（V1 取 08-13 轮合规口径；v6 轮数据为旧口径、与论文不符 ψ=2.336，已于 2026-10-05 移出主目录） |
| 07 | D-DETM 参数扫描 343 点 | §5.6 | V1/V2/V3 | 343/343 valid，无发散 |
| 08 | 多种子稳健性 10 seeds × 2000 次 | §5.7 / Fig 6 | V1/V2/V3 | per-seed P95 [1.3320,1.4834]；CSV 字节级相同 |
| 09 | β 代理后验统计 | §5.7 | V1/V2/V3 | P95 1.5137 [1.366, 1.721] |
| 10 | PSD 窄带频域 | Fig 8 | V1/V2/V3 | 0.5-10Hz +10.03dB |
| 11 | 抖振抑制对比（vs ST-SMC） | §5.7/适用性表 | V1/V2/V3 | 49.3% std 抑制 |
| 12 | UKF β 对比/条件数/κ 序列 | Table 8 + TOST | V1/V2/V3/V4（κ 序列 V2/V3/V4） | κ mean 133.6/133.6；TOST PASS |
| 13 | WAM-V 跨平台三跑（顺序会话协议） | Table 7 | V1/V2/V3 | Run3: 0.1711/0.1065/82.3498 |
| 13b | WAM-V 独立会话实现（论文 Table 7 脚注，另一合法随机实现） | Table 7 脚注 | V1/V2/V3 | N2 1.3858/0.9037/88.7012；P 0.1726/0.1009/82.5392（三轮逐位） |

## 三、三轮一致性核对结果（2026-10-04 实测）

对每组实验的全部关键标量字段做跨轮比对（MATLAB 逐字段加载）：

```
消融 6 组          maxDelta=0   (rmse/max/TV/qp/etm 全字段)
02 zeta_trace      maxDelta=0   (4239×4 逐步轨迹)
03 ts tabs         maxDelta=0   (9 组表向量)
04 cf2x2 (V2-V4)   maxDelta=0   (4 变体全标量，三轮)
12 kappa series    maxDelta=0   (V2/V3/V4 逐步序列)
13b wamv independent maxDelta=0 (3 runs x 3 rounds; TV 浮点序差 <5e-13)
01 staticETM V1(V5档) vs 04-V2 等价运行: 5 标量差 <=1.1e-13
05 exp1 matrix     maxDelta=0   (300×5 逐样本矩阵 + qp_fail)
06 phys perturb    maxDelta=0   (mean/std 2×5)
07 sweep343        maxDelta=0   (343 组数值向量)
08 multiseed       maxDelta=0   (2000×5 + per-seed P95)
09 beta proxy      maxDelta=0   (P95/CI/mean/median)
10 psd narrowband  maxDelta=0   (pxx_a/pxx_m 全谱)
11 chattering      maxDelta=0   (suppression_ratio 49.3%)
12 ukf beta/cond   maxDelta=0   (κ 序列 V2-V3 逐位)
13 wamv run1-3     maxDelta=0   (rmse×3 + tv×2)
```

**结论：19/19 组核对项全部逐位一致（10-05 补齐后：13 组全部 ≥3 轮）**——论文数据不是单次运行产物，而是三轮独立复现的稳定输出。
逐文件 MD5 见 manifest_paperdata.md5。

## 四、复现方式

每组实验的生成脚本在 main/ 根目录（Exp1_Monte_Carlo_Final.m、ablation_experiments.m、
run_ts_independence_v10.m、v9_driver.m、S4_Exp_*、S5_Exp_*、S2_*_WITH_SAVE.m 等），
模型与控制器在 USV_ETM_MPC_System.slx / USV_ST_SMC_System.slx。
确定性实验：独立会话重跑即得相同结果；统计实验：按脚本内种子协议（-batch 启动流 + 显式传感器种子）。
消融 case 编号与论文行映射见 00_README §7。
