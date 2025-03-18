# User-Localization-via-Self-Supervised-Learning
This repository contains the code and documentation for a user localization project that leverages self-supervised learning to predict the position of a user based on channel state information (CSI). The project addresses a key challenge in wireless communications—reducing the need for large amounts of labeled data by first learning useful representations from a large unlabelled dataset and then fine-tuning on a smaller labeled dataset.

Project Overview
Accurate user localization is critical for various applications including navigation, smart city infrastructure, surveillance, security, and IoT. While traditional supervised learning methods require vast amounts of labeled CSI data, this project exploits self-supervised learning techniques to reduce that dependency.

![image](https://github.com/user-attachments/assets/1f02c01d-5e78-4a19-a894-82c26fee6da3)


The project consists of two main stages:

Self-Supervised Pre-Training:

Objective: Learn meaningful representations from a large pool of unlabelled CSI data.
Data: 36,192 unlabelled samples, each containing:
H_Re: Real part of the estimated channel matrices (shape: [number of samples, 56, 924, 5])
H_Im: Imaginary part of the estimated channel matrices (same shape as H_Re)
SNR: Signal-to-noise ratio measurements (shape: [number of samples, 56, 5])
Approach: Design a custom self-supervised task (objective function/target) to extract robust features from the CSI data.
Supervised Fine-Tuning:

Objective: Utilize the learned representations to build a model that predicts user positions.
Data: 4,096 labelled samples, where each sample includes the CSI data (H_Re, H_Im, SNR) and the corresponding ground truth positions in a Cartesian coordinate system ([x, y, z]).
Approach: Train a supervised model using the extracted features and ground truth positions. The dataset is partitioned into training and validation sets for model development and hyperparameter tuning.
Test Set: 883 samples (without labels) on which the final trained model predicts user positions.
Data Details
Labelled Data: (https://drive.google.com/file/d/1atPChIUtYv8oiYwPFBZoJ8Ha0pCLFLxF/view)

Contains CSI data with ground truth positions.
Each file in labelled_data.zip includes:
H_Re, H_Im, SNR, and Pos (ground truth positions; shape: [number of samples, 3] with order [x, y, z])
Total labelled samples: 4,096
Unlabelled Data: (https://drive.google.com/file/d/18iEpL4Q6ybtPPwP2rYcZ3ocdfmMbyHp2/view)

Contains CSI data (H_Re, H_Im, SNR) without positions.
Total unlabelled samples: 36,192
Test Data: (https://drive.google.com/file/d/1gYQ_ZbX36cOiiwpdqQ94ON7Mt8DlqvEr/view)

Contains CSI data similar to the unlabelled set.
Total test samples: 883
Note: Part of the map (two streets) is intentionally omitted from the labelled dataset but is present in the test set.
Measurement Setup:

The CSI data was acquired using a massive MIMO channel sounder with an 8x8 antenna array.
Due to malfunctioning antennas, only 56 out of 64 antennas provided useful measurements.
The transmitter (an SDR-equipped cart) moved within a residential area, and ground truth positions were obtained using differential GPS. The positions were transformed into a local Cartesian coordinate system with the base station at (0,0).
Implementation Details
Self-Supervised Learning
Task Design:
Develop a custom objective function or pretext task that forces the model to learn informative representations from the unlabelled CSI data. This could involve reconstructing parts of the CSI, predicting transformations, or other tasks that capture the underlying structure of the channel responses.
Supervised Learning
Fine-Tuning:
Use the representations extracted from the self-supervised model as inputs to a supervised model that is trained on the 4,096 labelled samples. The model outputs the predicted [x, y, z] coordinates for each user.
Training and Evaluation
Model Development:
Partition the labelled dataset into training and validation sets.
Tune hyperparameters to achieve optimal localization performance.
Testing:
Run the fine-tuned model on the test data to predict the positions.
Evaluation Metric:
Localization accuracy is typically measured using metrics such as Mean Absolute Error (MAE) over the three axes.
Files Included
Jupyter Notebooks and Scripts:
Notebook(s) for self-supervised pre-training and supervised fine-tuning.
Datasets:
labelled_data.zip
unlabelled_data.zip
test_data.zip
Additional Documentation:
Detailed project report and usage instructions.
Conclusion
This project demonstrates a two-stage approach for user localization in wireless systems. By first leveraging self-supervised learning to extract robust features from unlabelled CSI data and then fine-tuning a supervised model on a smaller labelled dataset, the system effectively predicts user positions. This approach significantly reduces the dependency on large amounts of labeled data while maintaining high localization accuracy. All code, data processing steps, and evaluation results are provided in this repository.
