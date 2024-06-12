# Energy Bank for the Elderly


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

<!-- <button class="github">
    <a href="https://github.com/jackyyeh5111/blind_IOT" style="color: white"><i class="fab fa-github mr-1"></i> Github Repository </a>
</button> -->

<br>

{{< youtube yIyVsVz74r8 >}}

<br>

## Introduction

This is the project we have done for [Bo Le Associates AI x CSR Competition](https://www.pwc.tw/zh/news/press-release/press-20180716.html). We noticed that the imbalance of retiring life has been a huge problem for the elderly, which may cause plummeting in physical, mental and cognitive functions. With the help of technology, we aim to co-create a satisfied senior society, making seniors no longer a passive group. Our solution is summarized in this [booklet](https://drive.google.com/file/d/1UfZjd3WD3IKXpix6KBbtza2nYNJeELlt/view).

🎖️ We got 2nd place in the final round, standing out from 100+ teams.

## Platform

Considering that **Line** is the most widely used communication platform for the elderly in Taiwan, we have designed the entire functionality of our system around Line. This approach ensures that the elderly can effortlessly accomplish various tasks within a unified platform. Users of the platform can be split into groups of service providers and demanders.

For providers, they provide services on the platform such as skills imparting, life experience sharing, and career consulting. For demanders, they can select the services they desire.

- Demonstration of the process how service providers use the plarform:
![ezgif-5-78a1389b92](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/1f27a9f5-5ff0-4ad8-8742-711d0e3ac32f)

- Demonstration of the process how service demanders use the plarform:
![ezgif-5-c188c7bd0e](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/3d91a74d-ca13-4556-b563-93deda782940)

## What I was Responsible for

- Implemented a chatbot for the elderly on the LINE platform using the LINE message API and Dialogflow.
- Built and Deployed the system on server Heroku.
- Set up demands and supplies via voice user interface. Automatically transfer voice into texts with punctuations.
- Prices suggesstion using logistic regression model.
- Implemented service recommendation system for providers by comparing their profile and potential services.

## My Best Teammates
![image](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/72b9caaa-2c45-4a83-a00d-3c6b254964da)

