# TNF-alpha VHH 筛选保留24条：设计指标、结构与方法汇总

整理日期：2026-11-19。范围为原OpenDDE单体复合物验证前30条中，按用户指定剔除6条后保留的24条唯一VHH序列，继续按ipSAE降序、ipTM降序排列。本报告不是原排名前24条。剔除清单：E08_L13_R2、E04_L14_R1、E04_L20_R1、E03_L11_R2、E10_L12_R1、E02_L12_R1。原始top30资料保留不变；本上传包仅收集筛选后的24条及其48对象PyMOL会话。此排名不是实验亲和力、成药性或三聚体结合成功率排名。

## 结论与指标边界

保留清单首位E01_L13_R1的单体ipSAE为0.820638，但其当前姿势与完整TNF三聚体明显冲突。此次按用户指定剔除6条，并不等于其余24条都通过三聚体几何筛查。保留24条中仍有7条在三种亚基映射下均明显冲突，12条在两种拟合下未见<2 Å邻亚基碰撞，另5条为局部或边界情况。使用这些序列时必须同时参考空间兼容性，不能只依据单体置信度。未做结合实验。

- **ipTM / ipSAE**：主表使用OpenDDE-ABAG对单体TNF＋VHH的单样本结果。ipTM来自summary_confidence；ipSAE由对应token_pair_pae和CIF通过Dunbrack参考脚本计算，PAE阈值10 Å、距离阈值15 Å，取双方向最大d0res值。所有结构已核验靶点/VHH序列及PAE链顺序、维度。
- **自治性**：此术语没有在本项目中预先定义数值指标。若指VHH脱离抗原后的独立折叠/稳定性，本批没有VHH单独预测或实验，标记“未评估”。复合物中的VHH pLDDT不能代替自治性。若指设计–复折自洽性，当前只有TNF对晶体参考的叠合RMSD，不是VHH设计–复折RMSD；本报告不混用。
- **物理类指标**：提供目标Cα叠合RMSD、VHH与新增邻亚基重原子<2 Å的碰撞原子对、主链碰撞、最近距离，以及CDR/框架接触残基数。未计算结合自由能、Rosetta能量、界面埋藏面积、氢键网络能量、MD稳定性或实验Kd。
- **序列成药性评分**：未运行经过验证的成药性/可开发性综合评分，不编造0–100分。新增可复核的序列初筛：长度、Cys数量和位置、N-X-S/T（X≠P）基序位置、最长连续同残基段。它们仅为复核线索，不能直接判定表达、聚集、免疫原性或成药性。未做人源化评分、溶解性预测、聚集预测、热稳定性或表达验证。

## 方法学与设计来源

**设计模型：RFD4-Proteina。** 使用原生服务，源码提交6fc44c060ef96d4c759bafeaa1eae1a66a5be67c，权重revision a06e2509c7033b010fc43614e9fd840303588502。以ozoralizumab VHH示例骨架设计，固定83个框架残基，重设计CDR；CDR1固定13，CDR2按示例9–10，扫描CDR3=11–20。10组表位/热点，每组10个CDR3长度，每长度2个设计，共200条记录、198条唯一序列。60步、thinking_high、batch=1、bf16-mixed；每任务seed见原始request/effective-config，跨热点组同长度复用了seed，不能视作所有设计完全独立随机重复。

**结构条件与靶点：** 1TNF三聚体A/B/C成熟编号6–157（每链152 aa）；TNF主链以C_CRDGRP=2、VHH框架主链以组1输入内部坐标条件；靶点和框架序列C_SEQ固定，靶点A链热点CB标记C_HOT。这里“固定”表示输入条件，不保证生成坐标严格满足；原200条中曾发现12条TNF Cα镜像样异常，原始质控标记在每条详情保留。

**表位来源：** 各设计E01–E10对应portfolio中保存的来源复合物和热点选择理由，来源结构逐条列于下方。热点不是本轮OpenDDE预测的约束，也未以原抗体复合物作为OpenDDE模板。

**本轮折叠模型：OpenDDE-ABAG。** CLI配置名opendde_v1，但显式加载抗原–抗体权重opendde_abag.pt；权重SHA256为5cf37441ddef2a2f148b81dd4a218ad274f996fecaf17dec901ab6cf1351713d，仓库commit ddfa1df8aff1babf1fddac4247b7d2351bd0ce9f，PyTorch 2.7.1+cu126。每条输入P01375 Val77–Leu233单体157 aa（A链）＋VHH（H链），seed101，10 recycle、200 diffusion steps、1个样本、fp32，无MSA/模板/热点或RFD4坐标。使用ctolab21、作业43701的8张A6000，200条均完成并评分。未使用该模型做本轮序列优化。

