---
title: Amazon Fling Frequently Asked Questions
sidebar: fling
product: Fling SDK
permalink: amazon-fling-frequently-asked-questions.html
reviewers: jeffersd
github: true
toc: false
---

What platforms are supported by the Amazon Fling SDK? 
:   The SDK is supported on Fire OS, Android, and iOS.

What type of media can my app send to Amazon Fire TV?
:   With the SDK, you can send anything your custom Amazon Fire TV receiver app can play back.  

How can I set the volume or mute the built-in media player receiver?
:   The built-in media player receiver does not support the mute and volume API calls at this time. A custom media player is required to use these APIs.  

How do the autoplay and playInBg options work when calling setMediaSource with the built-in media player receiver?
:   The built-in media player receiver does not support these options at this time. A custom media player is required to use these options.

How do I use the setPlayerStyle API to customize the style of the built-in media player receiver?
:   The built-in media player receiver does not support setting the player style at this time. A custom media player is required to use this API.

What does sendCommand do when sent to the built-in media player receiver?
:   The built-in media player receiver does not support the sendCommand API call. The intention of this API call is to allow custom receivers to receive information that can not be included in other API calls.  A custom media player is required to use this API.

Do you have resources or guidelines for design?
:   Yes, please visit [UX Guidelines][designing-amazon-fling-ux]. 

How can I get additional information?
:   If you have questions about the SDK please visit our [forum](https://forums.developer.amazon.com/spaces/66/index.html).

{% include links.html %}
