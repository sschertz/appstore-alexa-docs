---
title: Media Players for Fire TV
permalink: fire-tv-media-players.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Any media player that uses the Android's media playback and encryption APIs (such as the `MediaCodec`, `MediaCrypto`, and `AudioTrack` classes) will work on the Amazon Fire TV platform. The following are several recommended players, divided into free versus paid options:

**Free Options**:

*  [Amazon port of ExoPlayer](../fire-tv-media-players.md#exoplayer)
*  [Android MediaPlayer](../fire-tv-media-players.md#androidmediaplayer)

**Paid Options**:

*  [VisualOn OnStream MediaPlayer](../fire-tv-media-players.md#visualon)
*  [NexStreaming NexPlayer SDK](../fire-tv-media-players.md#nexplayer)

For information about the audio and video formats supported by Amazon Fire TV, see [Fire TV Device Specifications][device-and-platform-specifications].

Also, note that you can always build your own custom media players using standard Android APIs available in Android L (which is supported by the current Fire TV).

* TOC
{:toc}

## Amazon Port of ExoPlayer {#exoplayer}

ExoPlayer is an open-source media player developed by Google and intended for Android media apps. To learn more about ExoPlayer, see the following resources:

*  [ExoPlayer homepage](https://developer.android.com/guide/topics/media/exoplayer.html)
*  [ExoPlayer Video from Google](https://www.youtube.com/watch?v=6VjF638VObA)
*  [ExoPlayer Developer Guide](http://google.github.io/ExoPlayer/guide.html)

Amazon has a port of ExoPlayer that is compatible with Fire TV. Instead of integrating the default ExoPlayer into your Fire TV app, use the Amazon port of ExoPlayer. The Amazon port of ExoPlayer provides many fixes, workarounds, and other patches to make ExoPlayer work on Amazon devices.

To understand how to use ExoPlayer, consult the standard ExoPlayer resources as listed previously.

<a href="https://github.com/amzn/exoplayer-amazon-port"><button class="feedbackButton">Download the Amazon Port of Exoplayer</button></a>

## Android MediaPlayer {#androidmediaplayer}

The standard [Android MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html) classes that handle audio and video playback are supported on Fire TV. These media classes can handle basic media playback in your app; however, for more robust media needs, the Amazon port of ExoPlayer (or one of the paid media player options) is recommended.

## VisualOn OnStream MediaPlayer {#visualon}

You can use the [VisualOn OnStream MediaPlayer](http://visualon.com/onstream-mediaplayer) to integrate encrypted media playback in your Fire TV app. VisualOn supports AES 128-bit encryption with HLS, PlayReady DRM with the DASH and SmoothStreaming protocol, and AES 608 and 708 closed captions.

Unlike with ExoPlayer, you will need to buy a license from VisualOn to use the VisualOn OnStream MediaPlayer in your Fire TV app.

## NexStreaming NexPlayer SDK {#nexplayer}

The [NexStreaming NexPlayer SDK](http://www.nexstreaming.com/index.php) is supported on Amazon Fire TV. Like VisualOn, this media player provides a robust set of media playback features, including DASH, DRM content, Smooth Streaming, HTTP Live Streaming (HLS), and more.

Similar with VisualOn, NexPlayer requires you to purchase a license to use their player in your Fire TV app.

{% include links.html %}
