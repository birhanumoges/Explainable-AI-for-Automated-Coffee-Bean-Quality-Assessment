# Explainable-AI-for-Automated-Coffee-Bean-Quality-Assessment
An AI-powered visual inspection system that detects and segments coffee bean defects (Broken, Black, Insect Damage), quantifying their severity and providing explainable decisions for quality control.


### Coffee Bean Visual Inspection System
                
                      ┌────────────────────────────┐
                │      Coffee Bean Image      │
                │     (Camera / Dataset)     │
                └─────────────┬─────────────┘
                              │
                              ▼
                  ┌────────────────────────────┐
                  │     Data Quality Check      │
                  │ - Detect corrupt / blurry   │
                  │ - Verify image resolution   │
                  │ - Remove duplicates         │
                  └─────────────┬─────────────┘
                                │
                                ▼
                  ┌────────────────────────────┐
                  │     Image Preprocessing     │
                  │ - Resize (224x224)         │
                  │ - Normalization             │
                  │ - Data Augmentation         │
                  │   • Flip / Rotation         │
                  │   • Brightness / Zoom       │
                  └─────────────┬─────────────┘
                                │
                                ▼
                      ┌────────────────────────────┐
                      │   Binary Classification     │
                      │     Good vs Defective       │
                      │ CNN (ResNet / EfficientNet) │
                      └─────────────┬─────────────┘
                                    │
                   ┌────────────────┴─────────────────┐
                   │                                  │
                   ▼                                  ▼

           ┌─────────────────────────┐       ┌────────────────────────────┐
           │     Good Bean Branch     │       │     Defective Branch       │
           └────────────┬────────────┘       └─────────────┬──────────────┘
                        │                                   │
                        ▼                                   ▼

          ┌────────────────────────────┐     ┌─────────────────────────────┐
          │ Good Bean Type Classification │   │ Defect Type Classification  │
          │ - CNN / Vision Transformer   │   │ - CNN / EfficientNet        │
          │ Classes:                     │   │ Classes:                    │
          │   • Specialty (Longberry, Peaberry, Premium) │  │   • Structural Damage     │
          │   • Commercial               │   │   • Biological Damage       │
          │   • Light Roast              │   │   • Color / Fermentation Defects │
          │   • Dark Roast               │   │   • Immature / Processing Defects │
          │                               │   │   • Processing Layer Defects      │
          └─────────────┬───────────────┘     └─────────────┬──────────────────┘
                        │                                   │
                        ▼                                   ▼

          ┌────────────────────────────┐     ┌─────────────────────────────┐
          │ Feature Extraction Module  │     │ Defect Detection Module      │
          │ - Shape (length/width)     │     │ - YOLOv8 / FasterRCNN        │
          │ - Color Histogram          │     │ - Bounding Boxes             │
          │ - Texture (smoothness, variance) │ └─────────────┬─────────────┘
          │ - Surface Uniformity       │                   │
          └─────────────┬─────────────┘                   ▼
                        │                        ┌─────────────────────────┐
                        ▼                        │ Defect Segmentation      │
          ┌────────────────────────────┐        │ - U-Net / Mask R-CNN     │
          │ Coffee Quality Prediction   │        │ - Pixel-level defect masks│
          │ Model                        │        └─────────────┬─────────┘
          │ Inputs:                      │                  │
          │ - Bean Type                  │                  ▼
          │ - Defect Type                │        ┌─────────────────────────┐
          │ - Shape / Texture Features   │        │ Defect Severity Scoring  │
          │ Model: Neural Network / RandomForest │ │ - Pixel % coverage      │
          │ Output Example Grades: Premium / Reject │ │ - Defect severity weight│
          └─────────────┬─────────────┘        └─────────────┬─────────┘
                        │                                  │
                        ▼                                  ▼

                  ┌─────────────────────────────────────────────┐
                  │ Explainable AI (Grad-CAM / LIME)           │
                  │ - Shows which features / defects influenced│
                  │   prediction for bean grade / defect       │
                  └─────────────┬─────────────────────────────┘
                                │
                                ▼
                  ┌─────────────────────────────────────────────┐
                  │ Streamlit Dashboard Integration             │
                  │ Pages:                                      │
                  │ - Upload & Preview Images                    │
                  │ - Prediction Results                         │
                  │ - Class Distribution / Charts                │
                  │ - Defect Severity Visualization              │
                  │ - Prediction History / Logs                  │
                  └─────────────────────────────────────────────┘

