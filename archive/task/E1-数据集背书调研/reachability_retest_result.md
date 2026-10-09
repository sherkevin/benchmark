# 可达性重测原始记录

| 库 | URL | 实测 HTTP 码 | 时间戳 |
|---|---|---|---|
| TDC | https://tdcommons.ai/benchmark/admet_group/overview/ | HTTP 200 (curl rc=0) | 2026-08-06 15:12:09 CST |
| AqSolDB(GitHub) | https://github.com/mcsorkun/AqSolDB | HTTP 200 (curl rc=0) | 2026-08-06 15:12:10 CST |
| AqSolDB(raw csv) | https://raw.githubusercontent.com/mcsorkun/AqSolDB/master/results/data_curated.csv | HTTP 206 (curl rc=0) | 2026-08-06 15:12:11 CST |
| PubChem(PUG REST) | https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/aspirin/cids/JSON | HTTP 200 (curl rc=0) | 2026-08-06 15:12:12 CST |
| ChEMBL(REST API) | https://www.ebi.ac.uk/chembl/api/data/molecule/search?query=aspirin&format=json&limit=1 | HTTP 400 (curl rc=0) | 2026-08-06 15:12:12 CST |
| BindingDB(REST) | https://www.bindingdb.org/rest/getLigandsByUniprots?uniprot=P00533 | HTTP 200 (curl rc=0) | 2026-08-06 15:12:14 CST |
| UniProt(REST) | https://rest.uniprot.org/uniprotkb/P00533.json | HTTP 200 (curl rc=0) | 2026-08-06 15:12:44 CST |
| PDB(RCSB data API) | https://data.rcsb.org/rest/v1/core/entry/5NX2 | HTTP 200 (curl rc=0) | 2026-08-06 15:12:46 CST |
| PDBbind | http://www.pdbbind.org.cn/ | HTTP 200 (curl rc=0) | 2026-08-06 15:12:51 CST |
| ZINC(files.docking.org) | https://files.docking.org/catalogs/source/ | HTTP 200 (curl rc=0) | 2026-08-06 15:12:59 CST |
| 3DSC(raw csv) | https://raw.githubusercontent.com/aimat-lab/3DSC/main/superconductors_3D/data/final/MP/3DSC_MP.csv | HTTP 206 (curl rc=0) | 2026-08-06 15:13:08 CST |
| SuperCon via UCI #464 | https://archive.ics.uci.edu/dataset/464/superconductivty+data | HTTP 200 (curl rc=0) | 2026-08-06 15:13:09 CST |
| UCI作者repo(predict_tc) | https://github.com/khamidieh/predict_tc | HTTP 200 (curl rc=0) | 2026-08-06 15:13:28 CST |
| SuperCon(NIMS) | https://supercon.nims.go.jp/ | HTTP 000 (curl rc=0) | 2026-08-06 15:13:30 CST |
| NIMS MDR | https://mdr.nims.go.jp/ | HTTP 200 (curl rc=0) | 2026-08-06 15:13:30 CST |
| Materials Project API | https://api.materialsproject.org/ | HTTP 403 (curl rc=0) | 2026-08-06 15:13:38 CST |
| OQMD(optimade) | https://oqmd.org/optimade/structures | HTTP 200 (curl rc=0) | 2026-08-06 15:13:42 CST |
| MPDS API | https://api.mpds.io/ | HTTP 200 (curl rc=0) | 2026-08-06 15:13:50 CST |
| JARVIS(GitHub) | https://github.com/usnistgov/jarvis | HTTP 200 (curl rc=0) | 2026-08-06 15:14:02 CST |

## 补测记录（对首次结果存疑项）

| 库 | URL | 实测 | 时间戳 |
|---|---|---|---|
| ChEMBL 主站 | https://www.ebi.ac.uk/chembl/ | HTTP 200（text/html, 612,650 B） | 2026-08-06 15:14:28 CST |
| ChEMBL API status | https://www.ebi.ac.uk/chembl/api/data/status.json | HTTP 200（application/json） | 2026-08-06 15:14:42 CST |
| ChEMBL 旧式 molecule 端点 | https://www.ebi.ac.uk/chembl/api/data/molecule/CHEMBL25/JSON | HTTP 404（旧端点已迁移，主站与 status 均 200） | 2026-08-06 15:14:40 CST |
| OQMD optimade（连测 3 次） | https://oqmd.org/optimade/structures?response_fields=id&page_limit=1 | 200 / 200 / 000（application/json, 1,042 B）——**不稳定** | 2026-08-06 15:15:30–15:15:39 CST |
| OQMD 主站 | https://oqmd.org/ | HTTP 200 | 2026-08-06 15:16:09 CST |

**结论**：ChEMBL 首次 400 系探测路径写法问题，主站/API 状态端点均 200，判为可达；OQMD 判定为「不稳定可达（间歇 000）」，与 task12「000 不可达」相比发生翻转。SuperCon NIMS 主站 000（交互站拦脚本，与 task12 一致），走 MDR 镜像（200）。

测试时间窗：2026-08-06 15:14 CST
