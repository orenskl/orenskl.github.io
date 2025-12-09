---
layout: post
title: "Audio stutter / gaps with GStreamer"
subtitle: "Fixing audio issues with FLAC playback"
---

For some time I have noticed that for some tracks or albums I am playing from [TIDAL](https://www.tidal.com) I get audio issues - specifically 
I hear gaps or stutter. This sometimes strangely happens only on the first track of an album or randomly in another track. I started digging
this and first I looked at the [Mopidy](https://mopidy.com) logs. I noticed these messages :

```
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=0%
DEBUG    [MainThread] mopidy.audio.gst Got BUFFERING bus message: percent=100%
```

These messages are events that are triggered by the [GStreamer](https://gstreamer.freedesktop.org) pipeline to indicate that its source buffers are empty (or full), if they are empty
[Mopidy](https://mopidy.com) will pause the playback and therefore you hear these gaps or stutter in the track.

I suspected there is something going on with the source buffers in [GStreamer](https://gstreamer.freedesktop.org), after some digging and searching the web I found this
[issue](https://github.com/hzeller/gmrender-resurrect/issues/182) that mentions the same behavior but did not point me to any good solution.

Why would the source buffers drain out ? I have a 1Gbps internet connection so the source shouldn't be slow or lacking behind even if playing a 16 bit / 44Khz FLAC files.
I started playing around with the buffers parameters (buffer size and duration) but none of this helped.

The [GStreamer](https://gstreamer.freedesktop.org) version I was using was **1.22.0** so I decided to try to upgrade it to the latest stable version but this also did not 
improve the situation.

Eventually I tried downgrading [GStreamer](https://gstreamer.freedesktop.org) to **1.20.7** and to my surprise it solved the issue !!!

So this is it for this time.
