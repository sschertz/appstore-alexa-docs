---
title: Working with the Built-in Receiver on the Fire TV Platform
sidebar: fling
product: Fling SDK
permalink: working-with-built-in-receiver-on-fire-tv.html
reviewers: jeffersd
github: true
---

The Amazon Fire TV and Fire TV Stick have a built-in receiver application that supports Amazon Fling service. This means your controller app can send supported media content over to the Fire TV and Fire TV Stick without the need for a specific companion Fire TV application.

Your controller application can discover the Fire TV or Fire TV Stick using the publically available unique service identifier (SID) of the built-in receiver and send media content to it.

This page describes how you can achieve that and what you can do with this built-in receiver.  

## Discovering the Built-in Receiver

The built-in receivers on the Fire TV and Fire TV Stick are identified by the service identifier (SID) `"amzn.thin.pl"`.

To discover these receivers, you can use the following code snippets to search:

### On Android/Fire OS

```java
mController = new DiscoveryController(this);
mController.start("amzn.thin.pl", mDiscoveryListener);
```

### On iOS

```java
mController = [DiscoveryController alloc];
[mController open:@"amzn.thin.pl" : self];
```

## Playing Media Content on the Built-in Receiver

The built-in receiver on the Fire TV and Fire TV Stick provides a rich set of features to your controller app. 

You can do the following:

*  Send video clips, audio clips, as well as images
*  If your app provides closed captioning for video content, closed captioning may be displayed on the Amazon Fire TV  
*  Show title and description of the media clips  
*  Show album art when playing audio
*  Control how the end of playback is handled, in case you want to send media clips continuously

{% include note.html content="The built-in media player receiver does not support the mute and volume API calls at this time." %}

### Supported Media Types  

The built-in receiver on the Amazon Fire TV and Fire TV Stick can play any media type supported by the Fire TV platform. The detailed list of content types is available here: [Fire TV Device Specifications][device-and-platform-specifications].  

### How to Control Playback?

The `setMediaSource` API allows you to control how the playback works on the built-in receiver. The `metadataJson` parameter of this API is responsible for setting values for UI elements on the playback interface.

### Format of the Metadata JSON

In your controller application, please send the metadata JSON string that conforms to the following format:  

```json
{
    "type" : string, // required (if not present, video is assumed)
    "title" : string, // optional
    "description" : string, // optional
    "poster" : string, // optional – URL of the album art for an audio media source
    "tracks" : // optional – subtitles presented to the user
        [ // Array of subtitle objects
            {
                "src" : string, // required – URL of the WebVTT file
                "kind" : string, // required – always "subtitles"
                "srclang" : string, // required – language code
                "label" : string // required – what is shown on the UI
            }, …
        ],
    "noreplay": true|false, //optional
}
```

{% include links.html %}
