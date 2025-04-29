# Predicting Ligand Binding Affinity to D2 Dopamine Receptor
**`UC Berkeley C242 Machine Learning Final Project`**

## Dataset

Data was collected from [BindingDB](https://www.bindingdb.org/rwd/bind/chemsearch/marvin/SDFdownload.jsp?download_file=/rwd/bind/downloads/BindingDB_PDSPKi_202504_tsv.zip), a public molecular recognition database from UCSD. It is a comprehensive resource for experimentally measured binding affinities between small molecules and protein targets, and comprises almost 28k samples.

Key features:
- Ligand SMILES: ligands identified as SMILES strings, 
- Target Name: the receptor the ligand binds to
- Ki (nM): the inhibitory constant  which measures how strongly the ligand binds to the receptor (target variable).

Data preprocessing:
- The dataset is first filtered to keep only ligands that bind to receptor Dopamine D(2): 1439 samples.
- Features are then selected from the SMILES strings thanks to RDKIT, to obtain 6 descriptors (molecular weight, number H donors, number H acceptors, TPSA, Number rotatable bonds and ring count), and Morgan Fingerprints (512 bit binary vectors).

During the project, the [1439, 6] descriptors dataset was first used, and then the [1439, 512] Morgan fingerprints dataset was used.

## Project Goal 
Use RDKIT descriptors and Morgan Fingerprints extracted from the ligands SMILES as input features to predict the inhibitory constant Ki linked with receptor Dopamine D(2) as an output.

## Machine Learning Methods 
XGBoost: XGBoost Regressor applied to the RDKIT descriptor dataset and the Morgan fingerprint data. Used RandomizedSearchCV to automatically determine best hyperparameters, then further manually adjusted hyperparameters for better tuning.

Neural Network: Multi-layer Perceptron (MLP). Multiple architectures of different depth and width tested, as well as different hyperparameters. First using the RDKIT descriptors dataset, then using the 512 morgan fingerprints gave better results.


## Code Structure Overview
The project is organized into three main components:

### 1. Data Processing: data_processor.ipynb
This notebook processes the raw ziped dataset (BindingDB_PDSPKi_202504_tsv):

Preprocessing: extracts data, filters dataset, log(Ki), extracts descriptors and morgan fps from SMILES, Normalizes, and splits the data into training and testing sets.

Output: Saves the processed data (X_train, X_test, y_train, y_test for the RDKIT descriptors and X_train_fps, X_test_fps, y_train_fps, y_test_fps for the Morgan Fingerprints) in pickle files (data_splits.pkl and data_splits_fps.pkl).

### 2. Model Training: MLP.ipynb and XGBoost.ipynb
These notebooks train models using the preprocessed data:

Data Loading: Loads data_splits.pkl with:
    with open("data_splits.pkl", "rb") as f:
        X_train, X_test, y_train, y_test = pickle.load(f)
Model Training & Evaluation: Trains and evaluates models on X_train, y_train and tests on X_test, y_test.

### Workflow:
data_processor.ipynb: Processes and splits data, saves to data_splits.pkl and data_splits_fps.pkl.

MLP.ipynb and XGBoost.ipynb: Train and evaluate models using the preprocessed data.
