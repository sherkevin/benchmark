# Figure 2 v1 manifest

- Source: `figure2.html` (inline SVG), canonical extracted asset: `figure2.svg`
- Design spec: `DRAWING_SPEC.md`
- Intended placement: single-column `figure`, target height 1.8–2.2 in
- ViewBox: `0 0 1500 700` (aspect ratio 2.14)
- Semantic colors: ours blue `#2E5DA1`, frozen gray `#EEF1F5`, feedback amber `#FFF2D9/#D99000`, verifier purple `#EEE8F7/#6B4C9A`
- Redundant encodings: fill + border + lane labels + solid/dashed arrows; no red/green dependency
- Accessibility: root SVG has title and description; unselected output is ghosted and crossed out; selected-only boundary is labelled
- Status: v1 layout draft; not yet inserted into `main.tex`
