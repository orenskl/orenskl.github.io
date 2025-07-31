---
layout: post
title: "AirPlay support in HiFiStreamer"
subtitle: "Making Mopidy and Shairport-Sync live together"
---

It has been a long time since my last post here, more than two years. A lot has happend since than and I hope to post
more here on **HiFiStreamer** and streaming digital audio in general. For this post I would like to talk about 
[Mopidy](https://mopidy.com) and **AirPlay** and how I made them work together.

Most of my music listening is done via [TIDAL](https://www.tidal.com), but as you may know [TIDAL](https://www.tidal.com)
does not have all the music material you may want - sometimes I find myself listening to [Apple Music](https://www.apple.com/apple-music/) 
and finding there staff that does not exist in [TIDAL](https://www.tidal.com). I had to find a way to stream music from **Apple**
devices to **HiFiStreamer**.

[Shairport-Sync](https://github.com/mikebrady/shairport-sync/tree/master) allows you to do exactly that it is an open source 
**AirPlay** implementation. The only issue I has with integrating [Shairport-Sync](https://github.com/mikebrady/shairport-sync/tree/master)
into **HiFiStreamer** was that it shares the same **ALSA** audio device with [Mopidy](https://mopidy.com) and they both can play
music at the same time. I wanted a system that will play only from a single source no matter which one I choose or want to
listen to. For example if I am now listening to an album from [TIDAL](https://www.tidal.com) and I want to switch to an album
from [Apple Music](https://www.apple.com/apple-music/) I want the switch to be flawless.

Here is how I did this:

1. Both [Shairport-Sync](https://github.com/mikebrady/shairport-sync/tree/master) and [Mopidy](https://mopidy.com) uses **ALSA** as
   their output device so I had to change my **ALSA** configuration to use a direct mix plugin (**dmix**). For this ofcousre I made sure
   that the dmix plugin does not mess with the audio data and is **bitperfet**.
2. Synchronize [Shairport-Sync](https://github.com/mikebrady/shairport-sync/tree/master) and [Mopidy](https://mopidy.com) so that
   when one of them start to play the other one stops. For this I wrote a special daemon - [shairmopd](https://github.com/orenskl/shairmopd) - that monitors both players and multiplexes them.

You can find the latest version in the [Download](/downloads) page - enjoy !!!
