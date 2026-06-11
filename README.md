# Explainable-AI-for-Automated-Coffee-Bean-Quality-Assessment
An AI-powered visual inspection system that detects and segments coffee bean defects (Broken, Black, Insect Damage), quantifying their severity and providing explainable decisions for quality control.


---

## 🗺️ System Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    COFFEE BEAN IMAGE INPUT                        │
│                   (Camera / Dataset Upload)                       │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                      DATA QUALITY CHECK                           │
│   • Detect corrupt or blurry images                               │
│   • Verify image resolution standards                             │
│   • Remove duplicate entries                                      │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                     IMAGE PREPROCESSING                           │
│   • Resize to 224×224                                             │
│   • Normalize pixel values                                        │
│   • Data Augmentation:                                            │
│       - Flip / Rotation                                           │
│       - Brightness adjustment / Zoom                              │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│               BINARY CLASSIFICATION (Stage 1)                     │
│                    Good  vs  Defective                            │
│              Model: CNN (ResNet / EfficientNet)                   │
└───────────────────────┬──────────────────────┬───────────────────┘
                        │                      │
              ┌─────────┘                      └──────────┐
              │                                           │
              ▼                                           ▼
┌─────────────────────────────┐         ┌─────────────────────────────┐
│       ✅ GOOD BEAN BRANCH    │         │     ❌ DEFECTIVE BEAN BRANCH  │
└──────────────┬──────────────┘         └──────────────┬──────────────┘
               │                                       │
               ▼                                       ▼
┌─────────────────────────────┐         ┌─────────────────────────────┐
│  Good Bean Type Classifier  │         │   Defect Type Classifier     │
│  Model: CNN / ViT            │         │   Model: CNN / EfficientNet  │
│                              │         │                              │
│  Classes:                    │         │  Classes:                    │
│  • Specialty                 │         │  • Structural Damage         │
│    (Longberry, Peaberry,     │         │  • Biological Damage         │
│     Premium)                 │         │  • Color / Fermentation      │
│  • Commercial                │         │  • Immature / Processing     │
│  • Light Roast               │         │  • Processing Layer Defects  │
│  • Dark Roast                │         │                              │
└──────────────┬──────────────┘         └──────────────┬──────────────┘
               │                                       │
               ▼                                       ▼
┌─────────────────────────────┐         ┌─────────────────────────────┐
│  Feature Extraction Module  │         │   Defect Detection Module    │
│  • Shape (length / width)   │         │   Model: YOLOv8 / FasterRCNN │
│  • Color Histogram          │         │   • Bounding box localization│
│  • Texture (smoothness,     │         └──────────────┬──────────────┘
│    variance)                │                        │
│  • Surface Uniformity       │                        ▼
└──────────────┬──────────────┘         ┌─────────────────────────────┐
               │                        │   Defect Segmentation        │
               │                        │   Model: U-Net / Mask R-CNN  │
               │                        │   • Pixel-level defect masks │
               │                        └──────────────┬──────────────┘
               │                                       │
               │                                       ▼
               │                        ┌─────────────────────────────┐
               │                        │   Defect Severity Scoring    │
               │                        │   • Pixel coverage %         │
               │                        │   • Severity weight mapping  │
               │                        └──────────────┬──────────────┘
               │                                       │
               └──────────────────┬────────────────────┘
                                  │
                                  ▼
┌──────────────────────────────────────────────────────────────────┐
│                  COFFEE QUALITY PREDICTION MODEL                  │
│                                                                   │
│  Inputs:                                                          │
│  • Bean type class                                                │
│  • Defect type & severity score                                   │
│  • Shape / texture feature vector                                 │
│                                                                   │
│  Model: Neural Network / Random Forest                            │
│                                                                   │
│  Output Grades:   🥇 Premium  │  🟡 Standard  │  🔴 Reject         │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                  EXPLAINABLE AI (XAI) LAYER                       │
│   • Grad-CAM — highlights regions influencing prediction          │
│   • LIME     — local feature importance explanation               │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                  STREAMLIT DASHBOARD                               │
│                                                                   │
│   📤  Upload & Preview Images                                     │
│   📊  Prediction Results & Grade Output                           │
│   📈  Class Distribution Charts                                   │
│   🗺️  Defect Severity Visualization (Heatmaps)                   │
│   🕓  Prediction History & Logs                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔍 Pipeline Stages

### Stage 1 — Data Quality Check
Before any model sees the data, images are screened for corruption, blur, and resolution standards. Duplicates are removed to prevent data leakage.

### Stage 2 — Image Preprocessing
All images are resized to **224×224** and normalized. Augmentation (flip, rotation, brightness, zoom) is applied during training to improve generalization.

### Stage 3 — Binary Classification
A CNN (ResNet or EfficientNet backbone) separates beans into two branches: **Good** and **Defective**.

