---
weight: 1
title: "Lane Following using Pure Pursuit Controller on F1TENTH Car"
date: 2024-01-02T21:29:01+08:00
lastmod: 2024-01-02T21:29:01+08:00
draft: false
# authors: ["Dillon", "PCloud"]
description: "This is our final project for course ECE484-Principles-Of-Safe-Autonomy in 2023 Fall"
# featuredImage: "featured-image.webp"

tags: ["installation", "configuration"]
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
    <a href="https://github.com/jackyyeh5111/ECE484-Principles-Of-Safe-Autonomy-2023Fall-Final/tree/main" style="color: white"><i class="fab fa-github mr-1"></i> Github Repository </a>
</button>

Welcome! This is our final project for course ECE484-Principles-Of-Safe-Autonomy in 2023 Fall. The course page can be found [here](https://publish.illinois.edu/robotics-autonomy-resources/f1tenth/).

The project implements a vision-based lane following system. Our aim to make vehicle follow the lane accurately and quickly without collision using Pure Pursuit Controller given RGB images. Our vehicle platform is build on [F1TENTH](https://f1tenth.org/). 

Please check out our [final presentation video](https://www.youtube.com/watch?v=mselI6W_V-o) for brief summary for the proejct.

## Overview

The vehicle is able to follow the lane accurately without collision:
<figure style="border-style: none">
<img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/7e11faf1-e84e-420c-ac01-fbc8b3902dc0">
</figure>

## Method
<figure>
          <img width="936" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/2c44461c-4c9d-469f-abe3-95bb0d005945">
      <figcaption>Figure 1: method diagram</figcaption>
    </figure>

The project built vision-based lane following system from scratch. Lane detector identifies the lane from the captured frame and provides candidates of imaginary waypoint for the controller. Next, the controller selects the best waypoint based on the vehicle state, and sends out next control signal.

The whole system is integrated with ROS, consisting of three primary components:

1. Camera calibration
2. Lane detection
3. Controller

### Component 1 - Camera calibration

#### Inches to pixels
<figure style="text-align: center">
    <img width="789" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/6e64550e-54cd-4182-8091-342d93c39023">
      <figcaption>Figure 2: inches to pixels</figcaption>
    </figure>

Because the target yellow lane must stick on the ground, we can simply measure the relationship between inches and pixels by applying the projection matrix $P_{measure}$ to a planar board lying on the ground. 

#### View-widen trick
As **Figure 2** shows, pixels of white board in `camera view` take up the entire area in `measured bird-eye view`. That is to say, the other pixels in `camera view` is invisible in `measured bird-eye view`.

An intuitive question pops up:
What if different project matrix $P_{detection}$ is used in lane detection, e.g.$P_{detection} \neq P_{measure}$? Can we still derive the real-world inches of detected waypoints? (We will walk through lane detection later)

Yes, we can solve the problem through a combination of linear transformation as **Figure 3** shows.

<figure style="text-align: center">
<img width="918" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/6534d82f-d742-4396-8d2c-d7fa27d85c3b">
      <figcaption>Figure 3: derive real-wrold coordniate of target point</figcaption>
    </figure>

### Component 2 - Lane detection
demo:
<figure style="border: none">
<img width="200" alt="image" src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExdXltaXNscHg5d2tvemNubWNmZTVzZzJ4MWp2cnUwY242a3NqZG1iYyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/QL9o5rpbySvFbv40mc/giphy.gif">
</figure>

Lane detection steps:
<figure>
<img width="874" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/7c732480-739b-4586-b533-9fe6b5d174a0">
<figcaption>Figure 4: lane detection pipeline</figcaption>
</figure>
    
1. Perspective tranform
Apply projection matrix $P_{detection}$ metioned in section "view-widen trick".
2. Thresholding
    - **Hue thresholding**: lane's hue should lie between hue_min and hue_max.
    <figure style="border: none">
    <img width="300" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/5461155b-fe1c-44dc-8f9e-9f585f0d317f">
    </figure>
    - **Value thresholding**: the blue channel value should exceed a specified threshold compared to the red channel value. Our choice of utilizing multiple channels instead of a grayscale value results from experiments.
    - **Saturation thresholding**: we found that saturation channel can well separate the histogram distribution between background and lane across most of the frames. All we have to do is find a dividing point somewhere between as **Figure 5** shows. We use cumulative distribution function(CDF) to achieve this goal. Dividing point is defined as the bin with minimum pixel number between certain CDF ratio range as **Figure 6** shows.

    <figure style="text-align: center">
    <img width="440" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/0f0c6bca-bd64-4155-a9f1-ac625c97713a">
      <figcaption>Figure 5: saturation histogram.</figcaption>
    </figure>
    
    <figure style="text-align: center">
          <img width="800" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/b7e0b5a2-0389-437d-9909-9db198850318">
      <figcaption>Figure 6: saturation thresholding with CDF. Note that a heuristic is introduced is that the bin with minimum pixel number will appear between CDF 0.6 to 0.9.</figcaption>
    </figure>
3. Lane fit
Vertically adjust the window box progressively based on detected lane pixels. The initial center of the box, denoted as $w_{t}$, relies on the slope calculated from the centers of the previous two windows $w_{t-1}$ and $w_{t-2}$. Then we refine the center of each box by computing the mean of the x positions of the lane pixels included within the box. The process ends when $w_{t}$ reaches the image boundary or the number of detected pixels drops below a certain threshold.

<figure style="border: none">
         <img width="406" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/9020b5f5-2937-4687-9fc3-403a5108cc24">
    </figure>

### Component 3 - Controller
#### Longitudinal controller
We apply the dynamic speed model. Velocity decreases as the [curvature](https://en.wikipedia.org/wiki/Curvature) $\kappa$ increases.
$$
\kappa = \frac{\vert x'y'' - y'x'' \vert}{(x'^{2} + y'^{2})^{3/2}}
$$

Mapping formula:
```python!
vel = vel_max - curvature * (vel_max - vel_min) / (curv_max - curv_min)
```

#### Lateral controller (Pure Pursuit controller)
<img width="822" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/e0f55977-56f3-4974-9221-6ad24f3aba12">

In the [pure pursuit method](https://thomasfermi.github.io/Algorithms-for-Automated-Driving/Control/PurePursuit.html), we first determine a target point from multiple waypoint candidates on the desired path. This target point is determined with respect to a predetermined look-ahead distance $l_{d}$ from the vehicle. Then we compute the steering angle $\delta$ to move toward the target point according to the kinematic bicycle model.
 
We can get steering angle $\delta$ as below:
$$
\delta = \arctan \left(\frac{2 L \sin(\alpha)}{l_d}\right)
$$
where $l_{d}$ denotes look ahead distance, $\alpha$ denotes relative yaw error, and $L$ is the wheel base distance.

## Collision avoidance
In this project, two criteria are employed to determine if an obstacle exists ahead.
1. **Close last waypoint**: the first criteria is that the distance of last waypoint has to be close enough, because the lane is blocked by an obstacle in the middle. 
2. **Not reach boundary**: this criteria complements the first one. Sometimes the distance of last waypoint is close enough because the vehicle is turning instead of being blocked as case 1 in the following graph.

<img width="885" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/7960e9b7-7250-4608-b671-2952d8be8e18">

You may notice that this naive collision avoidance algorithm works only when the lane detection method is robust. In future work, we intend to improve the algorithm by integrating image object detection or lidar sensors.

## Troubleshooting
- Long response latency from the F1 car may result from a low battery issue.
- F1 car proceeds unsmoothly in a rumble if multiple control signals are sent within a single frame, even if the control signal remains unchanged.

## Simulation
Please refer to the [link](https://github.com/jackyyeh5111/ECE484-Principles-Of-Safe-Autonomy-2023Fall-Final/tree/main/controller_simulation) for simulation.

<img width="885" alt="image" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/105990e7-43ca-422c-96f3-22af7c10cd99">

## TODO
- [ ] ML method for lane detection such as [lanenet](https://github.com/MaybeShewill-CV/lanenet-lane-detection).
- [ ] Integrate lidar to collision avoidance algorithm
- [ ] Implement MPC