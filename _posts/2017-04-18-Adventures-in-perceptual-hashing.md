---
layout: post
---

### Background
One of the primary goals of my project at CUNY TV is to prototype a system of perceptual hashing that can be integrated into our archival workflows. Perceptual hashing is a method of identifying similar content using automated analysis; the goal being to eliminate the (often impossible) necessity of having a person look at every item one-by-one to make comparisons. Perceptual hashes function in a similar sense to standard checksums, except instead of comparing hashes to establish exact matches between files at the bit level, they establish similarity of content as would be perceived by a viewer or listener.

There are many examples of perceptual hashing in use that you might already have encountered. If you have ever used Shazam to identify a song, then you have used perceptual hashing! Likewise, if you have done a reverse Google image search, that was also facilitated through perceptual hashing. When you encounter a video on Youtube that has had its audio track muted due to copyright violation, it was probably detected via perceptual hashing.

One of the key differences between normal checksum comparisons and perceptual hashing is that perceptual hashes attempt to enable linking of original items and derivative or modified versions. For example, if you have a high quality FLAC file of a song and make a copy transcoded to MP3, you will not be able to generate matching checksums from the two files. Likewise, if you added a watermark to a digital video it would be impossible to compare the files using checksums. In both of these examples, even though the actual content is almost identical in a perceptive sense, as data it is completely different.

Perceptual hashing methods seek to accomodate for these types of changes by applying various transformations to the content before generating the actual hash, also known as a 'fingerprint'. For example, the audio fingerprinting library _Chromaprint_ converts all inputs to a sample rate of 11025 Hz before generating representations of the content in musical notes via frequency analysis which are used to create the final fingerprint. [^1] The fingerprint standard in MPEG-7 that I have been experimenting with for my project does something analogous, generating global fingerprints per frame with both original and square aspect ratios as well as several sub-fingerprints. [^2] This allows comparisons to be made that are resistant to differences caused by factors such as lossy codecs and cropping.

### Hands on Example
As of the version 3.3 release, the powerful Open Source tool FFmpeg has the ability to generate and compare MPEG-7 video fingerprints.  What this means, is that if you have the most recent version of FFmpeg, you are already capable of conducting perceptual hash comparisons!  If you don't have FFmpeg and are interested in installing it, there are excellent instructions for Apple, Linux and Windows users available at [Reto Kromer's Webpage](https://avpres.net/FFmpeg/#ch1)

For this example I used the following video from the University of Washington Libraries' [Internet Archive  page](https://archive.org/details/uwlibraries) as a source.

<iframe src="https://archive.org/embed/NarrowsclipH.264ForVideoPodcasting" width="320" height="240" frameborder="0" webkitallowfullscreen="true" mozallowfullscreen="true" allowfullscreen></iframe>


From this source, I created a GIF out of a small excerpt and uploaded it to Github.

![GIF](https://raw.githubusercontent.com/privatezero/Blog-Materials/master/Tacoma.gif)

Since FFmpeg supports URLs as inputs, it is possible to test out the filter without downloading the sample files! Just copy the following command into your terminal and try running it! (Sometimes this might time out. In that case just wait a bit and try again.)

__Sample Fingerprint Command__
{% highlight bash %}
ffmpeg -i https://ia601302.us.archive.org/25/items/NarrowsclipH.264ForVideoPodcasting/Narrowsclip-h.264.ogv  -i https://raw.githubusercontent.com/privatezero/Blog-Materials/master/Tacoma.gif -filter_complex signature=detectmode=full:nb_inputs=2 -f null -
{% endhighlight %}

__Generated Output__
{% highlight bash %}
[Parsed_signature_0 @ 0x7feef8c34bc0] matching of video 0 at 80.747414 and 1 at 0.000000, 48 frames matching
[Parsed_signature_0 @ 0x7feef8c34bc0] whole video matching
{% endhighlight %}

What these results show is that even though the GIF was of lower quality and different dimensions, the FFmpeg signature filter was able to detect that it matched content in the original video starting around 80 seconds in!

For some further breakdown of how this command works (and lots of other FFmpeg commands), see the example at [__ffmprovisr__](https://amiaopensource.github.io/ffmprovisr/#compare_video_fingerprints).

### Application at CUNY

At CUNY, we are interested in using perceptual hashing

[^1]: Lalinský, L. (2011, January 18). How Does Chromaprint Work? [https://oxygene.sk/2011/01/how-does-chromaprint-work/](https://oxygene.sk/2011/01/how-does-chromaprint-work/)
[^2]: [Chiariglione, L. (2014). Mpeg representation of digital media. Springer, 84-85.](http://www.worldcat.org/oclc/902763394)

