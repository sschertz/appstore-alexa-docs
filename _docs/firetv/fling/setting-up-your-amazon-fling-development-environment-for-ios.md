---
title: Setting Up your Amazon Fling Development Environment for iOS
sidebar: fling
product: Fling SDK
permalink: setting-up-your-amazon-fling-development-environment-for-ios.html
reviewers: jeffersd
github: true
---

Our SDK for iOS is distributed as an iOS framework which can be linked to your iOS controller app.

This document describes how to set up your iOS projects to include these libraries.  

## Prerequisites

### System Requirements

*  MacOS X  
*  Xcode 6+

### Supported Configurations

*  iOS build architecture: armv7, arm64, i386, and x86_64  
*  Player discovery: Local network players only

### Download and Install

1.  Download the [Fling SDK](https://developer.amazon.com/public/resources/development-tools/sdk#Amazon%20Fire%20TV%20SDKs) from the Amazon developer portal.
2.  Extract contents.

## Setting up Xcode

To include the iOS framework in your iOS controller app project, choose **Project** > **Add to Project** and select the framework directory. Alternatively, you can drag and drop the AmazonFling.framework from the Finder to your app project.

When you add an existing framework to your project, Xcode asks you to associate it with one or more targets in your project. Once associated, Xcode automatically links the framework against the resulting executable.

You must also add the following system frameworks and libraries to the "Link Binaries" phase of the target in order to build a controller app:

*  Bolts.framework
*  CFNetwork.framework
*  Security.framework
*  SystemConfiguration.framework

{% include note.html content="Bolts.framework can be found in the /third_party_framework folder inside of the AmazonFling iOS SDK package." %}

{% include image.html file="firetv/fling/images/amazon_fling_frameworks" type="png" border="true" %}

In your project's settings, add the following to "Other Linker Flags": **-lc++**

{% include image.html file="firetv/fling/images/other_linkers_flags" type="png" border="true" %}

Include the required header files in your code using the #include directive. If you are working in Objective-C, you may use the #import directive instead of the #include directive. The two directives have the same results, but the #import directive guarantees that the same header file is never included more than once.

```
#import <AmazonFling/DiscoveryController.h>
#import <AmazonFling/MediaPlayerStatus.h>
#import <AmazonFling/RemoteMediaPlayer.h>
#import <Bolts/BFTask.h>
```

## Next Steps

To develop an iOS controller app, see [Integrating Amazon Fling into your iOS App][integrating-amazon-fling-into-your-ios-app].

{% include links.html %}
