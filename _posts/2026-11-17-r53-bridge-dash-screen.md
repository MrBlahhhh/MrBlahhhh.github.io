---
title: "A dash screen for the R53 bridge, and an SD log that never stops"
date: 2026-11-17 12:00:00 -0500
categories: car tech
tags: [mini, r53, esp32, esp32-s3, k-line, can-bus, datalogger, sd-card, lvgl, waveshare, dash-screen]
cover: /assets/images/r53-dash-screen/cover.jpg
lightbox: true
excerpt: "The R53 K-line and CAN bridge now drives a touchscreen off its USB-C: live gauges, a 20-second graph, the SD log with New log and Mark buttons, and DME read presets. Underneath, it logs every CAN frame and every K-line exchange to the SD card whenever a card is in, and clears its oldest logs when the card fills."
article_header:
  type: overlay
  theme: dark
  background_color: "#0d0f13"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .7))"
    src: /assets/images/r53-dash-screen/cover.jpg
---

<!--more-->

{% include r53-dash-feature.html %}
