---
weight: 1
title: "Data Analyst @ Quadas (July 2017 - August 2017)"
date: 2024-01-02T21:29:01+08:00
lastmod: 2024-01-02T21:29:01+08:00
draft: true
# authors: ["Dillon", "PCloud"]
description: "My first job"
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

Quadas is a SaaS cloud advertising technology company that offers mobile programmatic cloud solutions for customers. They build transparent mobile DSP and data management platforms to improve the customer's data management abilities and provide programmatic advertising technology for digital marketing companies.

## Responsibility
1. Utilized PySpark to conduct distributed analysis on terabytes of user behavior data on AWS S3. <br><br>
Due to large amount of data, I decided to use PySpark to speed up the process of data I/O. In this step, I would get raw data of user behavior. Then I would do some feature engineering to convert these raw data into valuable features for each user.

  ```
  raw data:
    1. Device ID: DSP chose device ID
    2. Site ID: website the user visited
    3. …
  user features:
    1. avg_reqs_per_day
    2. avg_reqs_per_site
    3. ...
  ```

2. Improved online advertising effectiveness by 10% through detecting web traffic fraud with an unsupervised auto-encoder based approach. <br><br>
The concept is quite intuitive. I pre-trained an auto-encoder to have the ability to encode “real” user data into a subspace and decode it back. However, this decoding capability doesn't extend to fraudulent ones. Moreover, this approach allows us to use unsupervised learning and we usually have plenty of real user behavior data. Data labeling is usually expensive, hard, and in some cases unavailable.
<figure>
  <img width="70%" src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/fef1b83e-e7e1-45f1-b094-7a9fc3789cc2">
</figure>
  