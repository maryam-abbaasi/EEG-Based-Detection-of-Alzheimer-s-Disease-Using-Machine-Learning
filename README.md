# EEG Based Detection of Alzheimer's Diseases using Machine Learning 

## Project Overview

This project focuses on the detection of Alzheimer’s disease (AD) using EEG (Electroencephalography) signals and machine learning models.Resting-state, eyes-closed EEG recordings are processed, augmented, transformed into features, and classified using multiple machine learning algorithms.

The complete workflow follows these steps exactly as implemented in the Google Colab notebook:

* EEG data loading
* Data cleaning
* Data augmentation
* Train–test splitting
* Feature extraction using Kernel PCA
* Training multiple machine learning models
* Model evaluation and comparison

The goal is to identify which machine learning model performs best for EEG-based Alzheimer’s disease detection.


## Dataset Description

The EEG dataset used in this project is obtained from OpenNeuro.

* **Dataset Name:** A dataset of 88 EEG recordings from Alzheimer’s disease, Frontotemporal dementia, and Healthy subjects
* **DOI:** 10.18112/openneuro.ds004504.v1.0.1

## Participants Information

* **Alzheimer’s Disease (AD):** 36 subjects
* **Frontotemporal Dementia (FTD):** 23 subjects
* **Healthy Controls (CN):** 29 subjects
* **Total Subjects:** 88

Only Alzheimer’s disease (AD) and Healthy control (CN) subjects are used in this project.


## EEG Recording Details

* **EEG Device:** Nihon Kohden EEG 2100
* **Electrodes:** 19 scalp electrodes (10–20 international system)
* **Reference Electrodes:** A1 and A2 (mastoid)
* **Sampling Rate:** 500 Hz
* **Recording Condition:** Resting state, eyes closed
* **Average Recording Duration:** 12–14 minutes
* **Impedance:** Less than 5 kΩ
  

##  EEG Preprocessing Pipeline

The EEG signals were already preprocessed using EEGLAB (MATLAB) and are stored in the `derivatives` folder.

The preprocessing steps are:

1. **Band-pass Filtering**

   * Butterworth filter applied from 0.5 Hz to 45 Hz

2. **Re-referencing**

   * Signals re-referenced to A1–A2

3. **Artifact Subspace Reconstruction (ASR)**

   * Removes noisy EEG segments
   * Window size: 0.5 seconds
   * Standard deviation threshold: 17

4. **Independent Component Analysis (ICA)**

   * RunICA algorithm applied
   * 19 EEG channels transformed into 19 ICA components

5. **Automatic Artifact Removal**

   * ICLabel used for classification
   * Components classified as eye artifacts or jaw artifacts were removed
   * Eye movement artifacts were still found despite eyes-closed recordings


## Data Loading and Cleaning

* EEG CSV files are loaded from Google Drive using glob
* Google Drive is mounted in Google Colab
* All values are converted to numeric format
* Missing values are handled using forward fill and backward fill
* Each EEG sample is trimmed to 1024 samples
* Cleaned files are saved back to disk
* A backup copy of cleaned data is created before augmentation


## Data Augmentation

Data augmentation is applied to increase dataset size and reduce overfitting.

### Augmentation Techniques Used

* **Positive Time Shifting**

  * EEG signals shifted forward by 3–15 samples
* **Negative Time Shifting**

  * EEG signals shifted backward by 3–15 samples
* **Gaussian Noise Injection**

  * Noise added based on signal standard deviation
* **Rolling Mean Smoothing**

  * Window size between 3 and 8 samples

Each original EEG file generates multiple augmented versions.
Augmentation is applied separately to Alzheimer and Healthy samples.


## Train and Test Split

* Augmented EEG samples are mainly used for training
* Original EEG samples are mainly used for testing
* Small portion of original data is included in training to maintain balance

### Final Dataset Sizes

* **Training samples:** 202
* **Testing samples:** 41

## Feature Extraction

The following steps are applied exactly as in the notebook:

1. **Standardization**

   * StandardScaler used to normalize EEG data

2. **Kernel Principal Component Analysis**

   * RBF kernel applied
   * 5 principal components extracted per channel

3. **Feature Formation**

   * Mean of components calculated
   * Final feature vector size: 19 features per EEG sample


##  Dataset Preparation

* Alzheimer samples labeled as alzheimer
* Healthy samples labeled as healthy
* Features and labels saved as:

  * train.csv
  * test.csv
* No missing values present after processing
* Final feature matrix shapes:

  * Training: 202 × 19
  * Testing: 41 × 19


## Machine Learning Models and Results

All models are trained on the same training set and evaluated on the same test set.

| Model                    | Accuracy |
|--------------------------|----------|
| Logistic Regression       | ~68.29% |
| SVM                       | ~90.24% |
| KNN                       | ~87.80% |
| Decision Tree             | ~80.49% |
| Random Forest             | ~97.56% |
| Gaussian Naive Bayes      | ~75.61% |
| Bernoulli Naive Bayes     | ~58.54% |
| Ensemble Learning         | ~95.12% |


## Citations
Andreas Miltiadous, Katerina D. Tzimourta, Theodora Afrantou, Panagiotis Ioannidis, Nikolaos Grigoriadis, Dimitrios G. Tsalikakis, Pantelis Angelidis, Markos G. Tsipouras, Evripidis Glavas, Nikolaos Giannakeas, and Alexandros T. Tzallas (2023). A dataset of 88 EEG recordings from: Alzheimer's disease, Frontotemporal dementia and Healthy subjects. OpenNeuro. [Dataset]
doi:10.18112/openneuro.ds004504.v1.0.1
