<!-- 本文件为一手证据留档：一字未改地保存 The Playground 平台一道真实题目的题面全文。
     取自 GET /api/challenges/3d-refractive-index-reconstruction-with-non-paraxi-d8d9babd
     取证时间 2026-07-31 ｜ 原文字符数 12,179 ｜ 该题当时已有 357 次作答
     解读与通俗解释见同级《Playground平台造题与判分机制.md》第二节，本文件不加任何注释。 -->

# Three-dimensional refractive index reconstruction from eight-angle complex field measurements

## Problem background

Optical diffraction tomography recovers the three-dimensional refractive index distribution of a semi-transparent specimen from the light it transmits under illumination from several directions. The inversion is only ever as good as the model of propagation it is built on. When a specimen is thin and weakly refracting, the field that reaches the detector can be treated as having scattered once and the relation between index and field is close to linear. That treatment degrades as the specimen thickens and as the illumination and collection angles widen: the wave is scattered repeatedly on its way through, and its wavefront departs from the small-angle regime in which the simplest propagators are derived. A second limitation is geometric rather than physical. Illumination from a finite set of directions leaves a cone of axial spatial frequencies unsampled, so axial features are recovered more poorly than lateral ones however good the model and however long the optimisation runs, and any honest reconstruction carries that signature.

## Approach

The specimen occupies 308 axial slices on a 256 by 256 lateral grid of isotropic 0.1 micrometre voxels, and is described by a real, non-negative refractive index contrast relative to the background. It is thick on the scale of the wavelength and its contrast is large enough that a wave crossing it is scattered more than once.

Eight plane waves illuminate it in turn. For measurement index m from 0 to 7 the transverse wavevector lies on the ring at the objective numerical aperture with azimuth 2*pi*m/8 + pi/8, and the transverse wavevector is (NA/n0) times (cos azimuth, sin azimuth). Index m indexes the first axis of the measurement array, and the fields you report must use the same correspondence. Each incident wave is exactly periodic on the 256 by 256 sampling window.

Each measurement is the complex field, amplitude and phase together, that the objective forms on the plane through the middle of the volume, after a pupil that passes transverse spatial frequencies out to the numerical aperture and rejects the rest.

Recover the contrast by driving down, over non-negative volumes, the per-pixel mean squared modulus of the difference between the fields your forward model predicts and the measured fields, summed over the eight illuminations. Physical parameters: vacuum wavelength 0.5 micrometre, background refractive index 1.0, numerical aperture 0.65.

How you model the propagation through the specimen, and how you carry out the minimisation, are yours to choose. Whatever you build has to be differentiable with respect to the contrast volume, or you will need another way to obtain a descent direction through 308 slices.

## Reproduction target

Recover the three-dimensional refractive index contrast volume that explains the eight measured complex fields, and report the fields your recovered volume predicts together with the misfit history of the optimisation.

Scope: One inversion of the supplied eight-illumination measurement set, at the full 308 by 256 by 256 volume resolution. No denoising network, learned prior or external training data is involved.

## Workflow steps

1. Build a forward model for the measurement described above
   - Role: process
   - Action: Construct a propagation model that takes an index contrast volume to the eight complex fields, faithful to the specimen thickness, the contrast and the angular range stated above, and differentiable with respect to the volume.
   - Evidence: The fields predicted by the final volume, written at step s2, match the measurements to the accuracy the optimisation reached.

2. Report the fields predicted by your recovered volume
   - Role: scored
   - Action: Run the forward model once on your final index contrast volume and save the eight complex fields it predicts, in the same illumination order as the measurements.
   - Output file: `/app/outputs/predicted_fields.npy`
   - Format: npy
   - Contract: Complex fields predicted by the final volume, complex array of shape (8, 256, 256), illumination order matching the measurements.
   - Scoring: metric_recompute

3. Recover the refractive index contrast volume
   - Role: scored
   - Action: Minimise the complex-field misfit over non-negative index contrast and save the final volume.
   - Output file: `/app/outputs/reconstruction.npy`
   - Format: npy
   - Contract: Recovered refractive index contrast, real array of shape (308, 256, 256), axial axis first, non-negative everywhere.
   - Scoring: reference_match

4. Record the misfit history
   - Role: scored
   - Action: Record the per-pixel mean squared field misfit, summed over the eight illuminations, at every iteration of the optimisation.
   - Output file: `/app/outputs/loss_history.csv`
   - Format: csv
   - Contract: Misfit at every iteration of the optimisation, one row per iteration.
   - Scoring: structural_audit

5. Declare the solver settings you used
   - Role: scored
   - Action: State which minimisation method you used, how many iterations you ran, whether you enforced non-negativity, and the final misfit your run reached.
   - Output file: `/app/outputs/run_summary.json`
   - Format: json
   - Contract: Solver settings actually used and the final misfit reached.
   - Scoring: structural_audit

## Assets

The workflow depends on the assets below. This manifest names what each one is and
how the workflow uses it; resource discovery and collection are handled separately.

- **Eight-illumination complex field measurement set** (dataset) — The measured complex fields for the eight illumination directions, stored as a complex array of shape (8, 256, 256) under the key 'measurements' of /app/data/raw_data.npz. The first axis is the illumination index m described above. Present in the task environment; no download is required. Every physical parameter needed to model these measurements is stated in this document.

## Output files

Write all artifacts under `/app/outputs`:

