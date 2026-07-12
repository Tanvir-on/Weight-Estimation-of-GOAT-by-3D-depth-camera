# Weight-Estimation-of-GOAT-by-3D-depth-camera
# 🐐 Goat3D: Automated 3D Goat Morphometric Measurement Using RGB-D Imaging and YOLO Pose Estimation

## Introduction

Livestock body measurements are fundamental indicators of an animal's growth, health status, productivity, and breeding potential. Traditionally, these measurements are obtained manually using measuring tapes or calipers, a process that is labor-intensive, time-consuming, stressful for animals, and prone to human error. Recent advances in computer vision and depth sensing provide an opportunity to automate this process with high accuracy and consistency.

**Goat3D** is an RGB-D based computer vision framework developed to automatically estimate the three-dimensional body dimensions of goats using deep learning and point cloud analysis. The system combines RGB images, depth information captured by an Intel RealSense camera, YOLO-based pose estimation, and Open3D point cloud processing to obtain precise anatomical measurements without any physical contact with the animal.

The framework demonstrates how two-dimensional anatomical keypoints detected from RGB images can be accurately projected into three-dimensional space, allowing automatic estimation of body length, body width, and body height. The project is designed as a foundation for precision livestock farming, digital phenotyping, and automated livestock monitoring systems.

---

# Motivation

Body morphometric traits play a significant role in:

* Genetic selection
* Breed improvement
* Animal growth monitoring
* Weight estimation
* Health assessment
* Precision livestock farming

Despite their importance, collecting these measurements manually is impractical for large farms due to the significant amount of labor required. An automated, non-contact measurement system can greatly reduce human effort while improving measurement repeatability and animal welfare.

This repository presents a complete pipeline that transforms RGB-D data into meaningful three-dimensional livestock measurements using artificial intelligence and geometric analysis.

---

# System Overview

The framework integrates several computer vision techniques into a unified processing pipeline.

The overall workflow consists of:

1. RGB image acquisition
2. Depth image acquisition
3. Point cloud generation
4. Goat pose estimation using YOLO
5. Extraction of anatomical keypoints
6. Conversion of 2D image coordinates into 3D coordinates
7. Ground plane estimation
8. Automatic computation of body measurements
9. Visualization of results

The system eliminates the need for manual annotation after the model has been trained and provides an entirely automatic measurement process.

---

# Methodology

## 1. RGB-D Data Acquisition

The dataset is captured using an Intel RealSense RGB-D camera that simultaneously records:

* RGB images
* Depth information
* Camera intrinsic parameters

The RGB image is used for pose estimation, while the depth image is converted into a three-dimensional point cloud that preserves the spatial geometry of the animal.

---

## 2. Goat Pose Estimation

A custom-trained YOLO pose estimation model detects six anatomically significant landmarks on the goat's body:

* Head
* Neck
* Body Middle
* Tail
* Left Body
* Right Body

These landmarks represent the primary structural points required for estimating body dimensions.

The detected keypoints are also connected using a predefined skeleton to provide a visual representation of the goat's posture.

---

## 3. 2D to 3D Coordinate Mapping

Each detected keypoint corresponds to a pixel location in the RGB image.

Since the RGB image and depth map are spatially aligned, every pixel has an associated three-dimensional coordinate stored inside the point cloud.

The framework reshapes the point cloud into a 640 × 480 organized structure so that each pixel directly corresponds to one 3D point.

For every detected keypoint:

Pixel Coordinate (x, y)

↓

Point Cloud Index

↓

(X, Y, Z)

This mapping transforms conventional image coordinates into real-world spatial coordinates measured in meters.

---

## 4. Point Cloud Processing

The generated point cloud is processed using Open3D.

Several operations are performed, including:

* Loading organized point clouds
* Coordinate transformation
* Z-axis correction
* Visualization
* Extraction of specific anatomical points

The repository also includes utilities for visualizing selected landmarks directly inside the three-dimensional point cloud.

---

## 5. Goat Point Cloud Segmentation

