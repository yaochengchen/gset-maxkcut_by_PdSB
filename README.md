# Max-k-Cut solution certificates on the G-set benchmark

[中文说明](README.zh-CN.md) · [Results paper](paper/main.pdf) · [All results](results.csv)

[GitHub repository](https://github.com/yaochengchen/gset-maxkcut_by_PdSB) ·
[Dataset DOI: 10.5281/zenodo.23405852](https://doi.org/10.5281/zenodo.23405852)

This release publishes **35 explicit vertex partitions on 10 G-set graphs**, with
verification code and a short paper. It is a release of solutions and their
certificates; the solution-generation algorithms are outside its scope.

The comparison categories, based on the literature checked on October 15, 2026, are:

- **7 improvements** over the published best-known values identified.
- **17 matches**, including **3 provably optimal** all-edges-cut partitions.
- **10 reference values** for G1–G5 with k = 6 and k = 8, without an identified directly comparable published baseline.
- **1 additional partition**, G81 with k = 4, whose value 16666 is below the published Gurobi value 16670.

## Seven improved values

| Graph | k | This release | Published comparison | Gain | Comparison source |
|---|---:|---:|---:|---:|---|
| G81 | 3 | **16510** | 16392 | +118 | Garvardt et al. 2026, Table 7, LS |
| G22 | 4 | **18787** | 18776 | +11 | MOH 2017; PEAF 2026, Table 20 |
| G55 | 3 | **12437** | 12432 | +5 | Garvardt et al. 2026, Table 7, ILP; PAMC 2026, Table S2 |
| G1 | 5 | **17706** | 17705 | +1 | PEAF 2026, Table 21 |
| G2 | 5 | **17710** | 17707 | +3 | PEAF 2026, Table 21 |
| G3 | 5 | **17705** | 17703 | +2 | PEAF 2026, Table 21 |
| G4 | 5 | **17710** | 17709 | +1 | MOH 2017; PEAF 2026, Table 21 |

These are feasible values and lower bounds on the corresponding optima.
None of these seven partitions is claimed to be globally optimal.

## Matched values

| Graph | k | Cut values | Note |
|---|---|---|---|
| G1–G5 | 3 | 15165, 15172, 15173, 15184, 15193 | MOH / PEAF; graph order |
| G1–G5 | 4 | 16804, 16810, 16806, 16814, 16816 | PEAF; graph order |
| G5 | 5 | 17711 | PEAF |
| G22 | 3 | 17169 | PEAF / PAMC |
| G43 | 3, 4 | 8573, 9377 | MOH; Garvardt et al. / PEAF |
| G55 | 4 | 12498 | All edges cut; optimal |
| G70 | 3, 4 | 9999, 9999 | Both all edges cut; optimal |

The three optimal cases are part of the 17 matches. Their graphs have only
positive unit weights, and the certificates attain the upper bound given by the
total edge weight.

## Reference values

| k | G1 | G2 | G3 | G4 | G5 |
|---:|---:|---:|---:|---:|---:|
| 6 | 18255 | 18257 | 18254 | 18257 | 18258 |
| 8 | 18843 | 18844 | 18843 | 18844 | 18845 |

These values are reference points. No priority claim is made for them.

## Additional G81 partition

`solutions/G81_max4cut_solution.txt` has value **16666**. Shao and Wu's ODE
paper reports **16670** in the **Gurobi column of Table 3 (Max-4-Cut)** [5].
The release therefore classifies this partition as `below published best`,
with a difference of **−4**, rather than as an improvement or match.

## Objective and file format

For labels x in {0, …, k−1}, the objective is the signed sum

```text
cut(x) = sum(w_ij for edges (i, j) whose endpoint labels differ).
```

G81 has 20000 vertices, 40000 edges and 19983 negative edges. Its cut value
is a signed weight sum, not a count of crossing edges. The other nine graphs
in this release have only +1 edge weights.

Each solution file contains one integer per line. Line i is the part of vertex i
in the original G-set numbering, starting at vertex 1. Every partition uses all
k labels. `results.csv` includes sizes, cut values, comparison sources and the
SHA-256 digest of each solution file.

## Verification

Python 3 is sufficient; no third-party packages are required. From the repository root:

```text
python verify.py
python verify_independent.py
```

Both commands can download the original graph files from the
[Stanford G-set directory](https://web.stanford.edu/~yyye/yyye/Gset/).
For local graph files named `G1`, `G1.txt`, etc.:

```text
python verify.py --graph-dir /path/to/gset
python verify_independent.py --graph-dir /path/to/gset
```

The first verifier checks graph and solution hashes, label validity, and the
recomputed objective. The independently written second checker also verifies
that no single-vertex move improves a partition and confirms the three
all-edges-cut optimality certificates. Single-vertex local optimality does not
imply global optimality. These programs verify certificate arithmetic and
comparison differences; published baseline values are metadata, not a
literature search performed by the programs.

Saved verification results are in `verification/`. Both verifiers run from a
standalone checkout or extracted release; historical archives are not required.

## Contents

```text
solutions/                         35 vertex partitions
results.csv                        values, comparison categories and solution hashes
verify.py                          objective and integrity verifier
verify_independent.py              independent objective and local-move checker
verification/graph_sha256.json      hashes of the ten original graphs
verification/baseline_sources.csv   comparison sources and table locations
verification/independent_audit.csv  per-partition verification results
verification/verify.log             objective-verifier log
verification/independent_verify.log independent-checker log
paper/main.pdf                     short results preprint
paper/main.tex                     standalone LaTeX source with embedded certificates
CITATION.cff                       dataset citation metadata
.zenodo.json                       metadata for Zenodo/GitHub archiving
LICENSE                            data/document and verification-code licenses
```

The paper embeds the seven improved partitions in hexadecimal form. Its source
is standalone and compiles with `pdflatex main.tex` run twice from `paper/`.

## References

1. F. Ma and J.-K. Hao. A multiple search operator heuristic for the max-k-cut problem. *Annals of Operations Research* 248:365–403, 2017. [DOI](https://doi.org/10.1007/s10479-016-2234-0). Numerical comparisons use the [accepted journal manuscript](https://leria-info.univ-angers.fr/~jinkao.hao/papers/MaHaoAOR2016.pdf).
2. J. Garvardt, N. Grüttemeier, C. Komusiewicz and N. Morawietz. Parameterized Local Search for Max c-Cut. *Annals of Operations Research*, 2026. [DOI](https://doi.org/10.1007/s10479-026-07426-0). Published September 17, 2026.
3. S. Shao and C. Yang. A Parallel Evolutionary Algorithm Framework for Graph k-CUT Problems. *Journal of Intelligent Algorithms and Scientific Computing*, online first, 2026. [DOI](https://doi.org/10.4208/jiasc.2026-194-1). Published September 28, 2026; [arXiv](https://arxiv.org/abs/2607.17158).
4. N. Khan, R. Redding and N. Shukla. Leveraging Population Dynamics to Steer Efficient Search in Large-Scale Combinatorial Optimization. [arXiv:2606.25203v1](https://arxiv.org/abs/2606.25203), 2026.
5. S. Shao and Y. Wu. An ODE Approach to Multiple Choice Polynomial Programming. *East Asian Journal on Applied Mathematics* 15(1):1–28, 2025. [DOI](https://doi.org/10.4208/eajam.2023-184.151023); [arXiv v2](https://arxiv.org/html/2212.06371v2).

## Citation and license

Use `CITATION.cff` to cite the dataset, version 2.0.0:

Chen, Yaocheng. *Max-k-Cut solution certificates on the G-set benchmark*,
version 2.0.0. Zenodo. https://doi.org/10.5281/zenodo.23405852.

The DOI is reserved for this release and becomes active when its Zenodo record
is published. The data, paper and documentation
are CC BY 4.0; the two verification programs are MIT licensed. Original graph
files are not included. See `LICENSE`.

## Author and funding

Yaocheng Chen, Instituto de Astronomia, Geofísica e Ciências Atmosféricas
(IAG-USP), Universidade de São Paulo, Rua do Matão 1226, São Paulo,
SP 05508-090, Brazil. Contact: **yaochengchen@usp.br**.

Support: FAPESP grants **25/25586-5** and **2021/02120-0**.
