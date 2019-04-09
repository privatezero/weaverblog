---
layout: post
title: 'WSU-Capture: a Lightweight FFV1 capture tool!'
---

Those who know me know that I am a big fan of the [FFV1/Matroska](https://www.loc.gov/preservation/digital/formats/fdd/fdd000343.shtml) combination for archival video reformatting and that I __love__ [vrecord](https://github.com/amiaopensource/vrecord) as a tool for capturing in this format. Here at WSU, we are unfortunately not able to use vrecord as it doesn't (currently) support the Windows machines and the Osprey capture card that are in use in our archives.

In looking for alternative software options that would A: let us capture in FFV1, and B: give us more granular control over attributes such as pixel aspect ratio, I realized that FFmpeg can directly use DirectShow compatible cards, such as our Osprey, as an [input device](https://ffmpeg.org/ffmpeg-devices.html#dshow). Since I am already somewhat familiar with using FFmpeg for capture, I decided that this would be a good opportunity to build out a lightweight tool that could enable archival quality FFV1 capture while also trying to simplify workflows!

### Ruby based GUI

<img src="https://github.com/WSU-CDSC/wsu-capture/raw/master/main-gui.png" alt="Main screen for WSU-Capture" width="500">

