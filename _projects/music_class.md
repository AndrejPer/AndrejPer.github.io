---
layout: page
title: Music classification with convolution
description:
img: assets/img/metal_audio.png
importance: 1
category: school
related_publications: true
---

This notebook explores application of ConvRBM {% cite lee2009convolutional %} to classification of music genra. The main idea is to represent the audio extract as a spectrogram and then solve it as a slef-supervised vision problem. You can check out the details in the notebook below.

<iframe 
src="{{ '/assets/html/convRBM.html' | relative_url }}" 
width="100%" 
height="600px" 
frameborder="0"
loading="lazy"></iframe>