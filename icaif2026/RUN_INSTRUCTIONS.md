# 当前状态(更新于 2026-07-30)

这份文件夹里的代码和数据现在处于"已验证可复现"的状态。下面是最新进展,之前那份(2026-07-28 前后写的)"预计3.5-4.5小时"版说明已经过时,历史记录留在文末,不要照那份操作。

## 一句话总结

- MCMC 链、DM 检验表、回测表——**两次独立重跑,字节级完全一致**,可复现性问题已解决。
- Driscoll-Kraay SE、issuer 固定效应稳健性检验、Granger 因果检验(修正列选择+延长滞后期)——**都已跑出真实结果**。
- 目前确认的问题清单和建议处理方式,见发给 Jimena 的状态更新消息(不重复贴在这里,避免两处不同步)。

## 里面有什么

- `01_CETI_Pipeline_FIXED.ipynb` —— 修了:Granger检验漏合并NPL数据;`df_panel_training`未定义导致Driscoll-Kraay/issuer FE/Bai-Perron三个诊断cell从未跑通;`fit.resid.values`的AttributeError。新增了`[C-14 EXTENDED]`(延长Granger滞后期)、`[PREP]`(构造df_panel_training)、`[C-5 EXTENDED]`(issuer FE的系统级稳健性检验)三个cell。
- `03_TailRisk_HorseRace_FIXED.ipynb` —— 修了路径写死、随机种子非确定性(改成按model/asset/quantile哈希生成种子)两个bug。**已完成两次独立完整重跑验证**(删除chain文件后重新生成,不是读缓存),8个chain文件和所有下游表格字节级完全一致。
- `02_CETI_Analysis_reference_only.ipynb` —— 代码本身没改动。用于核对calm/crisis拼接问题(Section 6)和crisis事件定义(cell 2的`CRISES`列表)。
- `clri_lstm_ready.csv` / `financial_system_index.csv` / `Consolidado_SBS_Final.csv` / `df_master_export.csv` —— notebook需要用到的数据文件。
- `chain_LSTM_{BASE,CETI}_{BBVA,BCP}_{a01,a05}.npy`(8个文件)—— **当前有效、已验证可复现的MCMC参数文件**,是两次独立重跑后确认字节级一致的那一批。
- `HORSERACE_OUTPUT/TABLES/` —— `dm_test_results.csv`(新的Table 5数字)、`horse_race_all.csv`、`crisis_breakdown.csv`,均已验证两次独立跑法结果一致。
- `run1_backup重新跑之前的outputs和chains/` —— 第一次跑的备份,留作对比用,不是当前有效版本。
- `old_chains_for_reference_DO_NOT_USE_asof_regeneration/` —— 更早期的GitHub仓库原始chain文件,仅供历史对比,**不要用这批**。

## 如果 Jimena 想自己重新验证一遍

1. `01_CETI_Pipeline_FIXED.ipynb` 需要原始 Economatica 数据(`DATA/`文件夹),放到能找到该文件夹的项目目录下,Restart Kernel → Run All。重点看这几个新cell的输出:`[C-14 PREP]`(NPL合并)、`[PREP]`(df_panel_training构造)、`[C-6]/[C-7]`(Bai-Perron+DK SE)、`[C-5 EXTENDED]`(issuer FE稳健性)、`[C-14 EXTENDED]`(Granger延长滞后期)。
2. `03_TailRisk_HorseRace_FIXED.ipynb` 不需要原始数据,只需要`clri_lstm_ready.csv`。**如果文件夹里已经有本次验证过的8个chain文件,直接Run All会走"loaded from cache",几秒钟出结果,和现有的`dm_test_results.csv`应该完全一致**——这就是复现验证本身。如果想验证"从零开始重新生成也一样",需要先把8个chain文件移到别的地方,Run All等待MCMC重新采样(单次约3.5-4.5小时),再和现有结果做byte-level对比。

## 有问题随时找我

---

## 历史记录(2026-07-28,已过时,仅供参考)

<details>
<summary>点击展开——这是MCMC重新生成之前写的操作说明,当时的"预计3.5-4.5小时"等描述现在已经不适用(已经跑完并验证两次)</summary>

第一步：跑 01_CETI_Pipeline_FIXED.ipynb（修Granger检验）,需要原始Economatica数据，从头跑到底，跑到`[C-14 PREP]`应该打印"NPL merged into df_master: 673 of 730 weeks have a value"。

第二步：跑 03_TailRisk_HorseRace_FIXED.ipynb（重新生成一套匹配的MCMC参数）,不需要原始数据,只需要`clri_lstm_ready.csv`。如果文件夹里有旧的chain文件先删掉或移开,Restart Kernel → Run All,预计3.5-4.5小时,跑完后验证两次结果是否一致。

第三步：把新数字更新回论文。

（`financial_system_index.csv`原始数据本身有156个月重复记录，合并代码里已按同月取平均处理。）

</details>
