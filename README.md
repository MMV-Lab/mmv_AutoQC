<div align="center">
<h2>AutoQC</h2>

[Zixuan Pan](https://scholar.google.com/citations?user=3VuW2gcAAAAJ&hl=en), [Justin Sonneck](https://scholar.google.com/citations?user=ReDjyuAAAAAJ&hl=en&oi=ao), [Dennis Nagel](https://scholar.google.com/citations?user=bW4feA4AAAAJ&hl=en&oi=ao), Anja Hasenberg, [Matthias Gunzer](https://scholar.google.com/citations?user=1uh0hw4AAAAJ&hl=en&oi=ao), [Yiyu Shi](https://scholar.google.com/citations?hl=en&user=LrjbEkIAAAAJ&view_op=list_works), [Jianxu Chen](https://scholar.google.com/citations?hl=en&user=HdolpOgAAAAJ)

</div>

## Introduction
AutoQC is a benchmarking framework for automatic quality control of high-throughput microscopy images.
It provides a benchmark dataset and a set of baseline methods for evaluating the performance of quality control algorithms.

<p align="center">
  <img src="figs/dataset.png" width="70%">
</p>
<p align="center">Fig. 1: Overview of the benchmark dataset.</p>

## Benchmark Dataset Preparation
All datasets curated in this paper and split files are available at [BioStudies]( https://doi.org/10.6019/S-BIAD2133).
We also provide the pre-trained models for all baselines at [Google Drive](https://drive.google.com/drive/folders/1d8Fa2PZ3z7egrrjQfFMa5ohG7ThlAXIb?usp=share_link).
Once downloaded, please unzip the files and place them in the `data` folder of this repository.

## Installation
Before starting, we recommend creating a new conda environment and installing the required packages listed in [requirements.txt](requirements.txt). We test our methods on Python 3.9.7 and CUDA 11.8.

### Create a Conda Environment
You can create and activate a new conda environment with the following commands:

```bash
conda create -n autoqc python=3.9.7
conda activate autoqc
pip install -r requirements.txt
```


## Quick Start
We provide a [jupyter notebook](example.ipynb) in the root directory of this repository, which can be used to run inference on the example data.

To download the benchmark dataset from [BioStudies](https://doi.org/10.6019/S-BIAD2133):

```bash
python download_autoqc_data.py --out ./data
```

Using the flags `--only_train`, `--only_test`, or `--only_splits`, you can download individual subsets.

## Training and Evaluation

### Configuration
1. Edit the configuration files in the `configs` folder to match your experiment setup:
  - `configs/config.yaml`: Main configuration for training and evaluation.
  - `configs/datamodule/DNA.yaml`: Set the correct paths for dataset splits.
  - `configs/experiment/Benchmark_Methods/*.yaml`: Select and configure the baseline method you wish to use.
  - `pc_environment.env`: Set the paths for logs and data storage.

To reproduce the baseline results from the paper, you only need to update the split paths in `datamodule/DNA.yaml` and set the log/data paths in `pc_environment.env`.

### Training
Run the training script:
```bash
bash train.sh
```
This will start the training process using the selected configuration. After training completes, the model will automatically evaluate on the test set and save the results.

### Evaluation

If you want to directly perform evaluation using a pre-trained model, follow these steps:

1. Download the desired pre-trained model checkpoint from the provided Google Drive link and place it in your workspace.
2. In `configs/config.yaml`, set the following options:
  - `onlyEval: True`  # This will skip training and run evaluation only
  - `load_checkpoint: <path_to_your_checkpoint.ckpt>`  # Specify the path to your downloaded checkpoint

Example snippet for `config.yaml`:
```yaml
onlyEval: True
load_checkpoint: data/checkpoints/your_model.ckpt
```

Then run:
```bash
bash train.sh
```
The script will load the pre-trained model and perform evaluation on the test set, saving results and logs as configured.

## Cite
If you find this repository useful, please use the following BibTeX entry for citation.
```
@article{pan2025autoqc,
  title={AutoQC-Bench: a diffusion model and benchmark for automatic quality control in high-throughput microscopy},
  author={Pan, Zixuan and Sonneck, Justin and Nagel, Dennis and Hasenberg, Anja and Gunzer, Matthias and Shi, Yiyu and Chen, Jianxu},
  journal={npj Imaging},
  volume={3},
  number={1},
  pages={57},
  year={2025},
  publisher={Nature Publishing Group UK London}
}
```