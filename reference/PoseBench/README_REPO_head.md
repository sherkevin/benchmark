<div align="center">

# PoseBench

[![arXiv](http://img.shields.io/badge/arXiv-2405.14108-B31B1B.svg)](https://arxiv.org/abs/2405.14108)
[![NMI](http://img.shields.io/badge/Nature_Machine_Intelligence-B37A1B.svg)](https://rdcu.be/eW5oj)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19138652.svg)](https://doi.org/10.5281/zenodo.19138652)
[![PyPI version](https://badge.fury.io/py/posebench.svg)](https://badge.fury.io/py/posebench)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Docs](https://assets.readthedocs.org/static/projects/badges/passing-flat.svg)](https://bioinfomachinelearning.github.io/PoseBench/)
<a href="https://hydra.cc/"><img alt="Config: Hydra" src="https://img.shields.io/badge/config-hydra-89b8cd"></a>
<a href="https://github.com/psf/black"><img alt="Code style: black" src="https://img.shields.io/badge/code%20style-black-000000.svg"></a>
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Hugging Face](https://img.shields.io/badge/Hugging%20Face-FFD21E?logo=huggingface&logoColor=000)](https://huggingface.co/papers/2405.14108)

<img src="./img/PoseBench.png" width="600">

</div>

## Description

Comprehensive benchmarking of protein-ligand structure prediction methods

[Documentation](https://bioinfomachinelearning.github.io/PoseBench/)

> **⚠️ Notice:** We have discovered a bug in version `1.0.0` affecting the ligand scoring results of each primary-ligand dataset (Astex Diverse, DockGen-E, and PoseBusters Benchmark). We have addressed this bug in version `1.1.0`, which reports (~15% on average) reduced performance for each method currently in the benchmark. Please rerun your analyses if you have developed code on top of `PoseBench`. Thank you for your understanding!

## Contents

- [Installation](#installation)
- [Tutorials](#tutorials)
- [How to prepare PoseBench data](#how-to-prepare-posebench-data)
- [Available inference methods](#available-inference-methods)
- [How to run a sweep of benchmarking experiments](#how-to-run-a-sweep-of-benchmarking-experiments)
- [How to run inference with individual methods](#how-to-run-inference-with-individual-methods)
- [How to run inference with a method ensemble](#how-to-run-inference-with-a-method-ensemble)
- [How to create comparative plots of inference results](#how-to-create-comparative-plots-of-inference-results)
- [For developers](#for-developers)
- [Acknowledgements](#acknowledgements)
- [Citing this work](#citing-this-work)
- [Bonus](#bonus)

## Installation

<details>

### Portable installation

To reuse modules and utilities within `PoseBench` in other projects, one can simply use `pip`

```bash
pip install posebench
```

### Full installation

To reproduce, customize, or extend the `PoseBench` benchmark, we recommend fully installing `PoseBench` using `mamba` as follows:

First, install `mamba` for dependency management (as a fast alternative to Anaconda)

```bash
wget "https://github.com/conda-forge/miniforge/releases/download/24.11.3-0/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh  # accept all terms and install to the default location
rm Miniforge3-$(uname)-$(uname -m).sh  # (optionally) remove installer after using it
source ~/.bashrc  # alternatively, one can restart their shell session to achieve the same result
```

Install dependencies for each method's environment (as desired)

```bash
# clone project
sudo apt-get install git-lfs  # NOTE: run this if you have not already installed `git-lfs`
git lfs install
git clone https://github.com/BioinfoMachineLearning/PoseBench --recursive
cd PoseBench

# create conda environments (~80 GB total)
# - PoseBench environment # (~15 GB)
mamba env create -f environments/posebench_environment.yaml
conda activate PoseBench  # NOTE: one still needs to use `conda` to (de)activate environments
pip3 install -e .
pip3 install numpy==1.26.4 --no-dependencies
pip3 install prody==2.4.1 --no-dependencies
# - PyMOL environment # (~1 GB)
mamba env create -f environments/pymol_environment.yaml
conda activate PyMOL-PoseBench
pip install -e . --no-deps
# - casp15_ligand_scoring environment (~3 GB)
mamba env create -f environments/casp15_ligand_scoring_environment.yaml
conda activate casp15_ligand_scoring  # NOTE: one still needs to use `conda` to (de)activate environments
# NOTE: must comment out the `posecheck` requirement in `pyproject.toml` beforehand and restore it thereafter
pip3 install -e .
# - DiffDock environment (~13 GB)
mamba env create -f environments/diffdock_environment.yaml --prefix forks/DiffDock/DiffDock/
conda activate forks/DiffDock/DiffDock/  # NOTE: one still needs to use `conda` to (de)activate environments
pip3 install pyg-lib -f https://data.pyg.org/whl/torch-2.1.0+cu118.html
# - FABind environment (~6 GB)
mamba env create -f environments/fabind_environment.yaml --prefix forks/FABind/FABind/
conda activate forks/FABind/FABind/  # NOTE: one still needs to use `conda` to (de)activate environments
# - DynamicBind environment (~13 GB)
mamba env create -f environments/dynamicbind_environment.yaml --prefix forks/DynamicBind/DynamicBind/
conda activate forks/DynamicBind/DynamicBind/ && pip3 install pyg-lib -f https://data.pyg.org/whl/torch-2.1.0+cu118.html  # NOTE: one still needs to use
# - NeuralPLexer environment (~14 GB)
mamba env create -f environments/neuralplexer_environment.yaml --prefix forks/NeuralPLexer/NeuralPLexer/
conda activate forks/NeuralPLexer/NeuralPLexer/  # NOTE: one still needs to use `conda` to (de)activate environments
cd forks/NeuralPLexer/ && pip3 install -e . && cd ../../
# - FlowDock environment (~14 GB)
mamba env create -f environments/flowdock_environment.yaml --prefix forks/FlowDock/FlowDock/
conda activate forks/FlowDock/FlowDock/  # NOTE: one still needs to use `conda` to (de)activate environments
cd forks/FlowDock/ && pip3 install -e . && cd ../../
# - RoseTTAFold-All-Atom environment (~14 GB) - NOTE: after running these commands, follow the installation instructions in `forks/RoseTTAFold-All-Atom/README.md` starting at Step 4 (with `forks/RoseTTAFold-All-Atom/` as the current working directory)
mamba env create -f environments/rfaa_environment.yaml --prefix forks/RoseTTAFold-All-Atom/RFAA/
conda activate forks/RoseTTAFold-All-Atom/RFAA/  # NOTE: one still needs to use `conda` to (de)activate environments
cd forks/RoseTTAFold-All-Atom/rf2aa/SE3Transformer/ && pip3 install --no-cache-dir -r requirements.txt && python3 setup.py install && cd ../../../../
# - Chai-1 environment (~6 GB)
mamba env create -f environments/chai_lab_environment.yaml --prefix forks/chai-lab/chai-lab/
conda activate forks/chai-lab/chai-lab/  # NOTE: one still needs to use `conda` to (de)activate environments
pip3 install forks/chai-lab/
# - Boltz environment (~5 GB)
mamba env create -f environments/boltz_environment.yaml --prefix forks/boltz/boltz/
conda activate forks/boltz/boltz/  # NOTE: one still needs to use `conda` to (de)activate environments
cd forks/boltz/ && pip3 install -e .[cuda] && cd ../../
# - AutoDock Vina Tools environment (~1 GB)
mamba env create -f environments/adfr_environment.yaml --prefix forks/Vina/ADFR/
conda activate forks/Vina/ADFR/  # NOTE: one still needs to use `conda` to (de)activate environments
# - P2Rank (~0.5 GB)
wget -P forks/P2Rank/ https://github.com/rdk/p2rank/releases/download/2.4.2/p2rank_2.4.2.tar.gz
tar -xzf forks/P2Rank/p2rank_2.4.2.tar.gz -C forks/P2Rank/
rm forks/P2Rank/p2rank_2.4.2.tar.gz
```

Download checkpoints (~8.25 GB total)

```bash
# DynamicBind checkpoint (~0.25 GB)
cd forks/DynamicBind/
wget https://zenodo.org/records/10137507/files/workdir.zip
unzip workdir.zip
rm workdir.zip
cd ../../

# NeuralPLexer checkpoint (~6.5 GB)
cd forks/NeuralPLexer/
wget https://zenodo.org/records/10373581/files/neuralplexermodels_downstream_datasets_predictions.zip
unzip neuralplexermodels_downstream_datasets_predictions.zip
rm neuralplexermodels_downstream_datasets_predictions.zip
cd ../../

# FlowDock checkpoint (~2 GB)
cd forks/FlowDock/
wget https://zenodo.org/records/14478459/files/flowdock_checkpoints.tar.gz
tar -xzf flowdock_checkpoints.tar.gz
rm flowdock_checkpoints.tar.gz
cd ../../

# RoseTTAFold-All-Atom checkpoint (~1.5 GB)
cd forks/RoseTTAFold-All-Atom/
wget http://files.ipd.uw.edu/pub/RF-All-Atom/weights/RFAA_paper_weights.pt
cd ../../
```

(Optional) Download PLINDER-based similarity metrics for method generalization analysis (~0.5 GB total)

```bash
mkdir -p ./data/plinder/
wget -P ./data/plinder/ https://zenodo.org/records/16754298/files/annotations.csv
wget -P ./data/plinder/ https://zenodo.org/records/16754298/files/all_similarity_scores.parquet
```

(Optional) Alternatively, download PLINDER to perform a method generalization analysis for custom (new) datasets (~500 GB total)

```bash
# download fixed version of PLINDER
export PLINDER_RELEASE=2024-06
export PLINDER_ITERATION=v2
mkdir -p ./data/plinder/${PLINDER_RELEASE}/${PLINDER_ITERATION}/
gsutil -m cp -r "gs://plinder/${PLINDER_RELEASE}/${PLINDER_ITERATION}/*" ./data/plinder/${PLINDER_RELEASE}/${PLINDER_ITERATION}/

# unpack system files of fixed version of PLINDER
cd ./data/plinder/${PLINDER_RELEASE}/${PLINDER_ITERATION}/systems; for i in `ls *zip`; do unzip $i; touch ${i//.zip/}_done; done
cd ../../../../../

# customize `similarity_scoring.py` to similarity-match a (bespoke) subset of new PDB complex IDs for (blind) benchmarking
python3 posebench/analysis/similarity_scoring.py $PDB_ID_FROM_NEW_SUBSET # e.g., in a for-loop or in parallel

# combine each score of the new subset into a singular (new) `all_similarity_scores.parquet` file
python3 -c "import os, pandas as pd; from glob import glob; files = glob(os.path.join('scoring', 'scores', 'scores', 'all_scores', '*.parquet')); pd.concat([pd.read_parquet(f) for f in files], ignore_index=True).to_parquet('data', 'plinder', 'all_similarity_scores.parquet')"

# update annotations
python3 -c "import re; pdb_ids_pattern = '|'.join(map(re.escape, $PDB_IDS_IN_NEW_SUBSET)); similarity_df_custom = all_similarity_scores[~all_similarity_scores["target_system"].str.contains(pdb_ids_pattern, na=False)].sort_values(by='sucos_shape_pocket_qcov', ascending=False).groupby('group_key').head(1).reset_index(drop=True); similarity_custom = dict(zip(similarity_df_custom['group_key'], similarity_df_custom['sucos_shape_pocket_qcov'])); annotated_df['sucos_shape_pocket_qcov_custom'] = annotated_df['group_key'].map(similarity_custom); annotated_df.to_csv(os.path.join('data', 'plinder', 'annotations.csv'))"
```

</details>

## Tutorials

<details>

We provide a two-part tutorial series of Jupyter notebooks to provide users with examples
of how to extend `PoseBench`, as outlined below.

1. [Adding a new dataset](https://github.com/BioinfoMachineLearning/PoseBench/blob/main/notebooks/adding_new_dataset_tutorial.ipynb)
2. [Adding a new method](https://github.com/BioinfoMachineLearning/PoseBench/blob/main/notebooks/adding_new_method_tutorial.ipynb)

</details>

## How to prepare `PoseBench` data

<details>

### Downloading Astex, PoseBusters, DockGen, and CASP15 data

```bash
# fetch, extract, and clean-up preprocessed Astex Diverse, PoseBusters Benchmark, DockGen, and CASP15 data (~3 GB) #
wget https://zenodo.org/records/19138652/files/astex_diverse_set.tar.gz
wget https://zenodo.org/records/19138652/files/posebusters_benchmark_set.tar.gz
wget https://zenodo.org/records/19138652/files/dockgen_set.tar.gz
wget https://zenodo.org/records/19138652/files/casp15_set.tar.gz
tar -xzf astex_diverse_set.tar.gz
tar -xzf posebusters_benchmark_set.tar.gz
tar -xzf dockgen_set.tar.gz
tar -xzf casp15_set.tar.gz
rm astex_diverse_set.tar.gz
rm posebusters_benchmark_set.tar.gz
rm dockgen_set.tar.gz
rm casp15_set.tar.gz
```

### Downloading benchmark method predictions

```bash
# fetch, extract, and clean-up benchmark method predictions to reproduce paper results (~19 GB) #
# AutoDock Vina predictions and results
wget https://zenodo.org/records/19138652/files/vina_benchmark_method_predictions.tar.gz
tar -xzf vina_benchmark_method_predictions.tar.gz
rm vina_benchmark_method_predictions.tar.gz
# DiffDock predictions and results
wget https://zenodo.org/records/19138652/files/diffdock_benchmark_method_predictions.tar.gz
tar -xzf diffdock_benchmark_method_predictions.tar.gz
rm diffdock_benchmark_method_predictions.tar.gz
# DynamicBind predictions and results
wget https://zenodo.org/records/19138652/files/dynamicbind_benchmark_method_predictions.tar.gz
tar -xzf dynamicbind_benchmark_method_predictions.tar.gz
rm dynamicbind_benchmark_method_predictions.tar.gz
# NeuralPLexer predictions and results
wget https://zenodo.org/records/19138652/files/neuralplexer_benchmark_method_predictions.tar.gz
tar -xzf neuralplexer_benchmark_method_predictions.tar.gz
rm neuralplexer_benchmark_method_predictions.tar.gz
# RoseTTAFold-All-Atom predictions and results
wget https://zenodo.org/records/19138652/files/rfaa_benchmark_method_predictions.tar.gz
tar -xzf rfaa_benchmark_method_predictions.tar.gz
rm rfaa_benchmark_method_predictions.tar.gz
# Chai-1 predictions and results
wget https://zenodo.org/records/19138652/files/chai_benchmark_method_predictions.tar.gz
tar -xzf chai_benchmark_method_predictions.tar.gz
rm chai_benchmark_method_predictions.tar.gz
# Boltz-1 predictions and results
wget https://zenodo.org/records/19138652/files/boltz_benchmark_method_predictions.tar.gz
tar -xzf boltz_benchmark_method_predictions.tar.gz
rm boltz_benchmark_method_predictions.tar.gz
# AlphaFold 3 predictions and results
wget https://zenodo.org/records/19138652/files/af3_benchmark_method_predictions.tar.gz
tar -xzf af3_benchmark_method_predictions.tar.gz
rm af3_benchmark_method_predictions.tar.gz
# CASP15 predictions and results for all methods
wget https://zenodo.org/records/19138652/files/casp15_benchmark_method_predictions.tar.gz
tar -xzf casp15_benchmark_method_predictions.tar.gz
rm casp15_benchmark_method_predictions.tar.gz
```

### Downloading benchmark method interactions

```bash
# fetch, extract, and clean-up benchmark method interactions to reproduce paper results (~12 GB) #
# cached ProLIF interactions for notebook plots
wget https://zenodo.org/records/19138652/files/posebench_notebooks.tar.gz
tar -xzf posebench_notebooks.tar.gz
rm posebench_notebooks.tar.gz
```

### Downloading sequence databases (required only for RoseTTAFold-All-Atom inference)

```bash
# acquire multiple sequence alignment databases for RoseTTAFold-All-Atom (~2.5 TB)
cd forks/RoseTTAFold-All-Atom/

# uniref30 [46G]
wget http://wwwuser.gwdg.de/~compbiol/uniclust/2020_06/UniRef30_2020_06_hhsuite.tar.gz
mkdir -p UniRef30_2020_06
tar xfz UniRef30_2020_06_hhsuite.tar.gz -C ./UniRef30_2020_06

# BFD [272G]
wget https://bfd.mmseqs.com/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt.tar.gz
mkdir -p