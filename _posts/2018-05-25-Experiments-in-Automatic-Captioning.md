---
layout: post
---

Recently I have been investigating tools to help with automating at least part of the transcription process for the audiovisual materials my institution makes available online. As we have hundreds (thousands?) of hours worth of A/V content already digitally available via our various platforms, and are actively producing more, using automation for a portion of the transcription process makes both logical and fiscal sense for us.

The tool I have been most actively experimenting with is the [IBM Watson speech to text service](https://www.ibm.com/watson/services/speech-to-text/). I should note that this post is simply a reflection of my thoughts on this process so far and is not intended as an explicit recommendation or endorsement of Watson over any of the other similar tools. This testing involved signing up for a non-paid level account on IBM Watson.

To generate video subtitles using Watson I have been using two scripts. [One, written in bash](https://github.com/WSU-CDSC/microservices/blob/master/vid2watson.sh) that handles the actual piping of the file to Watson by converting an input to 16 kHz mono, uploading to Watson and then storing the raw Watson output in a `.json` file. It also creates a rough dump of the raw transcript into text. [The second, written in Ruby](https://github.com/WSU-CDSC/microservices/blob/master/watson2vtt.rb) takes the raw Watson output and parses it into four second segments formatted as a `.vtt` subtitle file. This file can then be used directly with its corresponding video to provide a rough subtitle track.

I made the decision to parse the text into four second segments with the idea of minimizing the amount of intervention required by someone proofreading the `.vtt` files. By standardizing the text chunks it eliminates the need for any manipulation of text timing (which also makes it easier to do editing in a basic text editor rather than specialized software). Hopefully having the editor only focus on textual content will save time and money (as well as the sanity of the proofreaders).

So far the results, while far from perfect, have been honestly better than I expected them to be! Quality of the transcript is very dependent on the style of speech employed by speakers in the video, and as the speech model employed seems to be tuned towards a very particular kind of American English, it struggles with speakers who speak in a more 'casual' style (such frequently interjecting 'ya know') where it will mis-assign and make up words. Proper nouns and non-English words are also mis-assigned frequently. A word that Watson often struggles with is 'Archivist', which is unfortunate given the context. This being said, I have been surprised that for several of the videos I have tested there have been broad segments (again, dependent on speaker) that created extremely usable subtitles even with no proofreading.

>00:00:12.690 --> 00:00:22.560

>Just in it's a national need
for responsible and thoughtful

>00:00:22.560 --> 00:00:25.800

>curation of Indian artifacts.

>00:00:25.800 --> 00:00:29.700

>And this program is just
over and above setting us up

>00:00:29.700 --> 00:00:31.440

>for success and those
kinds of things.

>00:00:31.440 --> 00:00:37.330

>00:00:37.330 --> 00:00:46.230

>You know it brought us into
this space that was safe

>00:00:46.230 --> 00:00:48.270

>and it brought us
into this space

>00:00:48.270 --> 00:00:55.480

>where we may come from different
tribal communities, indigenous,

---

> 00:00:12.64 --> 00:00:17.21
> 
Justin is the national need for responsible 

>00:00:16.89 --> 00:00:18.8

>and (PAUSE) 

>00:00:19.43 --> 00:00:21.2
thoughtful 

>00:00:22.54 --> 00:00:26.81
curation of Indian artifacts and this program

>00:00:26.81 --> 00:00:30.84
is just over and above setting us up for success in those

>00:00:30.84 --> 00:00:31.49
kinds of things

>00:00:37.25 --> 00:00:39.3
you know it 

>00:00:39.36 --> 00:00:43.18
brought us into the space stop 

>00:00:42.93 --> 00:00:44.55
(PAUSE) 

>00:00:44.57 --> 00:00:46.44
was safe 

>00:00:46.17 --> 00:00:49.78
and it brought us into the space where 

>00:00:49.68 --> 00:00:51.33
(PAUSE) 

>00:00:51.64 --> 00:00:56.0
we may come from different tribal communities indigenous 