**此前折叠验证：ESMFold2。** 原始批次为3条TNF参考序列＋1条VHH，无MSA/模板，seed0，3 loops、50 steps、1 sample。每条详情提供此前VHH链平均pLDDT与VHH–TNF ipTM、ipSAE及结构链接。全局三聚体ipTM会包含TNF–TNF贡献，因此不作为本报告主排序。只有特定两条做过单体＋MSA复核，并非本清单24条都做过。

**三聚体叠合：** 以152个同编号Cα作Kabsch正常旋转/平移，将OpenDDE靶点分别叠合到1TNF A/B/C，带动VHH刚体变换；另做核心位点拟合敏感性检查。导出结构是参考三聚体＋放置VHH，未松弛、未重新预测、没有新的三聚体ipTM/ipSAE。1TNF成熟143为L，而P01375为D，且晶体缺少1–5；这些差异限制边界接触的解释。

## 保留24条主指标

碰撞数仅指对齐A后VHH与另外两亚基的<2 Å重原子对；RMSD为TNF对1TNF参考，不是VHH自洽性。热点为同一对齐亚基上≤4.5 Å几何邻近，不是PLIP判定。

|排名|设计|OpenDDE ipTM|OpenDDE ipSAE|TNF Cα RMSD Å|邻亚基碰撞对|其中主链–主链|热点邻近|三聚体评估|
|---:|---|---:|---:|---:|---:|---:|---:|---|
|1|E01_L13_R1|0.9106|0.8206|1.353|351|84|1/4|三种映射均明显冲突|
|2|E10_L18_R1|0.7356|0.4923|1.313|0|0|0/4|两种拟合未见<2Å碰撞|
|3|E09_L11_R2|0.7473|0.4435|1.408|0|0|0/4|两种拟合未见<2Å碰撞|
|4|E01_L20_R2|0.7312|0.4389|1.267|26|0|2/4|局部/边界，需复核|
|5|E02_L13_R2|0.7522|0.4311|1.315|414|96|1/4|三种映射均明显冲突|
|6|E05_L13_R2|0.7601|0.4303|1.291|403|81|0/4|三种映射均明显冲突|
|7|E09_L11_R1|0.7078|0.4083|1.421|0|0|0/4|两种拟合未见<2Å碰撞|
|8|E03_L18_R2|0.7068|0.3803|1.267|0|0|2/4|两种拟合未见<2Å碰撞|
|9|E06_L13_R1|0.6943|0.3385|1.471|296|48|2/4|三种映射均明显冲突|
|10|E07_L11_R2|0.6488|0.3088|1.351|0|0|0/4|两种拟合未见<2Å碰撞|
|11|E02_L19_R1|0.6686|0.2898|1.433|5|0|2/4|局部/边界，需复核|
|12|E04_L13_R2|0.6538|0.2658|1.314|0|0|2/4|局部/边界，需复核|
|13|E09_L19_R1|0.6259|0.2418|1.282|0|0|0/4|两种拟合未见<2Å碰撞|
|14|E10_L20_R2|0.6092|0.2362|1.511|0|0|2/4|两种拟合未见<2Å碰撞|
|15|E08_L18_R1|0.6057|0.2242|1.946|0|0|0/4|局部/边界，需复核|
|16|E06_L14_R1|0.6168|0.2240|1.311|289|48|3/4|三种映射均明显冲突|
|17|E06_L15_R1|0.5112|0.2010|1.340|0|0|0/4|两种拟合未见<2Å碰撞|
|18|E08_L13_R1|0.5790|0.1964|1.348|0|0|2/4|两种拟合未见<2Å碰撞|
|19|E07_L16_R1|0.5268|0.1739|1.355|0|0|0/4|两种拟合未见<2Å碰撞|
|20|E10_L11_R1|0.6023|0.1735|1.420|8|0|0/4|局部/边界，需复核|
|21|E04_L17_R1|0.5708|0.1684|1.320|487|162|0/4|三种映射均明显冲突|
|22|E01_L13_R2|0.5962|0.1642|1.241|0|0|2/4|两种拟合未见<2Å碰撞|
|23|E03_L17_R1|0.5367|0.1599|1.329|0|0|2/4|两种拟合未见<2Å碰撞|
|24|E04_L16_R2|0.5460|0.1321|1.352|258|42|0/4|三种映射均明显冲突|

