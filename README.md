# VAE SST Emulator

This repository provides an implementation of a **Variational Autoencoder (VAE)** for learning and emulating global **Sea Surface Temperature (SST)** variability using climate model simulations from the **Coupled Model Intercomparison Project Phase 6 (CMIP6)**.

The goal of this work is to investigate the potential of **deep generative models** as computationally efficient surrogates for climate simulations. By learning the statistical structure of SST fields from historical climate model outputs, the model can reconstruct SST patterns and generate long-term simulations that preserve important spatial and temporal variability.

This repository accompanies the research work submitted to the **Climate Informatics Conference**.

---


The repository includes both **Jupyter notebooks** and **Python scripts** for the full modeling pipeline.

---

# Dataset

The experiments use **monthly Sea Surface Temperature (SST)** fields from the **GISS-E2-1-H climate model** available through the **CMIP6 archive**.

Due to GitHub file size limitations, the dataset is hosted externally.

Dataset download link:

https://drive.google.com/drive/folders/1xwdQYbtuco6mCsnLPWYtuBhHJKGRPHjm?usp=sharing

Download the files and place them in a directory named `Data`:

Data/
├── COBE_SST_resize.nc
└── GISS-E2-1-H_historical_1850_2014.nc


---

# Data Description

The SST dataset spans **165 years (1850–2014)** with **monthly resolution** and is provided on a coarser spatial grid of:
48 latitude × 144 longitude

This corresponds to a spatial resolution of approximately:
3.75° × 2.5°


The final dataset shape is:
(165 years × 12 months × 48 lat × 144 lon)


---

# Preprocessing Pipeline

To ensure meaningful learning of SST variability, the following preprocessing steps are applied:

1. **Anomaly Calculation**

The global SST field is dominated by a long-term warming trend and large-scale mean patterns. To focus on variability, monthly SST anomalies are computed by subtracting the climatological mean.

2. **Linear Trend Removal**

Following standard climate analysis practice, a **linear detrending procedure** is applied to remove long-term trends from the anomaly data.

3. **Normalization**

The detrended anomaly fields are normalized using training data statistics to stabilize neural network training.

4. **Temporal Reshaping**

The dataset is converted from: (years, months, lat, lon) to (time, lat, lon) 
where:
time = years × months


---

# Training / Validation / Test Split

The dataset is split chronologically to preserve the temporal structure of climate variability.

| Period | Years | Samples |
|------|------|------|
| Training | 1850–1965 | 1380 months |
| Validation | 1966–1990 | 300 months |
| Test | 1991–2014 | 300 months |

This ensures that the model is evaluated on **future data not seen during training**.

---

# Model

The model implemented in this repository is a **Convolutional Variational Autoencoder (VAE)** that learns a compressed latent representation of SST fields.

The architecture consists of:

Encoder  
→ Convolutional feature extraction  
→ Latent variable representation  

Decoder  
→ Reconstruction of SST spatial fields

The model is trained using the standard **VAE loss function** consisting of:

- Reconstruction loss
- KL divergence regularization

---

# Running the Code

## 1 Install dependencies

Create a Python environment and install required packages:

pip install numpy
pip install torch
pip install xarray
pip install netCDF4
pip install scipy
pip install matplotlib


## 2 Run preprocessing

python data_loading_and_preprocessing.py


## 3 Train the model

python model_training_&_results.py



Alternatively, the workflow can be executed step-by-step using the provided **Jupyter notebooks**.

---

# Reproducibility

This repository contains all scripts required to reproduce the preprocessing, training, and evaluation pipeline used in the experiments.

To reproduce results:

1. Download the dataset from the link above
2. Place the files in the `Data` directory
3. Run preprocessing
4. Train the VAE model

---

# License

This project is released under the **MIT License**.

---

# Acknowledgements

This work uses climate model data from the **Coupled Model Intercomparison Project Phase 6 (CMIP6)** and builds upon methods from the fields of **deep generative modeling and climate informatics**.


