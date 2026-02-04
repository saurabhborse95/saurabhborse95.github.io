---
layout: post
title: "TTC Computation Using Camera and LiDAR in ADAS"
date: 2026-02-03
categories: [ADAS, Sensor Fusion]
cover: assets/images/ttc.png
emoji: 📡
read_time: 6 min read
---

## Time-to-Collision (TTC) Using Camera and LiDAR

**Time-to-Collision (TTC)** estimates the remaining time before ego vehicle collides with a preceding object, assuming constant velocity. It is a core metric in **ACC, AEB, and FCW** systems.

---

## Camera-based TTC Estimation

Camera-based TTC relies on **monocular vision geometry** and **temporal scale change**.

### Steps
1. Detect object (e.g. bounding box from YOLO / SSD)
2. Extract keypoints inside ROI (ORB, AKAZE, SIFT)
3. Match keypoints between consecutive frames
4. Compute distance ratios of matched keypoints
5. Estimate TTC from median scale change

### Formula
Let `d_i` be distance between keypoint pairs:

\[
TTC_{cam} = -\frac{\Delta t}{1 - \text{median}\left(\frac{d_{t}}{d_{t-1}}\right)}
\]

### Characteristics
- No absolute depth required
- Sensitive to:
  - Poor feature matches
  - Object rotation
  - Illumination changes

---

## LiDAR-based TTC Estimation

LiDAR provides **direct depth measurements**, enabling geometric TTC computation.

### Steps
1. Associate LiDAR points to detected object
2. Remove outliers (e.g. percentile or min-x filtering)
3. Compute closest longitudinal distance `d`
4. Estimate relative velocity from distance change

### Formula
\[
TTC_{LiDAR} = \frac{d}{\dot{d}}
\]

where `d` is object distance and `\dot{d}` is relative closing speed.

### Characteristics
- High depth accuracy
- Robust to lighting
- Limited resolution at long range

---

## Camera–LiDAR Sensor Fusion

Sensor fusion combines **semantic richness of camera** with **metric accuracy of LiDAR**.

### Fusion Strategy
- Camera: object detection, tracking, classification
- LiDAR: depth validation, motion estimation
- TTC fusion:
  - Outlier rejection
  - Confidence-weighted averaging
  - Kalman / Bayesian filtering

### Benefits
- Improved TTC stability
- Reduced false braking
- Better performance in complex traffic scenarios

---

## ADAS Perspective (Interview Ready)

- Camera TTC: scale-based, monocular, indirect depth
- LiDAR TTC: geometric, direct depth
- Fusion improves:
  - Robustness
  - Functional safety (ISO 26262)
  - Real-world reliability

**Used in:** ACC, AEB, FCW, Traffic Jam Assist

---

