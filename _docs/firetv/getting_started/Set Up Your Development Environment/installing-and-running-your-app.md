---
title: Installing and Running Your App
permalink: installing-and-running-your-app.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

To test and debug your Fire TV app before submitting it to the Amazon Appstore, use Android Debug Bridge (ADB) to install and run your app on your Fire TV device. Installing your own app (outside of the Appstore) is sometimes referred to as "sideloading"
 an app. 

You must have already used ADB to connect your development computer to your Fire TV device. See [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device] for more information.

{% include note.html content="Certain development tools and resources referenced on this page are provided by third parties, not by Amazon. Any links to these tools and resources will take you to third-party sites." %}

* TOC
{:toc}

## Installing Your App (Command Line)

To install your app onto your Fire TV device from the command line, use the following command, where `<path-to-apk-file>` is the file system path to your app's APK:

```
adb install <path-to-apk-file>
```

If the installation was sucessful, ADB responds with the message similar to this one:

```
764 KB/s (217246 bytes in 0.277s)
pkg: /data/local/tmp/HelloWorld.apk
Success
```

To re-install an app that already exists on the device, you can use the `-r` option to reinstall the app:

```
adb install -r <path-to-apk-file>
```

Note that reinstalling an app does not replace any existing additional user data or cache. To clear this data, uninstall the old app before installing a new version, or clear the data by hand in **System > Applications**.

## Running Your App (Device)

Sideloaded apps appear in both the Recent row and in the My Library row in the Apps section. You can also find your app in the Settings menu:

1.  From Fire TV's main screen, select **Settings > Applications > Manage Installed Applications.**.
2.  Select your app.
3.  Select **Launch application**.

{% include note.html content="If you have a generation 1 device, some of the menus may be slightly different." %}

## Running Your App (Command Line)

To send a launch intent to your app on the Amazon Fire TV device, use the following command, where `com.amazon.sample.helloworld` is the package name of your app, and `MainActivity` is the name of your app's primary activity.

```
adb shell am start -n com.amazon.sample.helloworld/.MainActivity
```

ADB responds with a message similar to the following, and your app begins running:

```
Starting: Intent { cmp=com.amazon.sample.helloworld/.MainActivity }
```

## Uninstalling Your App (Device)

To uninstall your app from Amazon Fire TV on the device itself:

1.  From Fire TV's main screen, select **Settings > Applications > Manage Installed Applications**.
2.  Select your app.
3.  Select **Uninstall** > **Uninstall**.


## Uninstalling Your App (Command Line)

To uninstall your app from the command line, you need the package name for your APK. Use the following command to uninstall your app, where `com.amazon.sample.helloworld` is the package for your app:

```
adb uninstall com.amazon.sample.helloworld
```

If you are unsure of your app's package name, use the following command to see a list of all the installed APKs and 
their package names:

```
adb shell pm list packages -f
```

{% include links.html %}
