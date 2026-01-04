---
# 27 march 2024
layout: page
title: Abalone Age
description: Predicting the age of abalones based on physical measurements
img: assets/img/abalone.jpg
importance: 2
category: fun
related_publications: false
---

Here, I tried playing with a simple dataset about abalone shells. It consists of several physical measurement. The goal is to predict the age of the shell given the measurements. Age is determined with the number of rings the shell has, but sounting the number of rings is tidious. I will try making a linear model to predict the number of rings.

<iframe
src="{{ '/assets/html/abalone.html' | relative_url}}" 
width="100%" 
height="600px" 
frameborder="0">
</iframe>
