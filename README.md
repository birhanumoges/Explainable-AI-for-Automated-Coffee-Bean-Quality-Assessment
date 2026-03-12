# Explainable-AI-for-Automated-Coffee-Bean-Quality-Assessment
An AI-powered visual inspection system that detects and segments coffee bean defects (Broken, Black, Insect Damage), quantifying their severity and providing explainable decisions for quality control.

```
### Coffee Bean Visual Inspection System
                ┌─────────────────────────┐
                │   Coffee Bean Image     │
                │ (Camera / Dataset)      │
                └─────────────┬───────────┘
                              │
                              ▼
                  ┌─────────────────────────┐
                  │   Image Preprocessing   │
                  │ Resize (224x224)       │
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

               ┌──────────────────────┐     ┌────────────────────────┐
               │   Good Bean Branch   │     │    Defective Branch     │
               └───────────┬──────────┘     └───────────┬─────────────┘
                           │                            │
                           ▼                            ▼
            
            ┌──────────────────────────┐     ┌──────────────────────────┐
            │ Good Bean Type           │     │ Defect Type Classification│
            │ Classification           │     │ CNN / EfficientNet        │
            │ CNN / Vision Transformer │     │                          │
            │ Classes:                 │     │ broken                   │
            │ - Longberry              │     │ full soure               │
            │ - Peaberry               │     │ insect damege            │
            │ - Premium                │     │ Immature       │
            │ -                  │     │ husk         │
            │ -  │
            └───────────┬──────────────┘     └───────────┬──────────────┘
                        │                                │
                        ▼                                ▼

        (Optional)                     ┌─────────────────────┐
                                      │ Defect Detection    │
                                      │ YOLOv8 / FasterRCNN │
                                      │ Bounding Boxes      │
                                      └─────────┬───────────┘
                                                │
                                                ▼

                                      ┌─────────────────────┐
                                      │ Defect Segmentation │
                                      │ U-Net / Mask R-CNN  │
                                      │ Pixel Defect Mask   │
                                      └─────────┬───────────┘
                                                │
                                                ▼

                                      ┌─────────────────────┐
                                      │                     │
                                      │                     │
                                      │                     │
                                      │ Total Bean Pixels   │
                                      └─────────┬───────────┘
                                                │
                                                ▼

                ┌────────────────────────────────────────────┐
                │ Coffee Quality Prediction Model             │
                │ Inputs:                                     │
                │ - Bean Type (Longberry, Peaberry, etc.)    │
                │ - Defect Type                               │
                │                              │
                │ - Shape / Texture Features                   │
                │ Model: Neural Network / Random Forest       │
                │                │
                │ Example Grades: / Premium / Reject│
                └──────────────┬─────────────────────────────┘
                               │
                               ▼

                ┌──────────────────────────────────────────┐
                │      Explainable AI (Grad-CAM / LIME)    │
                │   │
                │ Shows why model predicted defect / grade │
                └──────────────────────────────────────────┘
```