## 逐条序列、来源和结构文件

### 1. E01_L13_R1

原top30排名：1；本筛选清单排名：1。

- 设计来源：5YOY；输入热点：A:67Q、A:104E、A:107E、A:138R。选择理由：Q67/E104/E107/R138 覆盖已报道接触面中央的极性与带电残基；保留原首批组合以作不同随机种子的重复对照。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.910639，ipSAE=0.820638，全复合物pLDDT=92.71（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.167388，ipSAE=0.000000，VHH平均pLDDT=81.70。
- 物理初筛：邻亚基最近重原子距离0.290 Å；碰撞涉及39个VHH残基；原预测界面CDR/框架接触残基数=18/5。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E01_L13_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E01_L13_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E01_L13_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E01_L13_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E01_L13_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E01_L13_R2/rfd4_provenance.json)

```fasta
>E01_L13_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYDMSWVRQAPGKGLEWVSSISTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARYDPHSSSWYPYRGQGTLVTVS
```

### 2. E10_L18_R1

原top30排名：4；本筛选清单排名：2。

- 设计来源：21TW_VHH1；输入热点：B:89T、B:92N、C:146E、C:147S。选择理由：独立复合物中的 VHH1 接触提供跨亚基证据；选 B:T89/N92 与 C:E146/S147 的极性子簇，比较与 8Z8M 组合的鲁棒性。
- CDR1/2/3长度：13/9/18；总长123 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.735631，ipSAE=0.492345，全复合物pLDDT=91.27（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.568294，ipSAE=0.141876，VHH平均pLDDT=89.44。
- 物理初筛：邻亚基最近重原子距离10.371 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=16/2。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E10_L18_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E10_L18_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E10_L18_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E10_L18_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E10_L18_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E10_L18_R1/rfd4_provenance.json)

```fasta
>E10_L18_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMAWVRQAPGKGLEWVSVITTGGSTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARHSSPAYYSYDADDYPYRGQGTLVTVS
```

### 3. E09_L11_R2

原top30排名：5；本筛选清单排名：3。

- 设计来源：8Z8M；输入热点：B:87Y、B:90K、C:146E。选择理由：TNF30/ozoralizumab 相关 VHH 来源，选择 B:Y87/K90 与 C:E146 的跨亚基组合；骨架来源与表位来源相容性作为待检验假设。
- CDR1/2/3长度：13/9/11；总长116 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.747318，ipSAE=0.443497，全复合物pLDDT=90.56（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.220837，ipSAE=0.000000，VHH平均pLDDT=82.69。
- 物理初筛：邻亚基最近重原子距离7.998 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=17/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E09_L11_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E09_L11_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E09_L11_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E09_L11_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E09_L11_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E09_L11_R1/rfd4_provenance.json)

```fasta
>E09_L11_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYSMGWVRQAPGKGLEWVSSISWGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARDVSYYDWDYRGQGTLVTVS
```

### 4. E01_L20_R2

原top30排名：6；本筛选清单排名：4。

- 设计来源：5YOY；输入热点：A:67Q、A:104E、A:107E、A:138R。选择理由：Q67/E104/E107/R138 覆盖已报道接触面中央的极性与带电残基；保留原首批组合以作不同随机种子的重复对照。
- CDR1/2/3长度：13/10/20；总长126 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.731232，ipSAE=0.438864，全复合物pLDDT=90.94（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.171523，ipSAE=0.000000，VHH平均pLDDT=81.71。
- 物理初筛：邻亚基最近重原子距离0.644 Å；碰撞涉及6个VHH残基；原预测界面CDR/框架接触残基数=13/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 96]；N-X-S/T基序起始位置[52]；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E01_L20_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E01_L20_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E01_L20_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E01_L20_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E01_L20_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E01_L20_R2/rfd4_provenance.json)

```fasta
>E01_L20_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSDYAMGWVRQAPGKGLEWVSAINSSGGTTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARMSSSFPSASALSADLFTYRGQGTLVTVS
```

### 5. E02_L13_R2

