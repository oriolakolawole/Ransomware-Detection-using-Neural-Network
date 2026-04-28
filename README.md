# Ransomware Detection & Family Classification via PE Header Visualisation

A deep learning project that detects ransomware and identifies its family by converting **Windows PE header bytes into images** and applying transfer learning with **ResNet50, Xception, and VGG16**.

---

## Core Idea

Each binary executable's PE header (first 1024 bytes) is reshaped into a **32×32 grayscale image**, upscaled to **256×256 RGB**, and fed into pre-trained CNN architectures. The intuition is that malware families share structural byte patterns in their headers that manifest as visually distinguishable textures — making image-based classification a viable approach.

```
PE Header (1024 bytes) → reshape (32×32) → normalise → RGB (256×256) → CNN
```

> [Click Here to view this Project](dataset1n.ipynb)

---

## Notebooks

| Notebook | Task | Output Classes |
|---|---|---|
| `dataset1n.ipynb` | Binary detection | 2 classes |
| `dataset1family.ipynb` | Multi-class family identification | 26 classes (25 families + goodware) |

---

## Dataset

**2,157 binary executable samples:**

| Class | Count | ID Range | Filename |
|---|---|---|---|
| Goodware | 1,134 | 10000–11133 | `.exe` name |
| Ransomware | 1,023 | 20000–21022 | SHA-256 hash |

**25 Ransomware Families:** Avaddon, Babuk, Blackmatter, Conti, Darkside, Dharma, Doppelpaymer, Exorcist, Gandcrab, Lockbit, Makop, Maze, Mountlocker, Nefilim, Netwalker, Phobos, Pysa, Ragnarok, RansomeXX, Revil, Ryuk, Stop, Thanos, Wastedlocker, Zeppelin.

**Train / Validation / Test split:** 70% / 20% / 10%

> In `dataset1family.ipynb`, the split is performed **per family** to ensure all 26 classes are proportionally represented across all sets.

---

## Models Compared

Three ImageNet pre-trained CNNs are fine-tuned (frozen base + custom classification head) for each task:

| Model | Binary Head | Family Head | 
|---|---|---|---|
| ResNet50 | Dense(128) → Dense(64) → Dense(1, sigmoid) | Dense(128) → Dense(64) → Dense(26, softmax) |
| Xception | Dense(256) → Dense(128) → Dense(64) → Dense(1, sigmoid) | Dense(256) → Dense(128) → Dense(64) → Dense(26, softmax) |
| VGG16 | Dense(128) → Dense(64) → Dense(1, sigmoid) | Dense(256) → Dense(128) → Dense(64) → Dense(26, softmax) | 

---

## Evaluation Metrics

Each model is evaluated with a comprehensive set of metrics:

- Accuracy, Precision, Recall, F1-Score
- False Negative Rate (FNR) & False Positive Rate (FPR)
- Matthews Correlation Coefficient (MCC)
- Cohen's Kappa
- AUC-ROC
- Confusion matrix heatmap

---

## Installation

```bash
pip install numpy pandas matplotlib seaborn scikit-learn tensorflow
```

> A **GPU is strongly recommended**. Training three models at 256×256 resolution on ~2,000 samples can be slow on CPU.

---

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   cd your-repo-name
   ```
2. Place `Ransomware_headers.csv` in the project root.
3. Install dependencies (see above).
4. Run the binary detection notebook:
   ```bash
   jupyter notebook dataset1n.ipynb
   ```
5. Run the family classification notebook:
   ```bash
   jupyter notebook dataset1family.ipynb
   ```

---

## Dependencies

| Package | Purpose |
|---|---|
| `tensorflow` | Model building, transfer learning, image resizing |
| `numpy` / `pandas` | Data manipulation and array operations |
| `matplotlib` / `seaborn` | Visualisation and confusion matrix heatmaps |
| `scikit-learn` | Train/test split and evaluation metrics |

---

## Repository Structure

```
.
├── dataset1n.ipynb             # Binary detection: goodware vs ransomware
├── dataset1family.ipynb        # Multi-class: ransomware family identification
├── Ransomware_headers.csv      # PE header dataset
└── README.md                   # This file
```

---

## Disclaimer

This project is for **research and educational purposes only**. The models are trained on a specific dataset and should not be used as a standalone malware detection tool in production environments.
