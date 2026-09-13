# Robust Object Detection and Tracking under Challenging Conditions

A computer vision project focused on robust object detection and multi-object tracking in aerial imagery using YOLO26n, the VisDrone dataset, and ByteTrack.

## Overview

Object detection in aerial images is challenging because objects are often small, densely packed, partially occluded, and captured from varying viewpoints.

This project investigates a YOLO26n-based detection pipeline on the VisDrone dataset and extends it with multi-object tracking using ByteTrack.

The project includes:

- YOLO26n object detection
- Transfer from pretrained weights
- 30-epoch training experiment
- Quantitative evaluation using Precision, Recall, mAP50, and mAP50-95
- Per-class performance analysis
- Error analysis
- Multi-object tracking with ByteTrack

## Dataset

The project uses the **VisDrone** dataset for aerial object detection.

The dataset contains 10 object categories:

- pedestrian
- people
- bicycle
- car
- van
- truck
- tricycle
- awning-tricycle
- bus
- motor

Training set: **6,471 images**

Validation set: **548 images**

The validation set contains **38,759 annotated objects**.

## Model

### YOLO26n

The detection model used in this project is **YOLO26n**, initialized from pretrained weights.

Model characteristics:

- Parameters: ~2.5M
- GFLOPs: ~5.9
- Input resolution: 640 × 640
- Batch size: 8
- GPU: NVIDIA Tesla T4
- Training epochs: 30

## Experimental Setup

The initial baseline was trained for 10 epochs.

A second experiment continued training from the best baseline checkpoint for 30 epochs.

### Training configuration

| Parameter | Value |
|---|---|
| Model | YOLO26n |
| Dataset | VisDrone |
| Image size | 640 × 640 |
| Batch size | 8 |
| Epochs | 30 |
| Optimizer | AdamW |
| GPU | NVIDIA Tesla T4 |
| Framework | Ultralytics |

## Results

### Baseline vs. 30 Epochs

| Metric | Baseline | 30 Epochs |
|---|---:|---:|
| Precision | 0.345 | **0.411** |
| Recall | 0.256 | **0.313** |
| mAP50 | 0.229 | **0.292** |
| mAP50-95 | 0.123 | **0.160** |

The longer training experiment improved all four main evaluation metrics.

The mAP50 increased from **0.229 to 0.292**, while mAP50-95 increased from **0.123 to 0.160**.

## Per-Class Performance

The final model showed substantial differences between object categories.

| Class | mAP50 |
|---|---:|
| pedestrian | 0.319 |
| people | 0.232 |
| bicycle | 0.061 |
| car | **0.708** |
| van | 0.334 |
| truck | 0.267 |
| tricycle | 0.191 |
| awning-tricycle | 0.109 |
| bus | 0.374 |
| motor | 0.325 |

The model performs particularly well on **cars**, while small and less frequent object categories such as bicycles and awning-tricycles remain challenging.

## Error Analysis

The error analysis focuses on typical difficulties in aerial object detection:

- Small objects
- Dense object distributions
- Occlusion
- Similar-looking object categories
- Class imbalance
- Objects appearing at different scales

These factors contribute to the lower performance of several small-object categories.

## Object Tracking

The trained YOLO26n detector was combined with **ByteTrack** for multi-object tracking.

The tracking pipeline assigns consistent IDs to detected objects across consecutive video frames.

This extends the project from static object detection to a more practical video-based computer vision system.

## Pipeline

```text
VisDrone Dataset
       ↓
Data Preparation
       ↓
YOLO26n
       ↓
Object Detection
       ↓
Evaluation
       ↓
Error Analysis
       ↓
ByteTrack
       ↓
Multi-Object Tracking
