# Explainable-AI-for-Automated-Coffee-Bean-Quality-Assessment
An AI-powered visual inspection system that detects and segments coffee bean defects (Broken, Black, Insect Damage), quantifying their severity and providing explainable decisions for quality control.

                        ┌─────────────────────────┐
            │   Coffee Bean Image     │
            │ (Camera / Dataset)      │
            └─────────────┬───────────┘
                          │
                          ▼
            ┌─────────────────────────┐
            │   Image Preprocessing   │
            │ Resize (224x224)        │
            │ Normalization           │
            │ Data Augmentation       │
            └─────────────┬───────────┘
                          │
                          ▼
            ┌─────────────────────────┐
            │  Binary Classification  │
            │   Good vs Defective     │
            │ CNN (ResNet/EfficientNet) │
            └───────────┬─────────────┘
                        │
         ┌──────────────┴──────────────┐
         │                             │
         ▼                             ▼
┌──────────────────────┐ ┌────────────────────────┐
│ Good Bean Branch     │ │ Defective Branch       │
└───────────┬──────────┘ └───────────┬─────────────┘
            │                          │
            ▼                          ▼
┌──────────────────────────┐ ┌──────────────────────────┐
│ Good Bean Type           │ │ Defect Type Classification│
│ Classification           │ │ CNN / EfficientNet       │
│ CNN / Vision Transformer │ │ Classes:                 │
│ Classes:                 │ │ - Structural Defects     │
│ - Longberry              │ │ - Color/Fermentation     │
│ - Peaberry               │ │ - Biological Damage      │
│ - Premium                │ │ - Immature Processing    │
│ - Green                  │ └───────────┬──────────────┘
│ - Roasted (Light/Medium/Dark)         │
└───────────┬──────────────┘            │
            │                             │
            ▼                             ▼
            │                  ┌─────────────────────┐
            │                  │ Defect Detection    │
            │                  │ YOLOv8 / Faster R-CNN │
            │                  │ Bounding Boxes      │
            │                  └─────────┬───────────┘
            │                            │
            │                            ▼
            │                  ┌─────────────────────┐
            │                  │ Defect Segmentation │
            │                  │ U-Net / Mask R-CNN  │
            │                  │ Pixel Defect Mask   │
            │                  └─────────┬───────────┘
            │                            │
            │                            ▼
            │                  ┌─────────────────────┐
            │                  │ Severity Estimation │
            │                  │ Defect Area Ratio   │
            │                  │ Defect Pixels /     │
            │                  │ Total Bean Pixels   │
            │                  └─────────┬───────────┘
            │                            │
            ▼                            ▼
┌────────────────────────────────────────────┐
│ Coffee Quality Prediction Model            │
│ Inputs:                                    │
│ - Bean Type (Longberry, Peaberry, etc.)   │
│ - Defect Type                              │
│ - Defect Severity                          │
│ - Color Features                           │
│ - Shape / Texture Features                 │
│ Model: Neural Network / Random Forest      │
│ Output: Quality Score / Grade              │
│ Example Grades: Specialty / Premium / Reject│
└──────────────┬─────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────┐
│      Explainable AI (Grad-CAM / LIME)    │
│ Highlights Important Regions of Image    │
│ Shows why model predicted defect / grade │
└──────────────────────────────────────────┘

