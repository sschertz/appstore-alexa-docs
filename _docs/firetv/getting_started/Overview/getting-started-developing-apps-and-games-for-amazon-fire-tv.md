---
title: Getting Started Developing Apps for Amazon Fire TV
permalink: getting-started-developing-apps-and-games-for-amazon-fire-tv.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

To get started building apps for Fire TV, first decide whether you want to build an Android app or web app:

*  **Android App**: If you're an Android Java developer, you can use existing tools (like Android Studio) and frameworks (including Unity) to develop apps and games for the 10-foot experience. Sample code, documentation, and guidelines are available to help you make the most of your apps. If you're building a streaming media app, you can use [Fire App Builder][fire-app-builder-overview] &mdash; a Java-based Android starter kit &mdash; to get up and running quickly.
*  **Web App**: If you're an HTML5 web developer, you can leverage the Amazon WebView to develop apps and games. You have the option to build [HTML5 web apps](https://developer.amazon.com/public/solutions/platforms/webapps), [Cordova apps](https://developer.amazon.com/public/solutions/platforms/cross-platform) using the Fire OS port, or [hybrid apps](https://developer.amazon.com/public/solutions/platforms/android-fireos/docs/building-and-testing-your-hybrid-app). If you're building a streaming media app, you can use the [Web App Starter Kit for Fire TV][the-web-app-starter-kit-for-fire-tv] to get up and running quickly.

{% include tip.html content="For an in-depth comparison between Web App Starter Kit for Fire TV (WASK) and Fire App Builder, see [Fire TV Development Framework Comparison][fire-tv-development-framework-comparison]." %}

Additionally, consider your skill set. Are you Java-based Android developer, or an HTML5/web developer? Choose an approach that aligns with your expertise and app requirements.

* TOC
{:toc}

## Android App Development for Fire TV

For Java-based Android developers, Fire TV uses the same tools, IDEs, and APIs you're already used to for Android development. To get started, see [Fire App Builder][fire-app-builder-overview], which is a starter kit for building Java-based Amazon Fire TV and Android apps. Fire App Builder is designed for streaming media TV apps (not games).

If you're building your own app from scratch, see these topics:

* [Setting Up Your Development Environment][setting-up-your-development-environment]: If you're new to Android development, this page helps you get started.
* [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device]: Connect your development computer to a Fire TV device over ADB, either via the network or a USB cable.
* [Installing and Running Your App][installing-and-running-your-app]: Install, run, and uninstall your app on a Fire TV device for testing before you submit it to the Appstore.

If you're an experienced Android developer, also check out this list of what's [different about developing on Fire OS][amazon-fire-tv-differences-from-android-tv-development].

## HTML5 Web App Development for Fire TV

If you're an HTML5 web app developer building a streaming media app, you can use the [Web App Starter Kit for Fire TV][the-web-app-starter-kit-for-fire-tv] (WASK). WASK is an open source project designed to get you up to speed quickly with a simple media-oriented app for Fire TV. This starter kit includes an example user interface designed for the 10-foot experience, support for the Fire TV remote control, and sample components that you can use to create and customize your own media app.

If you're building your HTML5 web app from scratch, see [Getting Started with Web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv].

## APIs for Your Fire TV App

When you build your Fire TV app, you can also implement other Amazon APIs to provide a more robust experience:

*  [In-App Purchasing API](https://developer.amazon.com/public/apis/earn/in-app-purchasing): When customers buy Fire TV devices and register with their Amazon accounts, they are already set up with their Amazon payment profiles and are ready to purchase apps or in-app items with no further effort. Amazon Fire TV and Fire TV Stick support the Amazon In-App Purchasing API, so you can offer consumable items, permanently entitled items, and even subscriptions for sale in your app.
*  [Amazon Fling SDK](/apis/experience/fling/docs/understanding-the-amazon-fling-service): The Amazon Fling SDK allows customers to fling or cast the screen that appears on their phone or tablet directly onto their television. Extending apps to two screens allows more than one person to engage with your app.

To see more Fire TV APIs and SDKs, see [Apps & Games Services SDKs](/resources/development-tools/sdk).

## Device and Media Specifications

If you're looking for information about the media, device, and specifications Fire TV supports, such as video formats, DRM, codecs, resolution rates, and more, see [Fire TV Device Specifications][device-and-platform-specifications].

## About the Fire TV Documentation

The Fire TV documentation is organized into the following groups:

*  [Getting Started][getting-started-developing-apps-and-games-for-amazon-fire-tv]
*  [Fire App Builder][fire-app-builder-overview]
*  [Catalog Integration][integrating-your-catalog-with-fire-tv]
*  [Fling SDK][understanding-the-amazon-fling-service]

## Fire TV Forums

If you need help, have a question, or want to share other feedback, use the [Fire TV and Fire TV Stick categories](https://forums.developer.amazon.com/spaces/43/Fire+TV+and+Fire+TV+Stick.html) on the Amazon Developer Forum.

{% include links.html %}
