# Sheep Breed Image Classification using Deep Learning

This repository contains the implementation and comparative analysis of three prominent Deep Learning architectures—**ResNet18**, **DenseNet121**, and **EfficientNet_B0**—applied to the task of fine-grained image classification on the **Sheep Image Classification Dataset**. 

Unlike previous sheep classification project leveraging deeper networks, this study explores lighter, highly efficient architectural variations optimized for resource-constrained environments while dealing with highly subtle inter-class visual similarities in livestock data.


---

## 📌 Project Overview
Automated identification of sheep breeds streamlines population recording and health monitoring for farmers, enhancing economic management. This project treats the classification of sheep muzzle/face profiles as a benchmark to evaluate how different structural design paradigms (residual learning, dense connectivity, and compound scaling) handle complex fine-grained visual differences under varying conditions.

### 📊 Dataset Details
* **Source:** [Sheep Image Classification Dataset](https://images.cv/dataset/sheep-image-classification-dataset)
* **Structure:** Already structured into `train`, `val`, and `test` subsets.
* **Classes (4 Breeds):** * Marino
  * Poll Dorset
  * Suffolk
  * White Suffolk
* **Characteristics:** The dataset features right-side profile images of sheep heads. Challenges include massive inter-class similarities (especially among white-wooled breeds like Poll Dorset and White Suffolk), diverse background environments (pastures vs. barns), and varying illumination profiles.

---

## 🛠️ Data Preprocessing & Augmentation
To ensure uniform inputs and improve model generalization, the pipeline employs `torchvision.transforms`:
1. **Resize:** Standardized to $224 \times 224$ pixels to fit pretrained model requirements.
2. **Data Augmentation:** `RandomHorizontalFlip` applied exclusively to the training set to boost orientation invariance.
3. **Conversion:** `ToTensor()` to map pixel ranges to $[0.0, 1.0]$.
4. **Normalization:** Normalized using ImageNet channel statistics ($\mu = [0.485, 0.456, 0.406]$, $\sigma = [0.229, 0.224, 0.225]$).
5. **DataLoader:** Managed in mini-batches of size `32` (with training data shuffled every epoch).

---

## 🏗️ Model Architectures & Training Strategy
We fine-tuned three distinct architectural archetypes by replacing their final fully connected/classifier layers to match our 4 target classes:

* **ResNet18:** Lightweight residual network utilizing skip connections to combat vanishing gradients. Trained via **Adam** ($lr=0.001$).
* **DenseNet121:** Implements dense connectivity where layers receive concatenated features from all preceding layers, maximizing feature reuse. Trained via **SGD** ($lr=0.01$, $\text{momentum}=0.9$).
* **EfficientNet_B0:** Leverages composite scaling (propotionally balancing depth, width, and input resolution). Trained via **Adam** ($lr=0.001$).

### Training Hyperparameters
* **Loss Function:** `CrossEntropyLoss()`
* **Epochs:** Max 40 epochs with an **Early Stopping** mechanism ($\text{patience}=5$) tracking validation loss.
* **Fine-tuning:** All layers set as trainable to allow features to adapt to the highly specific livestock traits.

---

## 📈 Evaluation & Experimental Results

The models delivered strong performance across the board, with **DenseNet121** emerging as the top performer due to its superior feature propagation capabilities on this specific dataset layout.

### Performance Summary

| Model | Val Accuracy | Val Loss | Test Accuracy | Test Loss |
| :--- | :---: | :---: | :---: | :---: |
| **ResNet18** | 92.22% | 0.2942 | 95.51% | 0.1700 |
| **DenseNet121** | **98.20%** | **0.0360** | **97.65%** | **0.0996** |
| **EfficientNet_B0** | 96.41% | 0.1057 | 95.09% | 0.1283 |

### Key Findings
* **DenseNet121** achieved outstanding generalization, displaying highly stable convergence and the lowest overall error profile ($\text{Val Loss} = 0.0360$).
* **EfficientNet_B0** acts as an ideal deployment alternative. It offers a highly competitive $95.09\%$ test accuracy while maintaining a drastically lighter parameter and computational footprint.
* **ResNet18** suffered from high validation loss volatility and minor overfitting behaviors during intermediate training stages, proving useful mainly as a quick performance baseline.

---

## 💻 Repository Structure
```text
├── dataset/               # Structured train, val, and test data folders
├── DLIA_Report.pdf        # Detailed project documentation and analysis
├── Source_Code.ipynb      # Complete PyTorch training and evaluation notebook
└── README.md              # Project overview
