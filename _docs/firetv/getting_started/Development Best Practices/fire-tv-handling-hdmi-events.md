---
title: Handling HDMI Events
permalink: fire-tv-handling-hdmi-events.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

When users connect or disconnect an HDMI cable, your app must handle these HDMI events following the guidelines here.

* TOC
{:toc}

## HDMI Disconnection

The recommended behavior of an HDMI disconnection &mdash; because the TV is powered OFF, input to the TV switched to another HDMI port, or the HDMI cable disconnected physically &mdash; is as follows:

*  Apps that play video *must* pause playback when the TV is turned off or HDMI cable is disconnected. When the TV is powered ON or HDMI cable connected, playback should remain paused until the user explicitly presses PLAY button on the remote.
*  Apps that play only audio *may* continue to play even if the HDMI is disconnected, provided the Fire TV is connected to another audio device via digital optical cable (Fire TV &ndash; 1st Generation only), Bluetooth A2DP headset, or audio headset via the Game controller.
 
## Letting the App Know about HDMI Events

To let your app know about connection or disconnection, listen for [`ACTION_HDMI_AUDIO_PLUG`][1] intent broadcast that has an extra ([`EXTRA_AUDIO_PLUG_STATE`][2]) indicating if the HDMI is in connected state or not.

## Other Connection Events

If a digital optical cable (Fire TV - 1st Generation only), audio headset, or BT A2DP headset is connected to Fire TV, unfortunately there is no reliable way of knowing this in Android Lollipop based Fire OS 5 or later. 

As such, it is recommended that you provide a User Preference setting in the app that controls this behavior.
 
 
[1]: https://developer.android.com/intl/reference/android/media/AudioManager.html#ACTION_HDMI_AUDIO_PLUG
[2]: https://developer.android.com/reference/android/media/AudioManager.html#EXTRA_AUDIO_PLUG_STATE