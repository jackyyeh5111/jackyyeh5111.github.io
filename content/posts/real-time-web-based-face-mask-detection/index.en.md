---
weight: 1
title: "Real-time Web-based Face Mask Detection"
date: 2024-01-02T21:29:01+08:00
lastmod: 2024-01-02T21:29:01+08:00
draft: false
# authors: ["Dillon", "PCloud"]
description: ""
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

    .demo {
        margin-top: 20px;
        margin-bottom: 20px;
        background-color: red;
        padding: 6px;
        color: white;
        text-align: center;
        transition-duration: 0.4s;
    }
    .demo:hover {
        background-color: #EC7063;
    }
</style>

<button class="github">
    <a href="https://github.com/jackyyeh5111/Mask-Detection-YOLO" style="color: white"><i class="fab fa-github mr-1"></i> Github Repository </a>
</button>

{{< youtube UkhFKQNMUnY >}}

<br>
This project is built with OpenCV, YOLOv3 and some Computer Vision concepts in order to detect face masks in real-time video streams.

![image](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/f17f5afd-142d-4120-a70f-f1d1d2b710cf)

## Key Features
1. Train YOLO with custom dataset.
2. Enabling face mask detection in real-time video streams using Streamlit.

## Motivation
In the midst of the persistent COVID-19 pandemic, the need for effective face mask detection applications has surged. However, the scarcity of substantial datasets containing images depicting individuals wearing masks has presented a significant obstacle, intensifying the complexity and difficulty of this undertaking.

## Dataset
Kaggle link: [link](https://www.kaggle.com/datasets/andrewmvd/face-mask-detection)

This dataset contains 853 images belonging to the 3 classes, as well as their bounding boxes in the PASCAL VOC format.

The classes include:
1. With mask
2. Without mask
3. Mask worn incorrectly

## Ref
- [Real-Time Video Streams With Streamlit-WebRTC](https://betterprogramming.pub/real-time-video-streams-with-streamlit-webrtc-bd38d15f2ef3)
- [Developing Web-Based Real-Time Video/Audio Processing Apps Quickly with Streamlit](https://towardsdatascience.com/developing-web-based-real-time-video-audio-processing-apps-quickly-with-streamlit-7c7bcd0bc5a8)
