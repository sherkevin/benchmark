# Benchmark document export manifest

- Source: /Users/jingwu/work/benchmark
- Destination repository: https://github.com/sherkevin/benchmark
- Export type: exhaustive document and paper-material snapshot
- Included material files: 15287
- Included material bytes: 1106087027
- Generated audit files: DOCUMENT_EXPORT_MANIFEST.md, DOCUMENT_EXPORT_INVENTORY.tsv, DOCUMENT_EXPORT_EXCLUSIONS.tsv, DOCUMENT_REDACTIONS.tsv
- Redacted source paths: 9 (path-preserving; values removed)
- Core source coverage: 7121 Markdown, 292 PDF, 171 TeX; audit confirmed zero missing paths

## Included formats

All readable document and paper-material formats from the source tree are included: Markdown, PDF, TeX, text, LaTeX support files, HTML/CSS, images, Office files, XML, diagrams, patches, checksums, and other explicitly document-oriented assets. Core Markdown/PDF/TeX files are included even under archive, log-named, virtual-environment, or hidden working directories.

## Excluded

- Git metadata and ignore-rule files.
- Log/runtime formats: .log, .out, .err, .stdout, .stderr, .jsonl, .ndjson, LaTeX build logs, terminal recordings, and videos.
- Source-code formats and notebooks.
- Model/checkpoint/binary formats and model tokenizer/config data.
- Machine configuration and raw dataset formats: YAML/YML, TOML, INI/CFG/CONF, CSV/TSV, ARFF, SDF/SMI, FASTA, InChI, and TTL.
- Environment code and runtime artifacts; document files inside those directories remain included when they match the requested document extensions.
- Secret-bearing files: original values never leave the Mac; the same relative paths are represented by redacted copies.
- Files at or above 90 MiB.

## Counts by extension

| Extension | Files | Bytes |
|---|---:|---:|
| .md | 7121 | 39018979 |
| .txt | 4205 | 50703009 |
| .png | 2496 | 425019492 |
| <no_ext> | 341 | 103702810 |
| .pdf | 292 | 426945738 |
| .tex | 171 | 3643388 |
| .aux | 133 | 1042993 |
| .html | 109 | 12979844 |
| .bbl | 89 | 774044 |
| .jpg | 62 | 17299111 |
| .css | 61 | 402459 |
| .mdx | 51 | 321726 |
| .svg | 33 | 373565 |
| .diff | 27 | 386332 |
| .xml | 19 | 666591 |
| .xlsx | 10 | 680494 |
| .qmd | 8 | 29487 |
| .bib | 7 | 55585 |
| .sty | 6 | 78088 |
| .sha256 | 5 | 610 |
| .before | 4 | 60492 |
| .bst | 4 | 192675 |
| .docx | 4 | 4003186 |
| .md5 | 4 | 3098 |
| .patch | 4 | 457043 |
| .cdxml | 3 | 43781 |
| .gif | 3 | 15651185 |
| .rst | 3 | 107708 |
| .template | 3 | 1283 |
| .after | 2 | 50779 |
| .jpeg | 2 | 215144 |
| .cls | 1 | 122209 |
| .eps | 1 | 59921 |
| .example | 1 | 555 |
| .excalidraw | 1 | 35671 |
| .xls | 1 | 957952 |

## Audit files

The exact included-path list is in DOCUMENT_EXPORT_INVENTORY.tsv. The candidate-level exclusion list is in DOCUMENT_EXPORT_EXCLUSIONS.tsv. Redacted paths and reasons are in DOCUMENT_REDACTIONS.tsv.