- `/app/outputs/predicted_fields.npy`
- `/app/outputs/reconstruction.npy`
- `/app/outputs/loss_history.csv`
- `/app/outputs/run_summary.json`

## Output contract

Every file the hidden verifier reads is described below. Write each file under `/app/outputs` and follow its schema exactly.

### predicted_fields.npy

- path: `/app/outputs/predicted_fields.npy`
- format: npy
- purpose: scored
- target_policy: metric_recompute
- description: Complex fields predicted by the final volume, complex array of shape (8, 256, 256), illumination order matching the measurements.
- schema:

```json
{
  "dtype": "complex",
  "shape": [
    8,
    256,
    256
  ],
  "units": {
    "field": "complex field amplitude, same normalisation as the measurements"
  }
}
```

### reconstruction.npy

- path: `/app/outputs/reconstruction.npy`
- format: npy
- purpose: scored
- target_policy: reference_match
- description: Recovered refractive index contrast, real array of shape (308, 256, 256), axial axis first, non-negative everywhere.
- schema:

```json
{
  "dtype": "real",
  "shape": [
    308,
    256,
    256
  ],
  "units": {
    "delta_n": "dimensionless refractive index contrast relative to the background"
  }
}
```

### loss_history.csv

- path: `/app/outputs/loss_history.csv`
- format: csv
- purpose: process_evidence
- target_policy: structural_audit
- description: Misfit at every iteration of the optimisation, one row per iteration.
- schema:

```json
{
  "required_columns": [
    "iteration",
    "data_residual"
  ],
  "units": {
    "data_residual": "per-pixel mean squared field misfit summed over the eight illuminations",
    "iteration": "count, starting at 1"
  }
}
```

### run_summary.json

- path: `/app/outputs/run_summary.json`
- format: json
- purpose: scored
- target_policy: structural_audit
- description: Solver settings actually used and the final misfit reached.
- schema:

```json
{
  "required": [
    "solver",
    "n_iterations",
    "positivity_enforced",
    "final_data_residual"
  ],
  "units": {
    "final_data_residual": "per-pixel mean squared field misfit summed over the eight illuminations",
    "n_iterations": "count",
    "positivity_enforced": "boolean",
    "solver": "free text naming the minimisation method you used"
  }
}
```

## Self-check before finishing (optional, not scored)

A machine-readable copy of the output contract is below. Before you finish, write and run
a small script that checks every file under `/app/outputs` against it: each declared file
exists, JSON objects contain the required keys, and CSV/TSV files contain the required
columns. Fix any mismatch before finishing.

This checks SHAPE ONLY — files, keys, columns. It does NOT judge scientific correctness,
and passing it does not mean your answer is right.

```json
{
  "outputs": [
    {
      "file": "predicted_fields.npy",
      "format": "npy",
      "purpose": "scored",
      "target_policy": "metric_recompute",
      "schema": {
        "shape": [
          8,
          256,
          256
        ],
        "dtype": "complex",
        "units": {
          "field": "complex field amplitude, same normalisation as the measurements"
        }
      },
      "description": "Complex fields predicted by the final volume, complex array of shape (8, 256, 256), illumination order matching the measurements."
    },
    {
      "file": "reconstruction.npy",
      "format": "npy",
      "purpose": "scored",
      "target_policy": "reference_match",
      "schema": {
        "shape": [
          308,
          256,
          256
        ],
        "dtype": "real",
        "units": {
          "delta_n": "dimensionless refractive index contrast relative to the background"
        }
      },
      "description": "Recovered refractive index contrast, real array of shape (308, 256, 256), axial axis first, non-negative everywhere."
    },
    {
      "file": "loss_history.csv",
      "format": "csv",
      "purpose": "process_evidence",
      "target_policy": "structural_audit",
      "schema": {
        "required_columns": [
          "iteration",
          "data_residual"
        ],
        "units": {
          "iteration": "count, starting at 1",
          "data_residual": "per-pixel mean squared field misfit summed over the eight illuminations"
        }
      },
      "description": "Misfit at every iteration of the optimisation, one row per iteration."
    },
    {
      "file": "run_summary.json",
      "format": "json",
      "purpose": "scored",
      "target_policy": "structural_audit",
      "schema": {
        "required": [
          "solver",
          "n_iterations",
          "positivity_enforced",
          "final_data_residual"
        ],
        "units": {
          "solver": "free text naming the minimisation method you used",
          "n_iterations": "count",
          "positivity_enforced": "boolean",
          "final_data_residual": "per-pixel mean squared field misfit summed over the eight illuminations"
        }
      },
      "description": "Solver settings actually used and the final misfit reached."
    }
  ]
}
```

## How you are scored

The verifier reloads your volume and runs the forward model itself, so the misfit it scores is recomputed from your reconstruction rather than read from your report. It checks that the fields you submitted agree with the ones your volume actually predicts, compares the recovered volume against a withheld reference, and audits the volume for the structural signature a limited-angle inversion must carry, together with non-negativity and the plausibility of the reported support and peak contrast. A volume that is essentially empty scores nothing.

## Constraints

Network access is available for installing packages. The measurement set is already present under /app/data and must not be replaced or regenerated. Write every output to /app/outputs under exactly the filenames listed above. The index contrast must be obtained by optimising the field misfit through your own forward model, not assembled from tabulated or externally obtained values.