原top30排名：7；本筛选清单排名：5。

- 设计来源：5YOY；输入热点：A:23E、A:67Q、A:138R。选择理由：从同一表位中选择较精简的三点约束，包含酸性、酰胺和碱性侧链，检验减少热点是否改善 VHH 可接近性。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.752197，ipSAE=0.431137，全复合物pLDDT=90.12（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.203309，ipSAE=0.000000，VHH平均pLDDT=84.23。
- 物理初筛：邻亚基最近重原子距离0.239 Å；碰撞涉及41个VHH残基；原预测界面CDR/框架接触残基数=13/8。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E02_L13_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E02_L13_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E02_L13_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E02_L13_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E02_L13_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E02_L13_R2/rfd4_provenance.json)

```fasta
>E02_L13_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYDMGWVRQAPGKGLEWVSAISTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARYSAYGYSNFDYRGQGTLVTVS
```

### 6. E05_L13_R2

原top30排名：8；本筛选清单排名：6。

- 设计来源：5M2J；输入热点：A:88Q、A:90K、A:92N。选择理由：单链 VHH 表位中央 Q88/K90/N92 为邻近极性簇，适合用少数侧链锚定小型结合界面。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.760057，ipSAE=0.430263，全复合物pLDDT=89.80（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.216512，ipSAE=0.000000，VHH平均pLDDT=82.53。
- 物理初筛：邻亚基最近重原子距离0.266 Å；碰撞涉及43个VHH残基；原预测界面CDR/框架接触残基数=12/3。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E05_L13_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E05_L13_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E05_L13_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E05_L13_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E05_L13_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E05_L13_R2/rfd4_provenance.json)

```fasta
>E05_L13_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMGWVRQAPGKGLEWVSVISTGGSTDYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARGSYSLSLDWSYRGQGTLVTVS
```

### 7. E09_L11_R1

原top30排名：9；本筛选清单排名：7。

- 设计来源：8Z8M；输入热点：B:87Y、B:90K、C:146E。选择理由：TNF30/ozoralizumab 相关 VHH 来源，选择 B:Y87/K90 与 C:E146 的跨亚基组合；骨架来源与表位来源相容性作为待检验假设。
- CDR1/2/3长度：13/10/11；总长117 aa。原RFD4靶点几何异常标记：True。
- OpenDDE：ipTM=0.707845，ipSAE=0.408327，全复合物pLDDT=90.44（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.180391，ipSAE=0.000000，VHH平均pLDDT=80.21。
- 物理初筛：邻亚基最近重原子距离6.746 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=14/5。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 96]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E09_L11_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E09_L11_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E09_L11_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E09_L11_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E09_L11_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E09_L11_R1/rfd4_provenance.json)

```fasta
>E09_L11_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYYMTWVRQAPGKGLEWVSYINTGGSYTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARAYSPYHYPYRGQGTLVTVS
```

### 8. E03_L18_R2

原top30排名：11；本筛选清单排名：8。

- 设计来源：5M2M；输入热点：A:21Q、A:23E、A:67Q。选择理由：来源为 VHH 复合物；选取该表位的一侧极性接触，避免强迫一个 CDR 同时覆盖整个接触面。
- CDR1/2/3长度：13/9/18；总长123 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.706827，ipSAE=0.380274，全复合物pLDDT=91.11（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.156469，ipSAE=0.000000，VHH平均pLDDT=85.34。
- 物理初筛：邻亚基最近重原子距离8.190 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=18/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置[105]；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E03_L18_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E03_L18_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E03_L18_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E03_L18_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E03_L18_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E03_L18_R2/rfd4_provenance.json)

```fasta
>E03_L18_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSVITTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARWDGTNFYNITPDDFTYRGQGTLVTVS
```

### 9. E06_L13_R1

原top30排名：12；本筛选清单排名：9。

- 设计来源：5M2J；输入热点：A:75L、A:97I、A:135E、A:137N。选择理由：以 L75/I97 疏水接触配合 E135/N137 极性边缘，测试同一已验证 VHH 表位的互补子区域。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.694274，ipSAE=0.338548，全复合物pLDDT=88.61（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.188267，ipSAE=0.000000，VHH平均pLDDT=84.09。
- 物理初筛：邻亚基最近重原子距离0.116 Å；碰撞涉及33个VHH残基；原预测界面CDR/框架接触残基数=18/6。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E06_L13_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E06_L13_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E06_L13_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E06_L13_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E06_L13_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E06_L13_R1/rfd4_provenance.json)

