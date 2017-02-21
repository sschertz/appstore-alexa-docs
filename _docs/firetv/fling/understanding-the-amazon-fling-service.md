---
title: Understanding the Amazon Fling Service
sidebar: fling
product: Fling SDK
permalink: understanding-the-amazon-fling-service.html
reviewers: jeffersd
github: true
---

## Introduction to the Fling SDK

Amazon Fling is a second-screen service that enables your customers to send media and web content from Fire OS, iOS, and Android mobile devices to Amazon Fire TV devices for display and playback. This document contains an overview of this service and how you can integrate this functionality into your apps.

You can download the Fling SDK from the [SDK Downloads page](https://developer.amazon.com/public/resources/development-tools/sdk#Amazon%20Fire%20TV%20SDKs).

## What Is Amazon Fling?

Amazon Fling is a service on the Amazon Fire TV platform that allows customers to send media and web content to Fire TV from their mobile devices. We provide you with an API that your app can use to communicate with this service. You have to implement the API in your mobile app to create a **controller**, which enables that app to discover Fire TV devices on the network, and provides the mechanisms to play media content such as video, audio, or photos.

On Fire TV, adding support for this API to your Fire TV playback app enables you to handle remote playback events. A Fire TV app that supports this functionality is called a **player**.  

The controller service provides these features:

*  Set the source URL of the media to be rendered.
*  Get information about that media such as duration, current position, and metadata.
*  Discover Fire TV devices on the network that support this functionality.  
*  Control media playback (play, pause, stop, seek).
*  Get the playback status, immediately or with callbacks.
*  Send custom commands to the Fire TV receiver app for playback.

To create a controller app see the guide on [Flinging From Fire OS or Android][integrating-amazon-fling-into-your-android-app] or [Flinging From iOS][integrating-amazon-fling-into-your-ios-app].

To integrate remote install into your app see the guide on [Remote Install With Fire OS and Android][android-remote-install] or [Remote Install With iOS][ios-remote-install].  

When implementing a controller you should follow the [UX Guidelines][designing-amazon-fling-ux]. 

To integrate the controller with an existing Google Cast application see the guide on [Flinging From Cast Apps][integrating-amazon-fling-with-cast]. To integrate with an existing Android Media Router app see the guide on [Flinging From Media Router][using-amazon-fling-with-android-mediarouter].

To create a player app see the guide on [Creating a Player App][integrating-amazon-fling-into-your-fire-tv-app]. The service enables you to completely customize the playback user experience.

A [Built-In Media Receiver][working-with-built-in-receiver-on-fire-tv] is available for controllers that do not require a customized player.

Unity plugins are also available for integration. See [Unity Controller Plugin][unity-controller-plugin] and [Unity Player Plugin][unity-player-plugin] for more details.  

In our SDK, we provide a JAR file for Fire OS and Android, and a Framework for iOS.  For details on the APIs see the [Android API Docs][fling-android-api-docs] and the [iOS API Docs][fling-ios-api-docs].

The underlying framework of this feature enables apps to find and communicate with Amazon Fire TV devices to render media content sent by that app. With our SDK, you can focus on great user experience in your app while the underlying framework handles network technology, discovery, versioning, and so on.

## What Can I Do With the SDK?

With this SDK, you can do the following:

*  Discover Fire TV devices from your controller app on a mobile device.
*  Control playback of the content sent from your controller app.
*  Customize the user experience of your Fire TV player app.  

### Discover Fire TV from your Controller App  

This SDK enables your controller app on a mobile device (iOS/Android/Fire OS) to find Amazon Fire TV devices available on the same network. The controller asks the underlying framework to discover all the devices with a particular system ID (SID). If the SID is not specified, the SDK discovers all Fire TV devices and communicates with the built-in media receiver.

The SDK returns a list of Fire TVs. Each Fire TV has a friendly name (for example, "John's 2nd Fire TV"), which the app presents to the customer. The customer picks the right Fire TV to fling to by that name.

### Control Playback from your Controller App  

The SDK allows the controller application to send control commands to the discovered Fire TV. The application can then control the Fire TV as if it is calling methods on the local object returned from the Fire TV discovery process. The controller app can set the source of the content to be rendered on the player, as well as monitor and control the rendering of that content.

### Customize Playback for your Fire TV Player App 

To provide a custom user experience, you can develop or enhance a media player application for Fire TV and add support for this SDK. The service defined by your Fire TV player app will automatically be started when a controller begins communicating with it and your player app will communicate with controller apps through your implementation that uses our SDK.

## Next Steps

To get started working with this API and to start developing controller and player apps, see the appropriate document for your development platform:   

*  [Setting Up Your Amazon Fling Development Environment for Android][setting-up-your-amazon-fling-development-environment-for-android]
*  [Setting Up Your Amazon Fling Development Environment for iOS][setting-up-your-amazon-fling-development-environment-for-ios]

For any additional questions check the [FAQ][amazon-fling-frequently-asked-questions] or the [Forum](https://forums.developer.amazon.com/spaces/66/index.html).

{% include links.html %}
