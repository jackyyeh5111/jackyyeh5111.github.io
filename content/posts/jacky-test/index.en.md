---
weight: 1
title: "Jacky tests"
date: 2020-03-03T21:29:01+08:00
lastmod: 2020-03-06T21:29:01+08:00
draft: false
# authors: ["Dillon", "PCloud"]
description: "Jacky tests"
# featuredImage: "featured-image.webp"

tags: ["installation", "configuration"]
# categories: ["documentation"]
lightgallery: true
toc:
  auto: true
---

# Monocular Feature-Based Visual Odometry using Python OpenCV

<style>
    figure {
      border: 1px #cccccc solid;
      padding: 4px;
      margin: auto;
    }

    figcaption {
      background-color: black;
      color: white;
      font-style: italic;
      padding: 1px;
      text-align: center;
    }
</style>

Here is [github repo](https://github.com/jackyyeh5111/Explore-Visual-Odometry-Method-using-Monocular-Camera) for the project. 

Inspired from this great [post](https://avisingh599.github.io/vision/monocular-vo/), my partner and I are determined to work on a project about visual odometry. Visual odometry is actually an another way to describe camera trajectory. The applications of which extend across various domains, serving as a foundational component for Visual Simultaneous Localization and Mapping (SLAM) systems.

**Figure 1** shows the typical pipeline for 2D-2D visual odometry. We conducted experiments at each stage to assess the significance of individual components except for the optimization(future work!).

<figure style="text-align: center">
  <img src="https://hackmd.io/_uploads/B11mhBXO6.png" width=500 />
  <figcaption>Figure 1: Visual odometry pipeline</figcaption>
</figure>

For those who are not familar with what visual odometry is or what 2D-2D means, please check my previous [article](https://hackmd.io/@jackyyeh/H12xz-bPT) for detailed introduction.

## Demo
==TODO==

## Problem Definition

### Input
A monocular camera captures a continuous stream of grayscale images. The frames captured at time $t$ and $t+1$ can be denoted as $I_{t}$ and $I_{t+1}$ respectively. Note that we have prior knowledge for all intrinsic parameters of the camera.

### Output
For every pair of frames, find the rotation matrix $\textbf{R}$ and the translation vector $\textbf{t}$, which represents the camera motion between the two frames. Note that the vector $\textbf{t}$ can only be computed [up to a scale factor](https://stackoverflow.com/questions/17114880/up-to-a-scale-factor) in the monocular scheme.

## Approach Outline
1. Receive image sequence $I_{t}, I_{t+1}$.
2. Feature detection
3. Feature matching or Feature tracking
    - Feature matching: detect features in $I_{t}, I_{t+1}$ and match them.
    - Feature tracking: detect features in $I_{t}$ and track those features to $I_{t+1}$ . A new detection is triggered if the number of features drop below a certain threshold. 
4. Compute the camera pose by motion estimation.
    
## Feature Detection
The feature detection component in monocular visual odometry plays a crucial role in identifying distinctive points across subsequent frames. In this project, we explored various feature detectors including SIFT, ORB, FAST, and BRIEF. Each of these detectors has its own strengths and weaknesses. Experiment results are placed in the result section.

## Feature Matching / Tracking
Both feature tracking and feature matching aims to **find correspondences** of feature point sets across subsequent frames. With enough corresponding feature pairs, we are able to compute the essential matrix described in next section. Both is imeplemented in this project:

1. **Feature Matching** ([opencv tutorial](https://docs.opencv.org/3.4/dc/dc3/tutorial_py_matcher.html))
Here we tested two matchers: Brute-Force and FLANN (Fast Library for Approximate Nearest Neighbors) implemented in OpenCV. Brute-Force matcher is straightforward. It takes the descriptor of one feature in the first set and matches it with all other features in the entire second set using a distance calculation. On the other hand, FLANN is designed for fast nearest neighbor search in large datasets with high-dimensional features.
<figure style="text-align: center">
  <img src="https://hackmd.io/_uploads/ByyTdb6DT.png" width=500 />
  <figcaption>Figure 2: Feature matching</figcaption>
</figure>


2. **Feature Tracking** ([opencv tutorial](https://docs.opencv.org/3.4/db/d7f/tutorial_js_lucas_kanade.html))
Here we applied Lucas–Kanade method for optical flow tracking. This method operates under the assumption that the flow remains relatively constant in a local neighborhood around the pixel being considered. 
<img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/325c143d-902b-486f-89ac-c6b621fd44fa"/>
<!-- <figure style="text-align: center">
  <figcaption>Figure 3: Optical flow</figcaption>
</figure> -->

## Motion Estimation
Motion estimation determines the movement of a camera through a sequence of images. It can actually be divided into three sub-steps:

1. Estimate essential matrix ([detailed post](https://hackmd.io/@jackyyeh/Hkk6MrqIT))
Several techniques can be used for the computation of an essential matrix. Here we implemented Nister’s 5-point and 8-point algorithm with RANSAC.
2. Decompose $R, t$ from the essential matrix ([detailed post](https://hackmd.io/@jackyyeh/Hkk6MrqIT))
Decomposition can be done by the equations:
$$
\begin{align}
    E &= [t_{\times}]R = U \Sigma V^{T} = [u_{1} \ u_{2} \ u_{3}] \Sigma V^{T} \\
    \\
    t_{\times} &= u_{3} \\
    R &= UYV^{T}
\end{align}
$$
<figure style="text-align: center">
  <img src="https://hackmd.io/_uploads/rys0pcJD6.png" width=300 />
  <figcaption>Figure 4: Camera pose transformation</figcaption>
</figure>

3. Recover the visual odometry
With relative transformation matrices $R, t$ between two frames, we can easily get the camera pose of new frame:
$$
\begin{align}
    R_{i+1} &= RR_{i} \\
    t_{i+1} &= \text{scale} \times Rt + t_{i}
\end{align}
$$
where scale factor we will never know in the monocular scheme. In my implementation, I just use the ground truth that KITTI dataset provides.

## Heuristics
scale

## Results

### Dataset
Kitti

## Future Work
1. Integrate graph optimization
2. Superpoint