```fasta
>E06_L13_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMSWVRQAPGKGLEWVSAISTGGSTDYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARTSSFYDYDYSYRGQGTLVTVS
```

### 10. E07_L11_R2

原top30排名：13；本筛选清单排名：10。

- 设计来源：5M2I；输入热点：B:90K、B:92N、C:146E。选择理由：该 VHH 原坐标跨两个 TNF 亚基；B:K90/N92 与相邻 C:E146 形成跨亚基锚点，保留三聚体几何。
- CDR1/2/3长度：13/9/11；总长116 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.648839，ipSAE=0.308827，全复合物pLDDT=90.23（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.152793，ipSAE=0.000000，VHH平均pLDDT=83.75。
- 物理初筛：邻亚基最近重原子距离7.270 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=17/1。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E07_L11_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E07_L11_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E07_L11_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E07_L11_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E07_L11_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E07_L11_R2/rfd4_provenance.json)

```fasta
>E07_L11_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSSISTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARDASSSFWDYRGQGTLVTVS
```

### 11. E02_L19_R1

原top30排名：15；本筛选清单排名：11。

- 设计来源：5YOY；输入热点：A:23E、A:67Q、A:138R。选择理由：从同一表位中选择较精简的三点约束，包含酸性、酰胺和碱性侧链，检验减少热点是否改善 VHH 可接近性。
- CDR1/2/3长度：13/9/19；总长124 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.668585，ipSAE=0.289819，全复合物pLDDT=90.79（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.176429，ipSAE=0.000000，VHH平均pLDDT=82.69。
- 物理初筛：邻亚基最近重原子距离0.539 Å；碰撞涉及2个VHH残基；原预测界面CDR/框架接触残基数=14/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E02_L19_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E02_L19_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E02_L19_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E02_L19_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E02_L19_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E02_L19_R1/rfd4_provenance.json)

```fasta
>E02_L19_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAISYGGTTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARSLYYYVSSSLTADDFDYRGQGTLVTVS
```

### 12. E04_L13_R2

原top30排名：17；本筛选清单排名：12。

- 设计来源：5M2M；输入热点：A:67Q、A:138R、A:140D、A:141Y。选择理由：覆盖 Q67 及 C 端 R138/D140/Y141 接触簇，兼顾带电与芳香锚点，检验与同来源另一侧热点的差异。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.653834，ipSAE=0.265778，全复合物pLDDT=87.69（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.182266，ipSAE=0.000000，VHH平均pLDDT=80.71。
- 物理初筛：邻亚基最近重原子距离2.031 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=16/7。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E04_L13_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E04_L13_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E04_L13_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E04_L13_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E04_L13_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E04_L13_R2/rfd4_provenance.json)

```fasta
>E04_L13_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYDMGWVRQAPGKGLEWVSSISTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARYDPYSSFWYPYRGQGTLVTVS
```

### 13. E09_L19_R1

原top30排名：18；本筛选清单排名：13。

- 设计来源：8Z8M；输入热点：B:87Y、B:90K、C:146E。选择理由：TNF30/ozoralizumab 相关 VHH 来源，选择 B:Y87/K90 与 C:E146 的跨亚基组合；骨架来源与表位来源相容性作为待检验假设。
- CDR1/2/3长度：13/9/19；总长124 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.625920，ipSAE=0.241849，全复合物pLDDT=90.59（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.178882，ipSAE=0.000000，VHH平均pLDDT=80.85。
- 物理初筛：邻亚基最近重原子距离7.970 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=10/2。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E09_L19_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E09_L19_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E09_L19_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E09_L19_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E09_L19_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E09_L19_R1/rfd4_provenance.json)

```fasta
>E09_L19_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSVITTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARSNYYYSSAPSYEPYFDYRGQGTLVTVS
```

### 14. E10_L20_R2

原top30排名：19；本筛选清单排名：14。

