# Spine Finite Element Benchmarking – README

This repository provides the code, data structure, and usage instructions for benchmarking linear and nonlinear finite element (FE) models of vertebral strength using calibrated CT scans derived from the VERSE dataset. More details are described in the publication:

- Matthias Walle, Bryn E. Matheson, Steven K. Boyd. Comparing Linear and Nonlinear Finite Element Models of Vertebral Strength Across the Thoracolumbar Spine: A Benchmark from Density-Calibrated Computed Tomography. bioRxiv 2025.04.19.649449; doi: https://doi.org/10.1101/2025.04.19.649449
  

![Figure_1](https://github.com/user-attachments/assets/56439e35-0778-48f1-b371-0d9b40a1788a)



Figure 1: Study Overview

## Directory Structure

```
.
├── data/
│   ├── calibration_files/        # Calibration text files for converting HU to density (full set needs to be downloaded from [Zenodo](https://doi.org/10.5281/zenodo.15313258))
│   ├── meta_data/                # CSV files with individual info (must be downloaded separately)
│   └── nii_files/                # CT and mask files in NIfTI format (full set needs to be downloaded from [Zenodo](https://doi.org/10.5281/zenodo.15313258))
├── notebooks/                    # Jupyter notebooks for processing and analysis
├── results/                      # Output figures, tables, and tabulated values
├── CITATION.cff                  # Please cite this project AND related projects below if using this data
├── environment.yml               # Conda environment definition
├── LICENSE                       # License for using this data
└── README.md
```

## Data Folder Details

### calibration_files/
Contains `.txt` files used to calibrate CT Hounsfield units (HU) to density. These are necessary for generating material properties in finite element models.
The full dataset must be downloaded from [Zenodo](https://doi.org/10.5281/zenodo.15313258).

The pretrained nnU-net model ([nnU-Net Github](https://github.com/MIC-DKFZ/nnUNet)) weights are available on Zenodo:
Vertebral Body Segmentation: [10.5281/zenodo.15238175](https://doi.org/10.5281/zenodo.15238175)
Calibration Tissue Segmentation: [10.5281/zenodo.15238422](https://doi.org/10.5281/zenodo.15238422)

### meta_data/
Contains CSV metadata describing the full VERSE cohort, including target vertebra, fracture status, and scan identifiers. You must manually download these CSV files from the original publication appendix:

- Löffler et al. Radiology: Artificial Intelligence, 2020  
  https://doi.org/10.1148/ryai.2020190138

### nii_files/
This folder contains example `.nii.gz` files, including:

- Calibrated and resampled CT images cropped for each (unfractured) vertebra label (e.g. _vertebra_20 = L1, see Verse19 labelling for more information)
- Cropped and relabeled segmentation masks

The full dataset of cropped NIfTI files must be downloaded from OSF [insert link] and processed accordingly.

## Getting Started

To run the notebooks and replicate the results:

1. Clone the repository:

```bash
git clone https://github.com/Bonelab/spineFE-benchmark.git
cd spineFE-benchmark
```

2. Create and activate the environment:

```bash
conda env create -f environment.yml
conda activate spineFE
```

3. Install the required package:

```bash
pip install git+https://github.com/Bonelab/Ogo.git
```

## Notebooks Overview

### 1. Finite Element Model Generation and Solving
Shows how to segment vertebrae, apply density calibration, convert images to material IDs, and generate linear and nonlinear micro-FE models.

### 2. Internal Density Calibration
Reprocesses the full VERSE images to apply internal density calibration on the original clinical scans. This is necessary if other structures in the image are of interest (e.g. muscle density) since the example images are cropped to the bone and pre-aligned. Download the original Verse 2019 data here: https://osf.io/nqjyw/

### 3. Reproduce Manuscript Figures and Tables
Recreates all visualizations, tables, and statistical results reported in the accompanying manuscript.

## Results Folder

The `results/` folder contains:

- Figures and tables used in the manuscript
- Tabulated strength and density values at the target vertebra for each subject
- Linear FEA-derived stiffness values at all non-fractured vertebrae

## Visual Data Overview

![image](https://github.com/user-attachments/assets/56c0a6b8-8c24-4669-a49c-02710273cbf0)

Figure 2: The dataset includes additional segmenations for vertebral body/process, trabecular/cortical bone and artificial intervertebral disks. 

## License and Data Source

### License
The data are distributed under the Creative Commons Attribution-ShareAlike 2.0 License (CC BY-SA 2.0):

https://creativecommons.org/licenses/by-sa/2.0/

By using or redistributing these files, you agree to the terms of this license, including appropriate attribution and share-alike conditions.

### Source
The original data come from the VERTEBRAE SEGMENTATION (VerSe) challenge:

- Löffler M, Sekuboyina A, Jakob A, et al.  
  A Vertebral Segmentation Dataset with Fracture Grading. Radiology: Artificial Intelligence, 2020.  
  https://doi.org/10.1148/ryai.2020190138

- Sekuboyina A. et al.  
  Labelling Vertebrae with 2D Reformations of Multidetector CT Images: An Adversarial Approach for Incorporating Prior Knowledge of Spine Anatomy. Radiology: AI, 2020.  
  https://doi.org/10.1148/ryai.2020190074

- Sekuboyina A, Bayat AH, et al.  
  VerSe: A Vertebrae Labelling and Segmentation Benchmark.  
  https://arxiv.org/abs/2001.09193

### Modifications
The `.nii.gz` files in this repository were modified for finite element analysis purposes. Changes include:

- Resampling to isotropic resolution
- Calibration to density using internal density calibration
- Cropping and re-alignment for FE analysis
- Relabeling of segmentation masks for cortical, trabecular, vertebral body and process and disk components

These modifications facilitate efficient and consistent FE modeling across the spine.
