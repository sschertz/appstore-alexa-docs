---
title: Setting Up Your Development Environment
permalink: setting-up-your-development-environment.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

To start developing Android-based Fire TV apps for the Amazon Fire TV platform, you need to first set up your development environment.

* TOC
{:toc}

## Set Up the JDK

You need the Java Development Kit (JDK) from Oracle to compile Java apps on your machine. 

First check to see if you already have the JDK. Open Terminal or Command Prompt and type `java -version`. If you have the JDK, the response should be something like this:

```
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
```

If you don't have the JDK, download the version of the JDK Installer for your machine from [Java SE Development Kit Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and run it. 

For more details, see the following: 

* [10 JDK 8 Installation for OS X](https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html)
* [JDK Installation for Microsoft Windows](https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html). Specifically, see "Running the JDK Installer" and "Updating the PATH Environment Variable."

For other operating systems and information, see [JDK 8 and JRE 8 Installation Start Here](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html).

## Set Up Android Studio {#setupandroidstudio}

Install [Android Studio](https://developer.android.com/studio/index.html), the official IDE for Android projects. See [Getting Started with Android Studio](https://developer.android.com/sdk/installing/studio.html) and [Install Android Studio](https://developer.android.com/sdk/installing/index.html) for information about setting up the Android Studio development environment on your machine.

## Next Steps

To connect your development computer to your Fire TV device with ADB, see [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device].

To install and run apps you develop on your Fire TV device, see [Installing and Running Your App][installing-and-running-your-app].

{% include links.html %}
