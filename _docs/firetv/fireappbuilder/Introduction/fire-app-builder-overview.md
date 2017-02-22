---
title: "Fire App Builder: A Starter Kit for Java-based Amazon Fire TV and Android Apps"
permalink: fire-app-builder-overview.html
sidebar: fireappbuilder
product: Fire App Builder
github: true
toc: false
---

Fire App Builder provides a Java-based framework that you can use to easily and quickly build streaming media Android apps for Amazon Fire TV. 

Fire App Builder lets you build an engaging, high-quality media experience on Fire TV following best practices and techniques &mdash; without having to develop all the code yourself. Fire App Builder's code is Java-based and uses Android Studio, Gradle, and other tools common to Android app development. 

Fire App Builder is released as an open source project on Github ([github.com/amzn/fire-app-builder](https://github.com/amzn/fire-app-builder)) under the Apache 2.0 license.

* TOC
{:toc}

## How You Work with Fire App Builder

When you create an app with Fire App Builder, you configure the settings for your data feed, screen layouts, and functionality through a series of JSON files. You also construct query syntax to get the categories and contents from your media feed.

For authentication, ads, analytics, or in-app purchasing, you can use a variety of pluggable components that implement interfaces. To customize the look and feel of your app, including font, colors, logos, layouts, and other details, you simply update some values in XML or JSON files (rather than coding directly in Java). 

Overall, Fire App Builder allows you to quickly develop a high quality app without doing Java programming. If you want to extend Fire App Builder with more advanced functionality, you can use Fire App Builder as the underlying framework and build on top of it, since most of Fire App Builder's components are modular. 

## Sample App from Fire App Builder

Fire App Builder contains a sample app (called "Application") with a home screen that looks as follows:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="Sample app with Fire App Builder" caption="The home screen of the sample app built with Fire App Builder." %}

The sample app in Fire App Builder contains a generic video feed from Lightcast that is used for testing purposes only.

## Who Fire App Builder Is For

Fire App Builder is designed for companies with streaming media assets (similar to Netflix or Hulu) who want to make their content available online through Fire TV and other Android TV platforms. You would be a good fit if you have a video feed where your media assets (movies, shows, or other video content) are published. 

The media feed can be JSON or XML, but it must be its own feed rather than a Youtube or Vimeo channel. (If it's XML, it can be a media RSS feed, such as what you submit to iTunes.) The feed can be in any structure &mdash; you'll use query syntax to select the categories and contents from your feed. 

Additionally, Fire App Builder requires you to configure files using Android Studio, so it's geared toward developer types who prefer to create their app using Java-based Android (instead of HTML5 web technologies). You can also build on top of the Fire App Builder framework to create more sophisticated apps. Essentially Fire App Builder is the Fire TV SDK for Android Java developers.
 
If you're more of a content creator instead of a coder, or you just want to build a Fire TV app for your Youtube videos, or if you're not comfortable working in Android Studio with code (even though no programming skills are required), consider using the [Web App Starter Kit for Fire TV](the-web-app-starter-kit-for-fire-tv) instead.

## Requirements to Work with Fire App Builder {#requirements}

To develop with Fire App Builder, you will need the following:

* **[Android Studio](http://developer.android.com/sdk/index.html)**. See [Getting Started with Android Studio](https://developer.android.com/sdk/installing/studio.html) and [Install Android Studio](https://developer.android.com/sdk/installing/index.html) from the Android documentation for information about setting up the Android Studio development environment on your machine.
* **[Java Development Kit (JDK) 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)**. You must have the [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) or later on your computer.
* **[Fire TV](https://www.amazon.com/dp/B00U3FPN4U) or [Fire TV Stick](https://www.amazon.com/Amazon-Fire-TV-Stick-Streaming-Media-Player/dp/B00GDQ0RMG)**. You will need to test your app on an actual Fire TV device &mdash; either the Fire TV or Fire TV Stick. (Although emulators are possible, they don't always work and aren't supported for Fire TV development.) The Fire TV has better performance, so if your media is resource intensive, you'll want to be sure it plays well on the Fire TV Stick too. (Note that Fire TV does not come with an HDMI cable, so you will need to supply that cable to connect the Fire TV box to your TV.)
* **Television with HDMI port**. You will need a television with an HDMI port that your Fire TV can connect to.
* **A-to-A USB cable**. If you're using Fire TV (not the stick) and connecting your computer to Fire TV through USB (instead of through a wireless network), you will need an A-to-A USB cable. Instead of using a cable, you can also connect to Fire TV devices through a network. (See [Connecting to Fire TV Through ADB][fire-app-builder-connecting-adb-to-fire-tv].)
* **Media feed with necessary elements.** You will need a media feed (in either JSON or XML format) with video assets as well as the following feed elements: title, ID, description, URL, card image, background image. (The same image can be used for both the card and background.) Any video format supported by [Exoplayer](https://google.github.io/ExoPlayer/supported-formats.html) is compatible with Fire App Builder.

## Fire App Builder Features

Fire App Builder provides the following features:

* **Five screens**: Splash screen, Home (two layouts), Content Details, Content Renderer, and Search.
* **Search functionality and search results**: Text search within your app. Also includes intent filters to integrate with the global Fire TV search if your media is integrated into the Fire TV catalog.
* **Exoplayer-based Amazon Media Player for streaming media**: The media player includes closed captions, HTTP Live Streaming (HLS), bandwidth settings, and more.
* **Components for ads, analytics, authorization, and in-app purchasing**: More than 10 components that you can easily plug into your app and configure through XML files. Some of these components include Amazon in-app purchases, Login with Amazon, Facebook Login, Omniture Analytics, Flurry Analytics, Adobe Pass Authentication, Freewheel ads, and VAST 2.0 ads.

## Fire TV Terminology and Devices

"[Fire TV](https://www.amazon.com/dp/B00U3FPN4U)" refers to the Fire TV box, while "[Fire TV Stick](https://www.amazon.com/dp/B00ZV9RDKK)" refers to the stick version. 

You can compare the specifications across devices at [Fire TV Device Specifications][device-and-platform-specifications]. The Fire TV has more power and faster performance, but the two devices run the same Fire OS and are more or less identical from an end-user's perspective. Each device has two generations &mdash; Generation 2 is the latest. 

Fire TV also optionally comes in a [Gaming Edition](https://www.amazon.com/Amazon-Fire-TV-Gaming-Edition-Streaming-Media-Player/dp/B00XNQECFM). You can also buy a [game controller](https://www.amazon.com/Amazon-Fire-Game-Controller-Alexa/dp/B00NO8LX7E) on its own and use it with Fire TV stick. Fire TV Stick (Generation 2) includes the voice remote by default, whereas Fire TV Stick (Generation 1) optionally provided the voice remote.

## Getting Started

To get started building an app with Fire App Builder, see [Beginning-to-End Process for Building an App with Fire App Builder][fire-app-builder-end-to-end-process], which lists the steps needed to develop an app with Fire App Builder.

As you follow the instructions in the documentation, note that it's usually assumed that you're working in the **Android view** in Android Studio. If you don't see a certain folder or path, check the view you're in.

Also, you can find any file by clicking **Shift** twice and then typing the file name. When you load the file, the path to the file appears just below the row of buttons on the top navigation bar.

## Getting Updates to the Project

You can get updates to the application by watching the [Fire App Builder Github repository](https://github.com/amzn/fire-app-builder) and running a `git pull` command to get the latest updates. See [
Pull Updates from Github][fire-app-builder-pulling-updates-from-github] for more details.

## Getting Support

If you have feedback or questions about Fire App Builder, you can get support through the [Fire TV Amazon forums](https://forums.developer.amazon.com/spaces/166/index.html).

## Noting Bugs or Feature Requests

To note bugs or make feature requests, you can do so through the Issues tab on the [Fire App Builder Github repository](https://github.com/amzn/fire-app-builder). Or you can submit the info to the [Fire TV Amazon forums](https://forums.developer.amazon.com/spaces/166/index.html).


{% include links.html %}
