# Rapid Flood Mapping Using Deep Learning and SAR

This project focuses on automating flood detection and mapping using Convolutional Neural Networks (CNNs) trained on Sentinel-1 (SAR) and Sentinel-2 (Optical) satellite imagery. The goal is to rapidly and accurately identify flooded regions to support disaster response and resilience planning.

---

## Project Overview

### Models Developed

1. *SAR Flood Detection Model (Sentinel-1):*
   - Designed to work in cloudy or nighttime conditions.
   - Uses 2 channels (VV and VH polarization).

2. *Optical Flood Detection Model (Sentinel-2):*
   - Utilizes 12 multispectral bands.
   - Provides complementary information and validation.

Both models perform *binary classification*:
- 1 → Flood  
- 0 → No Flood

---

## Workflow Summary

### Core Architecture

- *Backbone:* ResNet50V2 (pre-trained on ImageNet)
- *Input Shape:* (256 × 256 × num_channels)
- *Dynamic Channels:* 2 for SAR, 12 for Optical
- *Global Average Pooling (GAP):* Replaces fully connected layers to reduce overfitting.
- *Classifier Head:* Batch Normalization → Dense(512) → Dropout(0.2) → Softmax(2)
- *Loss Function:* Sparse Categorical Crossentropy

---

## Execution Order (Step-by-Step)

### Step 1: Data Integrity Check
- Verify that each label JSON has a corresponding image directory.
- Ensure SAR and Optical datasets are complete and consistent.

### Step 2: Image Loading and Normalization
- *Sentinel-1 (SAR):*
  - Normalize VV by dividing by 50.
  - Normalize VH by dividing by 100.
  - Resize all images to 256×256 pixels.
- *Sentinel-2 (Optical):*
  - Normalize all bands by dividing by 10,000.
  - Resize all images to 256×256 pixels.

### Step 3: Metadata DataFrame Creation
- Build a metadata table including:
  - Image paths
  - Labels
  - Tile numbers
  - Dates and location IDs
- Remove any optical samples missing bands to ensure data consistency.

### Step 4: Train-Validation Split
- Perform stratified splitting based on location_id.
- Ensures balanced flood and non-flood samples across locations.

### Step 5: Model Initialization
- Load ResNet50V2 backbone.
- Freeze base layers initially (optional for transfer learning).
- Add a custom classifier head: BatchNorm → Dense(512) → Dropout(0.2) → Softmax(2).

### Step 6: Data Pipeline Setup using TensorFlow tf.data
- Create efficient pipelines for data loading and preprocessing.
- Include:
  - *Caching:* Stores dataset in memory after first epoch to prevent repeated disk I/O.
  - *Prefetching:* Ensures GPU utilization by overlapping data loading and training.
  - *Shuffling:* Randomizes data order to prevent learning sequences.
  - *Batching:* Groups samples to optimize GPU processing.

### Step 7: Data Augmentation (for Sentinel-2)
- Apply random transformations to increase data variability:
  - Random horizontal and vertical flips
  - Random cropping
  - Random 90-degree rotations

### Step 8: Model Training
- Configure training parameters:
  - *Batch Size:* 9 for SAR, 3 for Optical (due to memory limits)
  - *Learning Rate:* 0.001
  - *Optimizer:* Adam
  - *Loss Function:* Sparse Categorical Crossentropy
  - *Epochs:* 75
- Implement callbacks:
  - *EarlyStopping:* Stop training when validation accuracy stops improving.
  - *ReduceLROnPlateau:* Reduce learning rate when metrics plateau.
  - *LearningRateScheduler:* Apply exponential decay to learning rate.

### Step 9: Evaluation
- Evaluate model using:
  - *Accuracy:* Overall correctness.
  - *Precision:* How many predicted floods are actual floods.
  - *Recall:* How many actual floods were correctly detected.
  - *F1-Score:* Harmonic mean of precision and recall.

### Step 10: Prediction and Visualization
- Generate predictions on unseen tiles.
- Output binary maps and confidence scores.
- Overlay predicted flood areas on geospatial maps for visualization.

---

## Hyperparameter Configuration

| Parameter | SAR Model | Optical Model |
|------------|------------|----------------|
| Batch Size | 9 | 3 |
| Learning Rate | 0.001 | 0.001 |
| Input Channels | 2 (VV/VH) | 12 (Bands) |
| Optimizer | Adam | Adam |
| Epochs | 75 | 75 |
| Dropout | 0.2 | 0.2 |
| Loss Function | Sparse Categorical Crossentropy | Sparse Categorical Crossentropy |

---

## Code Flow Diagram

```mermaid
flowchart LR
    %% ---------- TOP ROW ----------
    A[Start] --> B[Data Integrity Check]
    B --> C[Image Loading and Normalization]
    C --> D[Metadata and Label Preparation]
    D --> E[Train-Validation Split]
    E --> F[Model Initialization and Architecture Setup]

    %% ---------- BOTTOM ROW ----------
    F --> G[Build TensorFlow Data Pipeline]
    G --> H[Data Augmentation]
    H --> I[Model Training]
    I --> J[Performance Evaluation]
    J --> K[Generate Predictions]
    K --> L[Flood Map Visualization]
    L --> M[End]

    %% ---------- LAYOUT TWEAKS ----------
    %%  Force two visible tiers by linking D down to G visually
    D -.-> G
    style A fill:#e0e0e0,stroke:#000,stroke-width:1px
    style M fill:#e0e0e0,stroke:#000,stroke-width:1px
