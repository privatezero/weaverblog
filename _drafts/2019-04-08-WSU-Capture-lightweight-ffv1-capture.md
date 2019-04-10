---
layout: post
title: 'WSU-Capture: a Lightweight FFV1 capture tool!'
---

Those who know me know that I am a big fan of the [FFV1/Matroska](https://www.loc.gov/preservation/digital/formats/fdd/fdd000343.shtml) combination for archival video reformatting and that I __love__ [vrecord](https://github.com/amiaopensource/vrecord) as a tool for capturing in this format. Here at WSU, we are unfortunately not able to use vrecord, as it doesn't (currently) support the Windows machines and the Osprey capture card that are in use in our archives.

In looking for alternative software options that would A: let us capture in FFV1, and B: give us more granular control over attributes such as pixel aspect ratio, I realized that FFmpeg can directly use DirectShow compatible cards, such as our Osprey, as an [input device](https://ffmpeg.org/ffmpeg-devices.html#dshow). Since I am already somewhat familiar with using FFmpeg for capture, I decided that this would be a good opportunity to build out a lightweight tool that could enable archival quality FFV1 capture while also trying to simplify workflows!

### Ruby based GUI

<img src="https://github.com/WSU-CDSC/wsu-capture/raw/master/main-gui.png" alt="Main screen for WSU-Capture" width="500">

While I found great success controlling our card directly via FFmpeg, I felt that it would be a step back in terms of usability to try to force our archivists/students into an exclusively command line based environment. Fortunately, I stumbled across the Ruby gem [Flammarion](https://github.com/zach-capalbo/flammarion) which, with its stated purpose of working with "scripts where you just want to show some information or buttons without going through too much trouble" fit the bill perfectly. Flammarion combined easy set-up (only depending on an installation of Chrome Browser) with decent aesthetics and I found it very intuitive to use.

### FFV1/Matroska

Since I was building this for our specific context (exclusively NTSC content, etc) I hard coded a lot of the values for capture settings. I used vrecord as a cheat sheet for this, copying configuration options such as its [FFV1](https://github.com/amiaopensource/vrecord/blob/master/vrecord#L820) and [NTSC](https://github.com/amiaopensource/vrecord/blob/master/vrecord#L880) specs. This will enable us to create files that are in-line with the community of vrecord using institutions, despite using a different tool.

### Derivative creation

One of the comments I had from our University Archivist was that for a tool to be optimal, it should be as close to 'one click' as possible. Accordingly, I decided to integrate the creation of access files in sequence with the capture process. While it is possible to have FFmpeg create both a master and an access file simultaneously, out of an abundance of caution I set WSU-Capture up to generate a web-optimized H.264/MP4 immediately upon completion of capture. Since this tool was designed to always be used with interlaced sources, I set it to de-interlace access copies automatically, with the option to change that to inverse-telecine for any film content that we capture via our Tobin telecine.
