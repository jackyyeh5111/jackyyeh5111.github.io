# Life Bus for Visually Impaired


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
    <a href="https://github.com/jackyyeh5111/blind_IOT" style="color: white"><i class="fab fa-github mr-1"></i> Github Repository </a>
</button>

{{< youtube YsfemT_JWsw >}}

<br>

This project was done in my first semester in CS Master’s program. I Collaborated with students from NTUST’s Design Department. Together, we developed a product that facilitates a smoother and easier way for the visually impaired to get on/off the bus. The details of this project is summarized in [booklet](https://drive.google.com/file/d/0BxLJbVR-7RLcZUM5Rk9LNU1hcVU/view?usp=sharing&resourcekey=0-8nXi6cXDZkx2uxEJRImb3g).

## Motivation
We noticed that Blind or VI people feel uneasy about the unknown, therefore needs tools to help inquire about the environment to ease their anxiety and fears.

<figure style="text-align: center">
  <img src="https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/1a95bd38-8668-42cb-af7f-3156e141a0fa" />
  <figcaption>Figure 1: The blind has to bring SOS signboard to request pedestrians for help</figcaption>
</figure>

## Solution
1. Signal transmitter will be installed in every bus stops.With RSSI (received signal strength indicator), cell phone can determine the distance of the bus from their location.The closer the bus approaches, the vibration frequency and size will increase. If using headphones, beeping alerts can also notify them.
2. Cell phone will automatically transmit the route that has set up in advance to the cloud.
3. The cloud will choose accordingly to the chosen bus number and send a signal to that indicated bus. The light on the bus driver’s dashboard will flicker, indicating that a visually impaired person is going to board on their bus.
4. Once arriving at their destination, their cell phone will vibrate as reminder. The bus driver will get off to assist the blind or visually impaired person off the bus.
5. Once the blind or VI passenger has successfully boarded on the bus, their cell phone will transmit a signal to the cloud notifying other bus drivers that the passenger has boarded on the bus.
6. As their destination approaches, the “request stop” button will light up to remind the bus driver.

## What I was Responsible for? 

Constructed system platform with AWS IoT SDK, allowing data publishing/receiving via MQTT protocol.

![image](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/5976aac3-912f-4d3d-a12c-529aab2b495a)

## My Fantastic Teammates

![image](https://github.com/jackyyeh5111/jackyyeh5111.github.io/assets/22386566/22a7484b-822f-495b-9cd0-dbe857eed733)