- 设计来源：21TW_VHH1；输入热点：B:89T、B:92N、C:146E、C:147S。选择理由：独立复合物中的 VHH1 接触提供跨亚基证据；选 B:T89/N92 与 C:E146/S147 的极性子簇，比较与 8Z8M 组合的鲁棒性。
- CDR1/2/3长度：13/10/20；总长126 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.609216，ipSAE=0.236195，全复合物pLDDT=87.62（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.784952，ipSAE=0.528231，VHH平均pLDDT=92.32。
- 物理初筛：邻亚基最近重原子距离4.082 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=16/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 96]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E10_L20_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E10_L20_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E10_L20_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E10_L20_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E10_L20_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E10_L20_R2/rfd4_provenance.json)

```fasta
>E10_L20_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMGWVRQAPGKGLEWVSGISYSGGYTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARDVYAYTSSPYSDPSYYDYRGQGTLVTVS
```

### 15. E08_L18_R1

原top30排名：20；本筛选清单排名：15。

- 设计来源：5M2I；输入热点：B:137N、B:138R、C:115Y。选择理由：5M2I 跨亚基接触中的 B:N137/R138 与相邻 C:Y115 构成紧凑极性/带电/芳香簇；初稿 Q88/I97/Y115 跨度约 30 Å，提交前改为本组合（最大约 14 Å），降低空间上过度分散的约束。
- CDR1/2/3长度：13/9/18；总长123 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.605681，ipSAE=0.224178，全复合物pLDDT=89.50（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.207944，ipSAE=0.000000，VHH平均pLDDT=85.35。
- 物理初筛：邻亚基最近重原子距离2.643 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=11/9。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E08_L18_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E08_L18_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E08_L18_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E08_L18_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E08_L18_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E08_L18_R1/rfd4_provenance.json)

```fasta
>E08_L18_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMAWVRQAPGKGLEWVSAITTGGTTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCAKSSYPAYVYTDPDYYPYRGQGTLVTVS
```

### 16. E06_L14_R1

原top30排名：21；本筛选清单排名：16。

- 设计来源：5M2J；输入热点：A:75L、A:97I、A:135E、A:137N。选择理由：以 L75/I97 疏水接触配合 E135/N137 极性边缘，测试同一已验证 VHH 表位的互补子区域。
- CDR1/2/3长度：13/9/14；总长119 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.616798，ipSAE=0.223986，全复合物pLDDT=86.92（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.183788，ipSAE=0.000000，VHH平均pLDDT=79.77。
- 物理初筛：邻亚基最近重原子距离0.206 Å；碰撞涉及32个VHH残基；原预测界面CDR/框架接触残基数=16/6。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E06_L14_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E06_L14_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E06_L14_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E06_L14_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E06_L14_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E06_L14_R1/rfd4_provenance.json)

```fasta
>E06_L14_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAISSGGSTDYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCAKNDIFYGSSSYDYRGQGTLVTVS
```

### 17. E06_L15_R1

原top30排名：23；本筛选清单排名：17。

- 设计来源：5M2J；输入热点：A:75L、A:97I、A:135E、A:137N。选择理由：以 L75/I97 疏水接触配合 E135/N137 极性边缘，测试同一已验证 VHH 表位的互补子区域。
- CDR1/2/3长度：13/9/15；总长120 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.511156，ipSAE=0.201049，全复合物pLDDT=90.00（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.179521，ipSAE=0.000000，VHH平均pLDDT=81.70。
- 物理初筛：邻亚基最近重原子距离7.754 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=12/3。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E06_L15_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E06_L15_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E06_L15_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E06_L15_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E06_L15_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E06_L15_R1/rfd4_provenance.json)

```fasta
>E06_L15_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAISTGGSTNYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARSSALYTDPRYFDYRGQGTLVTVS
```

### 18. E08_L13_R1

原top30排名：24；本筛选清单排名：18。

- 设计来源：5M2I；输入热点：B:137N、B:138R、C:115Y。选择理由：5M2I 跨亚基接触中的 B:N137/R138 与相邻 C:Y115 构成紧凑极性/带电/芳香簇；初稿 Q88/I97/Y115 跨度约 30 Å，提交前改为本组合（最大约 14 Å），降低空间上过度分散的约束。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.579049，ipSAE=0.196366，全复合物pLDDT=87.95（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.174823，ipSAE=0.000000，VHH平均pLDDT=82.11。
- 物理初筛：邻亚基最近重原子距离7.287 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=15/2。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E08_L13_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E08_L13_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E08_L13_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E08_L13_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E08_L13_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E08_L13_R1/rfd4_provenance.json)