To isolate the goat from the surrounding environment, binary segmentation masks are applied to the point cloud.

The repository contains utilities that:

* Generate masks from segmentation labels
* Rename masks
* Project 3D points into the image plane
* Remove background points
* Export segmented goat point clouds

This significantly improves measurement accuracy by eliminating environmental noise.

---

# Morphometric Measurements

The framework automatically computes three important body dimensions.

## Body Length

Body length is calculated using Euclidean distance between the anatomical landmarks:

Neck → Body Middle

*

Body Middle → Tail

This provides a robust approximation of the animal's body length while accounting for slight body curvature.

---

## Body Width

Body width is estimated by measuring the lateral distance from:

Left Body → Body Middle

*

Body Middle → Right Body

The two distances are combined to estimate overall body width.

---

## Body Height

The framework estimates body height by first identifying the ground plane beneath the goat.

The vertical distance between the Body Middle landmark and the detected ground surface is then calculated as:

Body Height = |Ground Z − Body Middle Z|

This provides a reliable estimate of the animal's standing height without requiring manual intervention.

---

# Project Structure

```text
Goat3D/
│
├── pose_estimation.py      # Complete morphometric measurement pipeline
├── get_points.py           # YOLO pose inference and keypoint extraction
├── goat3dmasking.py        # Goat point cloud segmentation
├── 2Dto3D_overlap.py       # Mapping RGB pixels into 3D coordinates
├── ply_viewer.py           # Point cloud visualization
├── SDK43DCamera.py         # Camera intrinsic extraction
├── new.py                  # Manual coordinate selection utility
├── renamer.py              # Segmentation mask renaming
├── tiff2png.py             # TIFF image conversion
├── txt2binary.py           # Binary mask generation
├── best.pt                 # Trained YOLO pose model
├── Manual Data.csv         # Manual measurement reference
└── README.md
```

---

# Software Requirements

The project is developed in Python and relies on the following libraries:

* Python 3.10+
* Ultralytics YOLO
* OpenCV
* Open3D
* NumPy
* Intel RealSense SDK (pyrealsense2)

Install all dependencies using:

```bash
pip install ultralytics
pip install open3d
pip install opencv-python
pip install numpy
pip install pyrealsense2
```

---

# Running the Pipeline

Execute the main script:

```bash
python pose_estimation.py
```

The pipeline automatically:

* Loads the RGB image
* Detects goat keypoints
* Loads the corresponding point cloud
* Converts keypoints into 3D coordinates
* Estimates body length
* Estimates body width
* Estimates body height
* Prints the calculated measurements

---

# Applications

This framework can be applied to a wide range of livestock management and research tasks, including:

* Precision livestock farming
* Automated animal phenotyping
* Weight estimation
* Breed evaluation
* Growth monitoring
* Livestock digital twins
* Animal health monitoring
* Smart farming systems

---

# Future Work

Several enhancements can further improve the framework:

* Real-time processing from live RealSense streams
* Multi-goat detection and tracking
* Automatic body weight prediction using machine learning
* Body condition score estimation
* Surface mesh reconstruction
* Skeleton fitting algorithms
* Automatic ground plane estimation using RANSAC
* Integration with robotic livestock monitoring systems
* Cloud-based analytics dashboard
* Mobile application for field deployment

---

# Acknowledgements

This project was developed as part of ongoing research in precision livestock farming using RGB-D sensing, computer vision, and artificial intelligence. It demonstrates the integration of deep learning-based pose estimation with three-dimensional geometric analysis to automate livestock morphometric measurements accurately and efficiently.

---

# License

This project is released under the MIT License. You are free to use, modify, and distribute this software for academic and research purposes with proper attribution.

---

# Author

**Tanvir Hossain**

Department of Farm Power and Machinery

Bangladesh Agricultural University

**Research Interests**

* Precision Livestock Farming
* Agricultural Robotics
* Computer Vision
* RGB-D Imaging
* Deep Learning
* 3D Computer Vision
* Artificial Intelligence in Agriculture
