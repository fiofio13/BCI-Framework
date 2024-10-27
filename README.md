# A Human-AI Collaborative Framework  for Enhanced BCI Training

## Project Description
This project introduces a novel Brain-Computer Interface (BCI) framework for enhanced BCI
training. This combines hybrid data acquisition using Electroencephalography (EEG) and functional Near-Infrared Spectroscopy (fNIRS) with human-machine
collaboration through real-time feedback, aiming to design and implement an online system that will classify 4 Motor Imagery (MI) classes. Online results can be seen on the computer game Cybathlon 2024. The feedback mechanism, based on confidence scores, enables the user and machine learning (ML) model to optimise BCI training and classification accuracy.


## Structure
### 1. `code_data_collection/`
This folder contains the code and images to run the data collection task 

- **`offline_images.py`**: script for the data collection 
- **`images/`**: folder containing MI images instructions

### 2. `code_preprocessing/`
Contains Jupyter notebooks for preprocessing EEG and fNIRS data. Done after data collection 

- **`final_preprocessing_fnirs.ipynb`**: preprocessing steps for fNIRS data. Loops through all users at once. Change save & load path accordingly
- **`new_preprocessing_eeg.ipynb`**: preprocessing steps for EEG data, including filtering, artifact rejection (ICA), and resampling. Loops through all users at once. Change save & load path accordingly
- **`Carla/`**: all raw data collected is stored on this folder as the preprocessing notebooks are configured to load files from here. Processed data is stored in the respective EEG/fNIRS subfolders. Change accordingly 

### 3. `code_online/`
Contains the scripts for real-time data streaming, model training, saved models, and classification

- **`bmi_model_fold{1-5}.pth`**: Pretrained models for each fold from k-fold cross-validation. These are loaded during real-time classification 
- **`Classify_hybrid.py`**: training file for hybrid EEG-fNIRS model  
- **`model_EEG.py`**: Just EEG-based model 
- **`model_hybrid.py`**: Hybrid EEG-fNIRS model 
- **`util.py`**: Utility functions 

- **`eeg_stream.py`**: real-time streaming of EEG data. Needs to connect to BrainVision RDA (not succesfull)
- **`fnirs_stream.py`**: real-time streaming of fNIRS data using Lab Streaming Layer (LSL). Works 
- **`ml_stream.py`**: simulates real-time streaming of EEG and fNIRS data. Script to use for full online connection 
- **`ml_app.py`**: predicts current movement and gets confidence score (for feedback)
- **`ml_fb_client.py`**: sends predictions to virtual game (performs movement) and displays real-time feedback based on predictions confidence score


### 4. `data/`
The folder containing raw and processed EEG and fNIRS data. Will have: 
-  EEG: raw and processed data for all volunteers. Processed data and labels as `.npy` files, inside folder "processed"
- fNIRS: raw and processed data for all volunteers. Processed data and labels as `.npy` files, inside folder "processed"
- Both folders have a "Cybathlon" folder: containing the raw data for the error-induced cybathlon experiment 

Note: the data is also in code_preprocessing as the preprocessing scripts load the data within the folder. This can be changed on the preprocessing script's path 

---

## How to Run
### 1. Data Collection (`code_data_collection/`)
#### Steps:
1. **Activate the conda environment**:
   ```bash
   conda activate data_collection_env
   ```
2. Run the data collection script:
   ```bash
   python offline_images.py
   ```

### 2. Preprocessing (code_preprocessing/)
#### Steps:
1. **Activate the conda environment**:
   ```bash
   conda activate preprocessing_env
   ```
2. Open and run each Jupyter notebook. Ensure you have set the correct file paths in the notebooks

### 3. Running Online Classification (code_online/)
#### Steps:
1. **Activate the conda environment**:
   ```bash
   conda activate online_env
   ```
2. Train the model:
   ```bash
   python Classify_hybrid.py
   ```
3. Run the real-time connection in separate terminal windows:
   ```bash
   python ml_stream.py
   ```
   ```bash
   python ml_app.py
   ```
   ```bash
   python ml_fb_client.py
   ```
4. Ensure the Cybathlon game is open to visualize the results and feedback
---
## Dependencies
Ensure you have the following dependencies installed:
- Python 3.9
- PyTorch
- NumPy
- SciPy
- Scikit-learn
- Matplotlib
- Redis
- MNE 

## Conda Environment Setup
To easily set up the required conda environments for each folder, follow the steps below:

### 1. Download the Environment Files
Download the respective `.yml` files for each environment, located in each folder 

### 2. Install Each Environment
Once you've downloaded the `.yml` files, run the following commands to create the environments:

- **Data Collection Environment:**
   ```bash
   conda env create -f data_collection_env.yml
   ```
- **Preprocessing Environment:**
   ```bash
   conda env create -f preprocessing_env.yml
   ```
- **Online Environment:**
   ```bash
   conda env create -f online_env.yml
   ```
### 3. Activate Each Environment
Activate the required environment using:
For data collection:
- conda activate data_collection_env
For preprocessing:
- conda activate preprocessing_env
For online classification:
- conda activate online_classification_env

---

## Future Work
- Expand the data collection, larger pool of participants 
- Alternative software for EEG because PyCoder requires Python 2.6 (discontinued)
- Successful EEG stream, hence allows the full system to conduct participant online session 
- Further enhance by model learning stress patterns, using error-induced recordings 
- Refine feature selection, and explore more complex ML architectures 
- - Increase folds 
- - Focus on training specific layers

## Contribution
Special thanks to Professor Aldo Faisal and Jinpei Han for their guidance, knowledge and support through this project

## Contact
For any questions, code not running or feedback, please email cfu23@ic.ac.uk or fiorella.urcia@gmail.com


---
