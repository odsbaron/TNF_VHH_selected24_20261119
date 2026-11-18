# TNF-alpha VHH 筛选保留24条

入口：[完整指标、序列、结构链接与方法报告](REPORT.md)。

- `designs/`：24个设计子目录，每条包含RFD4设计CIF、OpenDDE单体复合物CIF、ESMFold2三聚体复合物CIF、A/B/C三种刚体叠合PDB（共144份结构）、评分和运行溯源。
- `data/`：24条评分CSV、FASTA、剔除清单、表位来源、三聚体碰撞与拟合指标。
- `inputs/`：24条OpenDDE原始输入及靶点/框架参考结构。
- `visualization/`：仅保留24条的PyMOL会话，共48个raw/trimer对象，按ipSAE排序；末尾分数依次ipSAE、ipTM，3位有效数字。
- `scripts/ipsae.py`：原评分参考脚本；PAE未收录，不能仅凭此包重新计算ipSAE。
- `SHA256SUMS.txt`：包内文件SHA256清单。

仓库保留原始目录层级，REPORT.md 中的链接均为相对链接。

单体高置信度不等于完整TNF三聚体结合成功；24条中仍有7条明显三聚体冲突、5条局部或边界问题。未开展实验结合验证、VHH自治性测定或综合成药性评分。