```fasta
>E08_L13_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYGMSWVRQAPGKGLEWVSAISTGGSTDYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARGYYFYPSYFDYRGQGTLVTVS
```

### 19. E07_L16_R1

原top30排名：25；本筛选清单排名：19。

- 设计来源：5M2I；输入热点：B:90K、B:92N、C:146E。选择理由：该 VHH 原坐标跨两个 TNF 亚基；B:K90/N92 与相邻 C:E146 形成跨亚基锚点，保留三聚体几何。
- CDR1/2/3长度：13/10/16；总长122 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.526755，ipSAE=0.173897，全复合物pLDDT=88.90（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.475323，ipSAE=0.016784，VHH平均pLDDT=87.54。
- 物理初筛：邻亚基最近重原子距离5.251 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=18/2。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 96]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E07_L16_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E07_L16_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E07_L16_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E07_L16_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E07_L16_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E07_L16_R1/rfd4_provenance.json)

```fasta
>E07_L16_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMGWVRQAPGKGLEWVSAISSSGGTTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARVSYFSSSTVYFYDDRGQGTLVTVS
```

### 20. E10_L11_R1

原top30排名：26；本筛选清单排名：20。

- 设计来源：21TW_VHH1；输入热点：B:89T、B:92N、C:146E、C:147S。选择理由：独立复合物中的 VHH1 接触提供跨亚基证据；选 B:T89/N92 与 C:E146/S147 的极性子簇，比较与 8Z8M 组合的鲁棒性。
- CDR1/2/3长度：13/10/11；总长117 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.602298，ipSAE=0.173454，全复合物pLDDT=90.49（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.618671，ipSAE=0.310174，VHH平均pLDDT=90.62。
- 物理初筛：邻亚基最近重原子距离0.925 Å；碰撞涉及3个VHH残基；原预测界面CDR/框架接触残基数=14/0。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 96]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E10_L11_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E10_L11_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E10_L11_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E10_L11_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E10_L11_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E10_L11_R1/rfd4_provenance.json)

```fasta
>E10_L11_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSDYGMTWVRQAPGKGLEWVSYITWSGGSTYYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARADSPYSYDYRGQGTLVTVS
```

### 21. E04_L17_R1

原top30排名：27；本筛选清单排名：21。

- 设计来源：5M2M；输入热点：A:67Q、A:138R、A:140D、A:141Y。选择理由：覆盖 Q67 及 C 端 R138/D140/Y141 接触簇，兼顾带电与芳香锚点，检验与同来源另一侧热点的差异。
- CDR1/2/3长度：13/9/17；总长122 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.570826，ipSAE=0.168414，全复合物pLDDT=89.23（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.161382，ipSAE=0.000000，VHH平均pLDDT=83.17。
- 物理初筛：邻亚基最近重原子距离0.240 Å；碰撞涉及40个VHH残基；原预测界面CDR/框架接触残基数=18/5。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置[100]；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E04_L17_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E04_L17_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E04_L17_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E04_L17_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E04_L17_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E04_L17_R1/rfd4_provenance.json)

```fasta
>E04_L17_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAISTGGSTSYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARRKNGSLYYYADDFDYRGQGTLVTVS
```

### 22. E01_L13_R2

原top30排名：28；本筛选清单排名：22。

- 设计来源：5YOY；输入热点：A:67Q、A:104E、A:107E、A:138R。选择理由：Q67/E104/E107/R138 覆盖已报道接触面中央的极性与带电残基；保留原首批组合以作不同随机种子的重复对照。
- CDR1/2/3长度：13/9/13；总长118 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.596226，ipSAE=0.164186，全复合物pLDDT=89.19（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.180855，ipSAE=0.000000，VHH平均pLDDT=83.25。
- 物理初筛：邻亚基最近重原子距离5.536 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=6/3。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E01_L13_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E01_L13_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E01_L13_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E01_L13_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E01_L13_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E01_L13_R2/rfd4_provenance.json)

```fasta
>E01_L13_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAISTGGSTSYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARYVYANYHYYDYRGQGTLVTVS
```

### 23. E03_L17_R1

原top30排名：29；本筛选清单排名：23。

