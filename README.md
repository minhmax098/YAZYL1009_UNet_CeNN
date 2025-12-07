# Project: LUNA16 Lesion Segmentation with U-Net and CeNN Preprocessing
This repository contains the code for my course project on lung nodule segmentation using U-Net and Cellular Neural Networks (CeNN) on the LUNA16 dataset.

In this work, I compare two segmentation pipelines:

1. Baseline U-Net – a standard 2D U-Net trained on windowed CT slices.
2. CeNN + U-Net – the same U-Net, but the input is a 3-channel image formed by stacking the original CT slice with two CeNN-enhanced versions.

Both models are trained and evaluated under identical settings so that the effect of the CeNN preprocessing step can be assessed fairly.

# Abstract
Lung nodule segmentation on CT scans is a key step in computer-assisted analysis for early lung cancer screening. Although U-Net and related deep learning models perform well on many medical imaging tasks, they still struggle with nodules that are very small or have weak contrast compared to surrounding tissue.  
This project compares two processing pipelines on the LUNA16 dataset. The first trains a standard 2D U-Net on windowed CT slices. The second adds a Cellular Neural Network (CeNN) that produces two contrast-enhanced versions of each slice; these are stacked with the original image and fed as a three-channel input to the same U-Net architecture.  
Performance is measured with Dice, IoU, sensitivity, and specificity, and a paired Wilcoxon signed-rank test is applied to the per-patient Dice scores. In the experiments, the CeNN + U-Net variant does not outperform the baseline: Dice and IoU are slightly lower, and the p-value (0.176) indicates that the difference is not statistically significant.

# Dataset
- **Dataset:** LUNA16 (subset of LIDC-IDRI)
- The raw CT volumes (`subset0`–`subset9`) and `annotations.csv` are **not included** in this repository.
- Please download the data from the official LUNA16 website and place them under:

  ```text
  Lung-nodule-detection-LUNA-16/
      dataset/
          subset0/
          subset1/
          ...
          subset9/
      dataset/annotations.csv

# Environment
This project was developed and tested in Google Colab with:
- Python 3.x
- PyTorch >= 2.0
- segmentation-models-pytorch
- SimpleITK
- numpy, pandas, scikit-learn
- opencv-python (cv2)
- matplotlib, tqdm

# Repository organization
- data_prep/ : Code to convert LUNA16 CT volumes and annotations into 2D slices and masks.

CeNN.ipynb: generate CeNN-enhanced slices and merged 3-channel .npy files.

- train_codes/ : Code to train the networks.

Unet_Baseline.ipynb: train baseline U-Net (1-channel input).

Unet_CeNN.ipynb: train CeNN + U-Net (3-channel input).

- plots/
Scripts / notebooks to evaluate the trained models and plot Dice/IoU curves.

Evaluation.ipynb: load the best checkpoints and compute test-set metrics
(Dice, IoU, sensitivity, specificity).

- results/ 
Saved checkpoints and plots (not committed if they are too large).

# How to run
1. Prepare data
- Download LUNA16 subsets and annotations.
- Run the notebooks in data_prep/ to:
    Extract 2D slices and masks for the baseline,
    Build CeNN-enhanced 3-channel inputs for the CeNN + U-Net model.

2. Train baseline U-Net
- Open train_codes/Unet_Baseline.ipynb in Colab.
- Set the paths to the prepared data.
- Run all cells to train the model.
    The best model is saved as e.g. Results/Unet/.../sumnet_best.pt.

3. Train CeNN + U-Net
- Open train_codes/Unet_CeNN.ipynb.
- Point it to the CeNN-enhanced dataset.
- Run all cells to train the model and save its best checkpoint.

4. Evaluate
- Open plots/Evaluation.ipynb.
- Load both checkpoints (baseline and CeNN + U-Net).
- Run the evaluation cells to compute Dice, IoU, sensitivity, specificity,
and the Wilcoxon p-value.

# Main result
Final test-set performance:

| Method         | Dice (mean ± std) | IoU (mean ± std) | Sensitivity | Specificity |
| -------------- | ----------------- | ---------------- | ----------- | ----------- |
| U-Net baseline | 0.51 ± 0.37       | 0.42 ± 0.33      | 0.50        | 0.99        |
| CeNN + U-Net   | 0.47 ± 0.38       | 0.40 ± 0.34      | 0.48        | 0.99        |

The paired Wilcoxon signed-rank test on per-patient Dice scores gives 
p = 0.176
so the difference is not statistically significant at the 5% level.

# Acknowledgements
- LUNA16 organizers for providing the dataset.
- The original LUNA16 baseline code and CeNN references used as a starting point in this assignment.




