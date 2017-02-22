---
title: Remote Install
sidebar: fling
product: Fling SDK
permalink: remote-install.html
reviewers: jeffersd
github: true
toc: false
---

Remote Install allows you to discover Fire TV devices on the same network. Once a Fire TV is discovered from a user’s mobile device or tablet, you can check if your application is installed on that Fire TV and prompt the user to also install the Fire TV version of your app or game.

{% include image.html file="firetv/fling/images/remoteinstalllanding" type="png" border="true" %}

Remote installation is important for developers looking to increase the user base of their cross platform apps. By leveraging Remote Discovery’s ability to run on iOS and Android devices, you gain a new way to drive user acquisition for your Fire TV products. Conveniently, you can simply add Remote Install to your existing app or game without the need of fully implementing the rest of the Amazon Fling SDK’s APIs. Let’s take a look at what you need to know for getting it up and running.

* TOC
{:toc}

## Implementing Remote Install

There are only a few steps for creating or modifying your controller mobile app to support Remote Install:

1.  (Android only) Modifying your Android manifest to include network permissions.
2.  Initializing underlying framework and discovering available players on Fire TV devices.
3.  For a selected Fire TV device, add logic to check if your app is installed on it.
4.  Add UI flow to allow the user to initiate install of your Fire TV app.

Once your app discovers Fire TV devices running the Remote Install service, your app gets an object that implements the RemoteInstallService interface. You can then use the RemoteInstallService object to install an app or check the version of an app on the Fire TV device.

You can check the version of your app using the `getInstalledPackageVersion` API. Once your app has called the installByProductId method, the Fire TV will launch the Appstore page associated with the provided ASIN (Amazon Standard Identification Number). The user will then need to finish the installation with a Fire TV remote. The need to use the remote to complete the purchase provides the user control of the process. If the ASIN is invalid the Fire TV will show an error popup.

## iOS

The following documents will guide you through the process for iOS apps:

*  [Integrating Remote Install into Your iOS App][ios-remote-install]

## Android

The following documents will guide you through the process for Android apps:

*  [Integrating Remote Install into Your Android App][android-remote-install]

{% include links.html %}
