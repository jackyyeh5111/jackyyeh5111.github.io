---
weight: 1
title: "Monocular Feature-Based Visual Odometry"
date: 2024-01-02T21:29:01+08:00
lastmod: 2024-01-02T21:29:01+08:00
draft: false
# authors: ["Dillon", "PCloud"]
description: "This is our final project for course CS543 in 2023 Fall"
# featuredImage: "featured-image.webp"

# tags: ["installation", "configuration"]
# categories: ["documentation"]
lightgallery: true
toc:
  auto: true
---

<style>
    figure {
      padding: 4px;
      text-align: center;
      margin: auto;
    }

    figcaption {
      background-color: black;
      color: white;
      font-style: italic;
      padding: 1px;
      text-align: center;
    }

    .github {
        margin-top: 20px;
        margin-bottom: 20px;
        background-color: black;
        padding: 6px;
        color: white;
        text-align: center;
        transition-duration: 0.4s;
    }
    .github:hover {
        background-color: #5c666f;
    }
</style>

<button class="github">
    <a href="https://github.com/jackyyeh5111/Explore-Visual-Odometry-Method-using-Monocular-Camera" style="color: white"><i class="fab fa-github mr-1"></i> Github Repository </a>
</button>

This is the final project for [UIUC CS543 course](http://luthuli.cs.uiuc.edu/~daf/courses/CV23/CV23.html). Inspired from this great [post](https://avisingh599.github.io/vision/monocular-vo/), my partner and I are determined to work on something related to visual odometry. Visual odometry is actually an another way to describe camera trajectory. The applications of which extend across various domains, serving as a foundational component for Visual Simultaneous Localization and Mapping (SLAM) systems.

**Figure 1** shows the typical pipeline for 2D-2D visual odometry. We conducted experiments at each stage to assess the significance of individual components except for the optimization(future work!).

<figure style="text-align: center">
  <img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/842a29b8-5c34-4b0b-acd3-fe6d2e12615d" width=500 />
  <figcaption>Figure 1: Visual odometry pipeline</figcaption>
</figure>

For those who are not familar with what visual odometry is or what 2D-2D means, please check my previous [article](https://hackmd.io/@jackyyeh/H12xz-bPT) for detailed introduction.

## Demo

<figure>
<img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/f0ebf1fc-1dca-4808-ada1-c94ee6ac4ef8" />
</figure>

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
  <img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/fc9362f9-7b74-406f-ad80-d4c837827117" width=500 />
  <figcaption>Figure 2: Feature matching</figcaption>
</figure>


2. **Feature Tracking** ([opencv tutorial](https://docs.opencv.org/3.4/db/d7f/tutorial_js_lucas_kanade.html))
Here we applied Lucas–Kanade method for optical flow tracking. This method operates under the assumption that the flow remains relatively constant in a local neighborhood around the pixel being considered. 
<figure style="text-align: center">
  <img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/1c376dcf-4a2e-4dc6-b3e4-eb8d1f17cea5" width=500 />
  <figcaption>Figure 3: Optical flow</figcaption>
</figure>

## Motion Estimation
Motion estimation determines the movement of a camera through a sequence of images. It can actually be divided into three sub-steps:

1. Estimate essential matrix ([detailed post](https://hackmd.io/@jackyyeh/Hkk6MrqIT))
Several techniques can be used for the computation of an essential matrix. Here we implemented Nister’s 5-point and 8-point algorithm with RANSAC.
2. Decompose $R, t$ from the essential matrix ([detailed post](https://hackmd.io/@jackyyeh/Hkk6MrqIT))
Decomposition can be done by the equations:

$$ E = [t_{\times}]R = U \Sigma V^{T} = [u_{1} \ u_{2} \ u_{3}] \Sigma V^{T} $$
$$ t_{\times} = u_{3} $$
$$ R = UYV^{T} $$

<figure style="text-align: center">
  <img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/ba143ad7-7bbb-4112-8015-882f0640ef73" width=300 />
  <figcaption>Figure 4: Camera pose transformation</figcaption>
</figure>

3. Recover the visual odometry
With relative transformation matrices $R, t$ between two frames, we can easily get the camera pose of new frame:
$$ R_{i+1} = RR_{i} $$
$$ t_{i+1} = \text{scale} \times Rt + t_{i} $$
where scale factor we will never know in the monocular scheme. In my implementation, I just use the ground truth that KITTI dataset provides.

## Dataset
Kitti sequence 00 ([link](https://www.cvlibs.net/datasets/kitti/eval_odometry.php))

## Future Work
- [ ] Integrate graph optimization
- [ ] Superpoint