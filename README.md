# Project-3
Deep Learning for Antibiotic Resistance Gene Prediction

Antibiotic Resistance is a phenomenon exhibited by bacterial species; this allows the bacteria to grow even in the presence of the antibiotic for which they exhibit an antibiotic resistance gene (ARG). Such resistances are incorporated due to the production of specific resistance mechanisms or proteins which are the post-translational results of this ARGs. In therapeutics ARG related resistance is unwanted especially when the causative agent is a bacteria so, using deep learning techniques we can predict and distinguish an unknown gene sequence to be either ARG or NARG (Non-ARG) in nature.

This repository contains the code, methodology, and the deployed model for predicting Antibiotic Resistance Genes (ARGs) directly from raw DNA sequences using a 1D Convolutional Neural Network (CNN).

The `Steps_Followed.docx` file provides a highly detailed, step-by-step walkthrough of the data harvesting, preprocessing, model architecture, and deployment phases. Due to GitHub's file size limits, the raw genomic datasets (which contain millions of base pairs) are not hosted here, but the extraction and slicing methodologies are fully documented. The primary model pipeline is located in [Codes for 3rd Project.ipynb](https://github.com/MohakKarkhanis/Project-3/blob/main/Codes/Codes%20for%203rd%20Project.ipynb), and the deployable Terminal Python script is [Predictor for 3rd Project.py](https://github.com/MohakKarkhanis/Project-3/blob/main/Model/Predictor%20for%203rd%20Project.py).

**THE RAW GENOMIC DATA OF ALL THE TRAINING BACTERIA CAN BE FOUND HERE: https://drive.google.com/drive/folders/178bYZTGlvpqArCOH_jevYrN0BsfdcbSn?usp=drive_link"**

**Tools**
1. Language: Python
2. Deep Learning Framework: TensorFLow / Keras 1D CNN architecture
3. Libraries: BioPython SeqIO
4. Data Processing: NumPy, Scikit-learn

**Methodology**
1. Positive sample Acquisition (ARGs): Harvested positive ARG nucleotide sequences from the Comprehensive Antibiotic Resistance Database [CARD](https://card.mcmaster.ca/latest/data) using protein homology models and calculated the average length of the sequences. **6052 Positive Samples, 963.81bp average ATGC present in all samples, 303.91bp is the Standard Deviation**
2. Negative Sample Acquisition (NARGs): Downloaded full genomic data for 97 distinct pathogens from [NCBI Datasets](https://www.ncbi.nlm.nih.gov/datasets/). To balance the data without dimension mismatch, random fragments roughly matching the average ARG length was sliced from these genomes to create identical number of samples wrt to the Positive ones. **6052 Negative Samples, 963.1 bp average count**
3. Data Procesing and Encoding: The dataset was split into `Training:Testing:Validation (70:15:15)`. Sequences were restricted to `_MAX_SEQUENCE_LENGTH = 2000_`. Applied One-Hot Encoding to convert string characters into binary arrays of 1 or 0 interpretable by the CNN. **`A: [1,0,0,0], T: [0,1,0,0], G:[0,0,0,1], C[0,0,1,0], N[0,0,0,0]`** 
4. 1D CNN Architecture: Designed a custom Sequential Model to detect local spatial patterns in the DNA sequences: a. **Layer 1:** `Conv1D` (128 filters, kernel size 8, ReLU) + `MaxPooling1D(2)`  **Layer2:** `Conv1D` (64 filters, kernel size 8, Activation=ReLU) + `MaxPooling1D(2)`. To prevent overfitting Dropout(0.5) meaning each training step 50% of neurons are randomly deactivated thereby increasing the overall prediction accuracy.
**Output Layer:** `Dense(1, activation='sigmoid')`: 1 has been used to denote a binary relationship or output meaning our DNA sequence would be classified into either ARG or NARG (1 or 0 respectively).
6. Model Training: Compiled using the 'Adam' optimizer and 'binary_crossentropy' loss. The model was trained over 50 epochs with a batch size of 32.

**Results**
The 1D CNN model achieved an outstanding Test Accuracy of **0.9840** with a Test Loss of **0.0809**. The trained weights and architecture were saved as an `.h5` file for immediate, real-time deployment without the need for retraining.

**Steps to use the Predictor**
The model can be easily deployed on any novel `.fasta` sequence using the Terminal.
1. Download the [Model](Model) folder. Ensure that [Predictor for 3rd Project.py](https://github.com/MohakKarkhanis/Project-3/blob/main/Model/Predictor%20for%203rd%20Project.py) python script, the `.h5` model file, and the fasta sequence which needs to be predicted are in the same directory. It is recommended to change the `MODEL_FILE_PATH` variable in the `.py` file to the path where the `.h5` model has been saved.
2. Open Terminal within the directory to set the location and run the following command:
    `_python Predictor for 3rd Project.py your fasta sequence_`