### Stage 4 — Sub-Classification (Parallel Branches)

| Branch | Model | Output Classes |
|---|---|---|
| Good Bean | CNN / Vision Transformer | Specialty (Longberry, Peaberry, Premium), Commercial, Light Roast, Dark Roast |
| Defective Bean | CNN / EfficientNet | Structural, Biological, Color/Fermentation, Immature/Processing, Processing Layer |

### Stage 5 — Feature Extraction & Defect Analysis

| Module | Technique | Output |
|---|---|---|
| Feature Extraction | Shape, Color Histogram, Texture | Numeric feature vector |
| Defect Detection | YOLOv8 / Faster R-CNN | Bounding box coordinates |
| Defect Segmentation | U-Net / Mask R-CNN | Pixel-level defect masks |
| Severity Scoring | Pixel coverage + weights | Severity score (0–1) |

### Stage 6 — Quality Grade Prediction
A Neural Network or Random Forest fuses bean type, defect type, defect severity, and shape/texture features to output a final grade: **Premium**, **Standard**, or **Reject**.

### Stage 7 — Explainability (XAI)
Grad-CAM and LIME highlight which image regions and features drove each prediction, making the system auditable and interpretable.

### Stage 8 — Streamlit Dashboard
An interactive web app for uploading images, viewing predictions, exploring charts, and reviewing history.

---

## 🛠️ Tech Stack

```bash
pip install torch torchvision timm ultralytics segmentation-models-pytorch \
            scikit-learn opencv-python matplotlib seaborn streamlit lime grad-cam
```

| Category | Tools |
|---|---|
| Deep Learning | PyTorch, TorchVision, timm (ViT) |
| Object Detection | YOLOv8 (Ultralytics), Faster R-CNN |
| Segmentation | U-Net, Mask R-CNN (`segmentation-models-pytorch`) |
| Classical ML | scikit-learn (Random Forest) |
| Image Processing | OpenCV, Pillow |
| Explainability | Grad-CAM, LIME |
| Dashboard | Streamlit |
| Visualization | Matplotlib, Seaborn |

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/your-username/coffee-bean-classifier.git
cd coffee-bean-classifier
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Prepare your dataset
```
data/
├── good/
│   ├── specialty/
│   ├── commercial/
│   ├── light_roast/
│   └── dark_roast/
└── defective/
    ├── structural_damage/
    ├── biological_damage/
    ├── color_fermentation/
    ├── immature_processing/
    └── processing_layer/
```

### 4. Train the models
```bash
# Binary classifier
python train_binary.py --epochs 50 --backbone resnet50

# Good bean classifier
python train_good_beans.py --epochs 30

# Defect classifier
python train_defects.py --epochs 30

# YOLOv8 defect detection
python train_yolo.py --data data.yaml --epochs 100
```

### 5. Launch the dashboard
```bash
streamlit run app.py
```

---

## 📊 Output Grades

| Grade | Description |
|---|---|
| 🥇 **Premium** | No defects, specialty-type bean, uniform shape and texture |
| 🟡 **Standard** | Minor defects or commercial-grade bean |
| 🔴 **Reject** | Significant structural, biological, or fermentation defects |

---

## 📁 Project Structure

```
coffee-bean-classifier/
│
├── data/                        # Dataset directory
├── models/
│   ├── binary_classifier.py     # Good vs Defective CNN
│   ├── good_bean_classifier.py  # Bean type sub-classifier
│   ├── defect_classifier.py     # Defect type sub-classifier
│   ├── yolo_detector.py         # YOLOv8 bounding box detection
│   ├── segmentation.py          # U-Net / Mask R-CNN segmentation
│   └── quality_predictor.py     # Final grade prediction model
│
├── utils/
│   ├── preprocessing.py         # Resize, normalize, augment
│   ├── feature_extraction.py    # Shape, color, texture features
│   ├── severity_scoring.py      # Defect severity calculator
│   └── explainability.py        # Grad-CAM & LIME utilities
│
├── app.py                       # Streamlit dashboard
├── train_binary.py
├── train_good_beans.py
├── train_defects.py
├── train_yolo.py
├── requirements.txt
└── README.md
```

---

## 🔬 Model Summary

| Stage | Model | Task |
|---|---|---|
| Binary Classifier | ResNet50 / EfficientNet-B4 | Good vs Defective |
| Good Bean Classifier | EfficientNet / ViT | 4-class bean type |
| Defect Classifier | EfficientNet-B3 | 5-class defect type |
| Defect Detector | YOLOv8 / Faster R-CNN | Bounding box detection |
| Defect Segmentor | U-Net / Mask R-CNN | Pixel-level masking |
| Quality Predictor | Neural Network / Random Forest | Premium / Standard / Reject |
| Explainability | Grad-CAM + LIME | Feature attribution |

---

