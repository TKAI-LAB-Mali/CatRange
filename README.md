# RealKcat: Robust Prediction of Enzyme Variant Kinetics

## Overview
Welcome to the RealKcat repository! This project provides a reproducible pipeline to predict enzyme kinetics parameters, specifically `kcat` and `km`, using curated datasets. The repository includes tools and scripts for training and inference of both `kcat` and `km` models, along with utilities for data processing, model training, and standardized prediction.

---
## **Quick Inference with Pretrained Model:**  
For a hands-on demonstration and interactive inference, use our [`RealKcat_Inference_Interface.ipynb`](https://colab.research.google.com/drive/1z8cPg2J-EF01rd0yl7fgGlvWDohOj5m0?usp=sharing) notebook. Open it directly in Google Colab:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ssbio/CatRange/blob/main/CatRange_Inference_Interface.ipynb)

This notebook allows you to perform inference on `kcat` and `km` predictions without needing to install or configure anything locally. Simply connect to a Colab runtime, follow the provided instructions, and start exploring the RealKcat models interactively.

If you only want to make predictions with the pretrained model locally without retraining, you can use the **RealKcat_Inference.ipynb** notebook. This notebook offers an easy, interactive way to explore and make predictions for `kcat` and `km` values. Just provide your enzyme sequence and substrate Isomeric SMILES, and the notebook will guide you through the prediction process.

---

## Retraining the Models

If you’re interested in retraining the models and reproducing the results from scratch, please follow the steps below to download and set up the required datasets.

## 📂 Download and Setup the Datasets

Follow these steps to download and correctly set up the datasets in the repository's `data` folder:

1. **Download the Dataset**:
   - Visit [Chowdhury Lab Downloads](https://chowdhurylab.github.io/downloads.html).
   - Locate **KinHub-27k (Manually-curated Enzyme Parameter Database; verified from 2158 papers)** and download the dataset file (e.g., `KinHub-27k.zip`).

2. **Move the Downloaded File**:
   - Move `KinHub-27k.zip` to the `data` folder in the root directory of this repository.

3. **Extract the Files into the `data` Directory**:
   - Open a terminal or command prompt, navigate to the `data` directory, and unzip the dataset:
     ```bash
     cd path/to/RealKcat/data
     ```
     - **On Linux/macOS**:
       ```bash
       unzip KinHub-27k.zip
       ```
     - **On Windows** (using Command Prompt or Git Bash):
       ```bash
       tar -xf KinHub-27k.zip
       ```
     - Alternatively, on Windows, you can right-click `KinHub-27k.zip` and choose "Extract All..." to unzip directly into the `data` folder.

4. **Verify the Extracted Files**:
   - After extraction, your `data` folder should have the following structure:

     ```
     data/
     ├── data_split/
     ├── PafA_data/
     ├── Save_kinetic_bin_range.pkl
     ├── WT_MD_database_v1.xlsx
     ├── WT_MD_dataset.pt
     ├── WT_MD_dataset_wNeg.pt
     ```

5. **Proceed with Training or Inference**:
   - With the data set up, you can now use the provided scripts to perform training or inference.

## Repository Structure

The `RealKcat` directory should be organized as follows:

```plaintext
RealKcat/
├── data/
│   ├── PafA_data/
│   │   ├── PafA_1_test_dataset_2.pt
│   │   ├── PafA_1_test_positions_2.pt
│   │   └── PafA_1_test_kcat_km_2.pt
│   ├── data_split/
│   ├── Save_kinetic_bin_range.pkl
│   ├── WT_MD_database_v1.xlsx
│   ├── WT_MD_dataset.pt
│   └── WT_MD_dataset_wNeg.pt
├── outputs/
├── scripts/
│   ├── test_ood_kcat_predict.py
│   ├── test_ood_km_predict.py
│   ├── test_PafA_kcat_predict.py
│   ├── test_PafA_km_predict.py
│   ├── train_kcat_model.py
│   └── train_km_model.py
├── src/
│   ├── data_processing.py
│   ├── evaluation.py
│   ├── model_training.py
│   └── utils.py
├── LICENSE
├── README.md
└── RealKcat_Inference.ipynb
```

### Key Directories and Files

- **data/**: Contains all datasets, including training and test data, as well as supplementary files (e.g., bin range statistics for kinetic parameters).
  - `PafA_data/`: Datasets specifically for testing on the PafA enzyme.
  - `WT_MD_*`: Datasets for wild-type (WT) and mutant datasets.
- **outputs/**: Directory for saving model outputs, trained models, and prediction results.
- **scripts/**: Contains scripts for training and testing models.
  - `train_kcat_model.py` and `train_km_model.py`: Scripts for training models to predict `kcat` and `km`, respectively.
  - `test_*`: Scripts to run inference on test datasets for both `kcat` and `km`.
- **src/**: Contains utility scripts for data processing, model training, evaluation, and general utilities.
  - `data_processing.py`: Functions for loading and preparing datasets, standardizing data, and handling tensor data.
  - `model_training.py`: Functions for initializing and training XGBoost models with hyperparameters.
  - `evaluation.py`: Functions to evaluate model performance.
  - `utils.py`: Utility functions for device selection, setting seeds, etc.
- **RealKcat_Inference.ipynb**: Jupyter notebook for interactive inference.
- **README.md**: This file, providing documentation for the repository.

## Installation

### Prerequisites

- Python 3.8 or higher (code is compatible with Python 3.12)
- Required libraries are listed in `requirements.txt`.

### Install Dependencies

To set up your environment, clone the repository and install the dependencies:

```bash
git clone https://github.com/TKAI-LAB-Mali/RealKcat
cd RealKcat
pip install -r requirements.txt
```

## Usage

### 1. Training Models

To train models for `kcat` and `km`:

```bash
# Train kcat model
python scripts/train_kcat_model.py

# Train km model
python scripts/train_km_model.py
```

These scripts load datasets, standardize them, and train models with specified hyperparameters.

### 2. Running Inference

Run inference on test datasets using the trained models. Each `test_*_predict.py` script loads a model and dataset, applies standardization, and makes predictions.

#### Out-of-Distribution (OOD) Inference

Evaluate the model's performance on an out-of-distribution (OOD) dataset to test its robustness.

```bash
# Predict kcat on OOD dataset
python scripts/test_ood_kcat_predict.py

# Predict km on OOD dataset
python scripts/test_ood_km_predict.py
```

#### PafA Enzyme Inference

Use the PafA dataset for detailed testing on a specific enzyme.

```bash
# Predict kcat on PafA dataset
python scripts/test_PafA_kcat_predict.py

# Predict km on PafA dataset
python scripts/test_PafA_km_predict.py
```

### 3. Results and Visualization

Model outputs, trained models, and prediction results are saved as figures in the `outputs/` directory.

## License

## 📚 Citation

If you use **RealKcat** in your work, please cite the following:

> 🧬 Anna Sajeevan K, Osinuga A, B A, Ferdous S, Shahreen N, Noor MS, Koneru S, Santos-Correa LM, Salehi R, Chowdhury NB, Calderon-Lopez B, Mali A, Saha R, Chowdhury R.  
> **Robust Prediction of Enzyme Variant Kinetics with RealKcat**  
> *bioRxiv* [Preprint], 2025 Feb 15. doi: [10.1101/2025.02.10.637555](https://doi.org/10.1101/2025.02.10.637555)  
> PMID: 39990461 · PMCID: PMC11844551

<details>
<summary>📄 BibTeX</summary>

```bibtex
@article{sajeevan2025robust,
  author = {Sajeevan, Anna K and Osinuga, Abraham and B, A and Ferdous, Sakib and Shahreen, Nabia and Noor, Mohammed Sakib and Koneru, Shashank and Santos-Correa, Laura Mariana and Salehi, Rahil and Chowdhury, Niaz Bahar and Calderon-Lopez, Brisa and Mali, Ankur and Saha, Rajib and Chowdhury, Ranjan},
  title = {Robust Prediction of Enzyme Variant Kinetics with RealKcat},
  journal = {bioRxiv},
  year = {2025},
  month = {Feb},
  day = {15},
  note = {Preprint},
  doi = {10.1101/2025.02.10.637555},
  pmid = {39990461},
  pmcid = {PMC11844551}
}

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes. 
