---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---


## GitHub Projects

### Predicting the predominant kind of tree (Kaggle)

In this [challenge](https://github.com/) , I am trying to predict the forest cover type (the predominant kind of tree cover) from strictly cartographic variables (as opposed to remotely sensed data) :

See GitHub page : <span style="color:blue">[https://github.com/AnthonyHoudaille/Kaggle-Forest-Type](https://github.com/AnthonyHoudaille/Kaggle-Forest-Type)</span>

### Estimating a position from a received signal strength for IoT sensors

Smart devices such as IoT sensors use low energy consuming networks such as the ones provided by Sigfox or Lora. But without using GPS networks, it becomes harder to estimate the position of the sensor. The aim of this study is to provide a geolocation estimation using Received Signal Strength Indicator in the context of IoT. The aim is to allow a geolocation of lowconsumption connected devices using the Sigfox network. State of the art modelsare able to be precise to the nearest kilometer in urban areas, and around tenkilometers in less populated areas.

See GitHub page: <span style="color:blue">[https://github.com//Received-Signal-Strengh-Position-Estimation](https://github.com//Received-Signal-Strengh-Position-Estimation)</span>

### Ship Detection (Deep Learning on satelite images)

See GitHub page: <span style="color:blue">[https://github.com/AnthonyHoudaille/FilRougeAIRBUS](https://github.com/AnthonyHoudaille/FilRougeAIRBUS) </span>

## Project Articles

{% include base_path %}

{% for post in site.projects reversed %}
  {% include archive-single.html %}
{% endfor %}
