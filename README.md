# Compressed-Sensing MRI Reconstruction with ISRSL0

Python implementation and evaluation of the Image-Smoothness-Regularized Smoothed-\(\ell_0\) (ISRSL0) algorithm for compressed-sensing magnetic resonance imaging (CS-MRI) reconstruction.

This project was completed for the **Medical Imaging Systems** course in the M.Sc. Biomedical Engineering program at **Amirkabir University of Technology**. It implements an ISRSL0 reconstruction pipeline, compares it with standard Smoothed-\(\ell_0\) (SL0) and zero-filled reconstruction baselines, and evaluates the influence of multiple k-space undersampling patterns on MR image reconstruction.

> **Note:** This repository is an independent educational implementation and evaluation of the ISRSL0 method. It is not an official implementation by the authors of the reference paper.

---

## Overview

Magnetic resonance imaging (MRI) acquisition can be accelerated by acquiring only a subset of k-space measurements and reconstructing the image using compressed-sensing methods. However, undersampling introduces aliasing artifacts and can reduce the visibility of fine anatomical structures.

This project implements the **ISRSL0** framework, which combines sparse recovery based on a smooth approximation of the \(\ell_0\) norm with image-smoothness regularization. The method uses an iterative reconstruction strategy that alternates between:

- A denoising and sharpening stage using Wiener or BM3D denoising.
- A gradient-descent stage for Smoothed-\(\ell_0\) sparse recovery.
- A projection step that enforces consistency with the acquired k-space measurements.
- A graduated reduction of the smoothing parameter during optimization.

---

## Features

- Implementation of the ISRSL0 compressed-sensing MRI reconstruction method.
- ISRSL0 reconstruction with two denoising approaches:
  - Wiener filtering.
  - BM3D denoising.
- Standard SL0 reconstruction baseline.
- Zero-filled inverse Fourier reconstruction baseline.
- Haar-wavelet sparse representation.
- Multiple 2D k-space undersampling masks:
  - Pseudo-radial.
  - Spiral.
  - Cartesian along \(k_x\).
  - Cartesian along \(k_y\).
  - Random.
  - Variable-density random.
  - Square.
- Reconstruction-quality evaluation using:
  - Peak signal-to-noise ratio (PSNR).
  - Structural similarity index (SSIM).
  - High-frequency error norm (HFEN).
- Visualization of reconstruction outputs and corresponding k-space data.
- Batch evaluation across MRI volumes with CSV export of reconstruction metrics.

---

## Dataset

Experiments were conducted using knee and brain MRI data from the [fastMRI dataset](https://fastmri.med.nyu.edu/).

The fastMRI data are **not included** in this repository. To reproduce the experiments:

1. Request/download the appropriate fastMRI dataset from the official source.
2. Store the data locally.
3. Update the dataset paths in the notebook before execution.

Please comply with the fastMRI dataset license, terms of use, and citation requirements. Do not upload raw fastMRI data or `.h5` files to this repository.

---

## Repository Contents

```text
Compressed-Sensing-MRI-Reconstruction/
├── isrsl0_cs_mri_reconstruction.ipynb
├── outputs/
│   ├── figures/
│   │   └── [reconstruction and sampling-mask visualizations
│   └── [CSV files containing evaluation results]
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Results

A representative evaluation was performed using **20% pseudo-radial k-space sampling** across **199 fastMRI knee volumes**.

| Reconstruction method | PSNR (dB) | SSIM | HFEN |
|---|---:|---:|---:|
| Zero-filled | 27.56 ± 3.60 | 0.739 | 0.271 |
| Standard SL0 | 28.09 ± 3.30 | 0.743 | 0.270 |
| ISRSL0 + BM3D | 31.10 ± 3.30 | 0.822 | 0.252 |
| ISRSL0 + Wiener | 32.18 ± 3.00 | 0.848 | 0.268 |

Under this experimental configuration, **ISRSL0 with Wiener denoising** improved PSNR by approximately **4.1 dB** and SSIM by **0.105** compared with the standard SL0 baseline.

Results may vary depending on the sampling mask, sampling rate, denoising method, selected MRI volumes, preprocessing, and reconstruction hyperparameters.

---

## Reconstruction Examples

The figures below compare zero-filled reconstruction, standard SL0, and ISRSL0 reconstruction with Wiener and BM3D denoising under 20% pseudo-radial k-space undersampling.

### Brain MRI reconstruction

![Brain MRI reconstruction comparison](outputs/figures/results/Reconstruction%20results%20comparison%20for%20Pseudo-Radial%2020%25%20mask%20%28a%20sample%20brain%20image%29.png)

*Comparison of reference, zero-filled, SL0, ISRSL0-Wiener, and ISRSL0-BM3D reconstructions for a representative brain MRI slice. The bottom row shows the associated k-space representations.*

### Knee MRI reconstruction

![Knee MRI reconstruction comparison](outputs/figures/results/Reconstruction%20results%20comparison%20for%20Pseudo-Radial%2020%25%20mask%20%28a%20sample%20knee%20image%29.png)

*Comparison of reference, zero-filled, SL0, ISRSL0-Wiener, and ISRSL0-BM3D reconstructions for a representative knee MRI slice. The bottom row shows the associated k-space representations.*
## Installation

Clone the repository:

```bash
git clone [https://github.com/Sepehr2001/Compressed-Sensing-MRI-Reconstruction.git](https://github.com/Sepehr2001/Compressed-Sensing-MRI-Reconstruction.git)
cd Compressed-Sensing-MRI-Reconstruction
```

Create and activate a virtual environment.

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### macOS/Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

---

## Requirements

The project uses Python and the following main packages:

```text
numpy
scipy
matplotlib
pandas
h5py
scikit-image
PyWavelets
bm3d
jupyter
```

---

## Usage

Launch Jupyter Notebook from the project directory:

```bash
jupyter notebook
```

Then open:

```text
isrsl0_cs_mri_reconstruction.ipynb
```

Before running the notebook, update the local file paths for the downloaded fastMRI data.

The notebook workflow includes:

1. Loading MRI data and fully sampled k-space.
2. Generating an undersampling mask.
3. Constructing masked k-space measurements.
4. Performing zero-filled, standard SL0, and ISRSL0 reconstruction.
5. Computing PSNR, SSIM, and HFEN.
6. Visualizing image-domain and k-space reconstruction results.
7. Running batch experiments and exporting quantitative results.

---

## Reference Method

This project is based on the following paper:

> A. Ghaffari et al., “ISRSL0 compressed sensing MRI with image smoothness regularized-smoothed \(\ell_0\),” *Scientific Reports*, 2024.  
> [https://doi.org/10.1038/s41598-024-74074-4](https://doi.org/10.1038/s41598-024-74074-4)

Please cite the original article when using or discussing the ISRSL0 method.

---

## Limitations

- This code is intended for educational and research purposes only.
- It is not validated for clinical use or diagnostic decision-making.
- The reconstruction parameters may require tuning for different acquisition settings, sampling patterns, datasets, or image contrasts.
- The fastMRI data are not distributed with this repository.

---

## Author

**Sepehr Kalanaki**  
M.Sc. Student in Biomedical Engineering (Bioelectric)  
Amirkabir University of Technology

- [GitHub](https://github.com/Sepehr2001)
- [LinkedIn](https://www.linkedin.com/in/sepehr-kalanaki-b4662122b/)