- 设计来源：5M2M；输入热点：A:21Q、A:23E、A:67Q。选择理由：来源为 VHH 复合物；选取该表位的一侧极性接触，避免强迫一个 CDR 同时覆盖整个接触面。
- CDR1/2/3长度：13/9/17；总长122 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.536705，ipSAE=0.159910，全复合物pLDDT=89.75（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.164969，ipSAE=0.000000，VHH平均pLDDT=84.68。
- 物理初筛：邻亚基最近重原子距离5.671 Å；碰撞涉及0个VHH残基；原预测界面CDR/框架接触残基数=17/2。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置无；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E03_L17_R1/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E03_L17_R1/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E03_L17_R1/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E03_L17_R1/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E03_L17_R1/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E03_L17_R1/rfd4_provenance.json)

```fasta
>E03_L17_R1
EVQLVESGGGLVQPGGSLRLSCAASGFTFSSYAMGWVRQAPGKGLEWVSAITSGGYTSYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARLKSGADYTSADNYDYRGQGTLVTVS
```

### 24. E04_L16_R2

原top30排名：30；本筛选清单排名：24。

- 设计来源：5M2M；输入热点：A:67Q、A:138R、A:140D、A:141Y。选择理由：覆盖 Q67 及 C 端 R138/D140/Y141 接触簇，兼顾带电与芳香锚点，检验与同来源另一侧热点的差异。
- CDR1/2/3长度：13/9/16；总长121 aa。原RFD4靶点几何异常标记：False。
- OpenDDE：ipTM=0.545969，ipSAE=0.132113，全复合物pLDDT=87.02（非VHH单链指标）。
- 原ESMFold2三聚体：VHH–TNF ipTM（原表方向最大）=0.178925，ipSAE=0.000000，VHH平均pLDDT=82.60。
- 物理初筛：邻亚基最近重原子距离0.291 Å；碰撞涉及31个VHH残基；原预测界面CDR/框架接触残基数=20/5。
- 自治性：未评估；序列成药性综合评分：未计算。序列初筛：Cys共2个，位置[22, 95]；N-X-S/T基序起始位置[108]；最长同残基连续段为GGG（位置8–10）。Cys计数不证明二硫键已正确形成，基序存在不证明实际发生糖基化。

[RFD4设计CIF](designs/E04_L16_R2/rfd4_design.cif) · [OpenDDE原始复合物CIF](designs/E04_L16_R2/opendde_monomer_complex.cif) · [OpenDDE置信度JSON](designs/E04_L16_R2/opendde_summary_confidence.json) · [ESMFold2三聚体预测CIF](designs/E04_L16_R2/esmfold2_trimer_complex.cif) · [三聚体刚体叠合PDB](designs/E04_L16_R2/placement_on_A.pdb) · [RFD4输入与运行溯源](designs/E04_L16_R2/rfd4_provenance.json)

```fasta
>E04_L16_R2
EVQLVESGGGLVQPGGSLRLSCAASGFTFSNYAMGWVRQAPGKGLEWVSAISSGGSTDYPDSVKGRFTISRDNAKNTLYLQMNSLRPEDTAVYYCARQSSDGGLSADNFSYRGQGTLVTVS
```

## 可复现输入与汇总材料

[保留24条指标及序列CSV](data/remaining24.csv)

[保留24条FASTA](data/remaining24.fasta)

[用户确认的剔除清单](data/selection.json)

[表位与设计策略原始清单](data/epitope_portfolio.json)

[保留24条OpenDDE输入JSON](inputs/opendde_selected24.json)

[实际权重哈希](data/checkpoint.sha256)

[保留24条评分](data/remaining24.csv)

[保留24条的72次叠合及核心拟合指标](data/trimer_alignment_24.json)

[含原始及叠合结构的PyMOL会话](visualization/selected24_raw_and_trimer.pse)

参考结构：[RCSB 1TNF](https://www.rcsb.org/structure/1TNF)。ipSAE参考实现：[DunbrackLab/IPSAE](https://github.com/DunbrackLab/IPSAE)。本报告所有数值来自本地已完成运行或明确列出的序列初筛；没有额外启动设计或折叠预测。


## 本上传包说明

本包内结构保持原始坐标，文件重命名便于浏览。结构及原始置信度文件与本地源文件逐字节校验；完整SHA256清单见[SHA256SUMS.txt](SHA256SUMS.txt)。运行溯源JSON保留模型版本及请求参数，移除了部署机器路径与进程标识。原始PAE/full-data文件及模型权重未包含；已计算的ipSAE在评分表中，不应只凭summary_confidence重新推算ipSAE。
