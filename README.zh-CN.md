# G-set Max-k-Cut 最佳分区解与验证证书

[GitHub 仓库](https://github.com/yaochengchen/gset-maxkcut_by_PdSB) ·
[数据集 DOI：10.5281/zenodo.23405852](https://doi.org/10.5281/zenodo.23405852)

本发布包提供 10 张 G-set 图上的 **35 份分区解**、结果论文及验证程序。
求解算法与实现留待单独研究，本次公开的代码只负责验证证书。

截至 2026 年 10 月 15 日核对的文献，结果分为：

- **7 项超过已找到的已发表最佳值**：G81 k=3，G22 k=4，G55 k=3，以及 G1–G4 k=5。
- **17 项持平**，其中 G55 k=4、G70 k=3 和 k=4 共 3 项切开全部正权边，已证明最优。
- **10 项参考值**：G1–G5 的 k=6、8，未找到可直接比较的已发表基线，不作首创性声明。
- **1 项其他有效分区**：G81 k=4 为 16666，低于 Shao 与 Wu 2025 年论文表 3 的 Gurobi 值 16670，因此不列入新纪录或持平项。

完整数值和引用见 [英文说明](README.md)、[results.csv](results.csv)及
[文献基线表](verification/baseline_sources.csv)。

## 结果论文

[论文 PDF](paper/main.pdf)沿用短篇预印本形式，介绍问题、已有基线、结果和证书验证。
附录直接嵌入 7 份改进分区的十六进制编码。作者信息和资助信息按作者提供的论文填写，
没有文末个人传记。论文不报告未公开算法的性能比较。

## 独立验证

在本目录使用 Python 3 运行，无需安装额外依赖：

```text
python verify.py
python verify_independent.py
```

程序自动下载官方 G-set 图并检查哈希；也可以加 `--graph-dir` 指向本地图文件目录。
第二个程序还检查单顶点移动的增益和 3 项最优性证书。
两个程序均可从单独解压后的发布包运行，不依赖原始实验存档或旧版本目录。

G81 含有负权边，目标值按带符号的跨分区边权和计算。局部最优不能替代全局最优性证明。
验证程序不会自动检索或确认文献纪录。

## 作者与许可

Yaocheng Chen，Instituto de Astronomia, Geofísica e Ciências Atmosféricas
(IAG-USP)，Universidade de São Paulo。

地址：Rua do Matão 1226, São Paulo, SP 05508-090, Brazil。
邮箱：**yaochengchen@usp.br**。
资助：FAPESP **25/25586-5**、**2021/02120-0**。

数据、论文和文档采用 CC BY 4.0；两个验证程序采用 MIT 许可。
引用元信息见 `CITATION.cff`，版本为 **2.0.0**。推荐引用：

Chen, Yaocheng. *Max-k-Cut solution certificates on the G-set benchmark*,
version 2.0.0. Zenodo. https://doi.org/10.5281/zenodo.23405852.

本 DOI 已为本次发布预留，在 Zenodo 记录正式发布后生效。
