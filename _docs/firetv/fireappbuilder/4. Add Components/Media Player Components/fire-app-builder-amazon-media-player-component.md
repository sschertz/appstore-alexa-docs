---
title: Amazon Media Player Component
permalink: fire-app-builder-amazon-media-player-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

The Amazon Media Player is based on the open source [ExoPlayer](http://google.github.io/ExoPlayer/), which was developed by Google for Android. The Amazon Media Player builds on ExoPlayer to provide a robust and stable media player that is compatible on Fire TV devices.

The functionality in Amazon Media Player closely mirrors that in ExoPlayer. For example, Amazon Media Player supports SmoothStreaming, DASH, HLS, Dolby, captions, and more. For details about what Amazon Media Player supports, look at the documentation for ExoPlayer. The minimum Android SDK required is API level 19 (KitKat).

The aim with Amazon Media Player is to develop higher level player APIs that abstract away the complex ExoPlayer APIs for core playback scenarios. Amazon Media Player provides you with a mostly fixed set of APIs that you can use even when ExoPlayer changes its APIs with version upgrades.

## Configure Amazon Media Player Component

There aren't any options to customize the Amazon Media Player Component. You simply load the component following the general instructions here: [Load a Component in Your App][fire-app-builder-load-a-component].

If you have another media player component already loaded, you must remove it. See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.

{% include_relative componentnote_mediaplayer.html %}

{% include links.html %}
