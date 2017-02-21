---
title: Designing Amazon Fling UX
sidebar: fling
product: Fling SDK
permalink: designing-amazon-fling-ux.html
reviewers: jeffersd
github: true
---

Amazon Fling allows you to send media sources from your mobile application to the Amazon Fire TV. Your app on the mobile device is the **controller**, while the app that plays your content on the Fire TV is the **player**.  

In this document, we will describe the UX design guidelines and recommendations for your **controller** applications.

## User Interface Design Guidelines

The goals of these guidelines are to provide a consistent user experience across all applications and platforms using Amazon Fling and improve discoverability of the operation.  

Controller app:

*  The Amazon Fling button should be easily discoverable and accessible to the user. 
*  The Amazon Fling button should reflect the state of discovery of and connection to the Amazon Fire TVs - these states are as follows:
    *  No Fire TVs available in the network
    *  Fire TVs discovered
    *  Connected to a Fire TV
*  Interface should show the playback state and basic remote controls. The recommended playback control icons _should_ be used to render the control buttons
*  Please refer to the [recommended icon archive](https://s3-us-west-1.amazonaws.com/amazon-fling/fling_icon_package.zip).  

## Controller App Operation

For your controller app to work, the mobile device and the Amazon Fire TV must be on the same Wi-Fi network. The mobile application is responsible to show whether there are Fire TVs available on the network. If there are Fire TVs available on the network, your application allows the user to pick the desired Fire TV from the list of available Fire TVs. Once the user selects the Fire TV to play the media on, your app retrieves the current state of that Fire TV and presents it to the user. At this point, the user is able to start playing back on this Fire TV and control the playback on the Fire TV.

The screenshots below are from the iOS FlingSample application in our SDK, and are as close to the Android FlingSample app as possible.

### Amazon Fire TV Discovery State Visualization

<table class="grid">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr>
<th>No Fire TV Discovered</th>
<th>At Least 1 Fire TV Discovered</th>
<th>Connected to a Fire TV</th>
</tr>
</thead>
<tbody>
<tr>
<td>{% include inline_image.html file="firetv/fling/docs/images/no_ftv_found" type="png" %}</td>
<td>{% include inline_image.html file="firetv/fling/docs/images/ftv_found_but_not_connected" type="png" %}</td>
<td>{% include inline_image.html file="firetv/fling/images/connected_to_ftv" type="png" %}</td>
</tr>
</tbody>
</table>

The table above shows how the Amazon Fling button should be rendered for different states of the Amazon Fire TV discovery process. The table below references icons that are available in the [recommended icon archive](https://s3-us-west-1.amazonaws.com/amazon-fling/fling_icon_package.zip).

<table class="grid">
<colgroup>
<col width="70%" />
<col width="30%" />
</colgroup>
<thead>
<tr>
<th>Discovery State</th>
<th>Icon Representation</th>
</tr>
</thead>
<tbody>
<tr>
<td>No Fire TVs discovered</td>
<td>No icon</td>
</tr>
<tr>
<td>At least one Fire TV discovered </td>
<td>{% include inline_image.html file="firetv/fling/images/ic_whisperplay_default_light_24dp" type="png" %}</td>
</tr>
<tr>
<td>Connected to the Fire TV </td>
<td>{% include inline_image.html file="firetv/fling/images/ic_whisperplay_default_blue_light_24dp" type="png" %}</td>
</tr>
</tbody>
</table>

### Primary Use Cases

This functionality supports the following primary use cases:

*  Connect to Amazon Fire TV and then start playback
*  Continue playback on the Fire TV
*  Control playback on the Fire TV

#### Connect to Amazon Fire TV and Start Playback

In this use case, the controller first connects to the Fire TV and then sends the media URL to the Fire TV to render.

{% include image.html file="firetv/fling/images/connect_then_play" type="png" %}

#### Continue Local Playback on Amazon Fire TV

In this sequence, the controller streams the media and plays it locally. During the playback, the user picks a Fire TV to send the media URL to. The controller should play on one device only (e.g. show the video thumbnail on the mobile device) and indicate that remote playback is in progress by showing the connected Amazon Fling button.

{% include image.html file="firetv/fling/images/play_then_connect" type="png" %}

#### Control Playback on the Amazon Fire TV

Your controller app may follow the shown UI guidelines for visualizing playback control. The icons are available in the [recommended icon archive](https://s3-us-west-1.amazonaws.com/amazon-fling/fling_icon_package.zip).

{% include image.html file="firetv/fling/images/control_playback" type="png" %}

<sub>Big Buck Bunny is copyright 2008, Blender Foundation / www.bigbuckbunny.org and is licensed under the [Creative Commons Attribution License 3.0](https://creativecommons.org/licenses/by/3.0/us/legalcode). Elephants Dream is copyright 2006, Blender Foundation/ Netherlands Media Art Institute/www.elephantsdream.org and is licensed under the [Creative Commons Attribution License 2.5](https://creativecommons.org/licenses/by/2.5/legalcode).</sub>

{% include links.html %}
