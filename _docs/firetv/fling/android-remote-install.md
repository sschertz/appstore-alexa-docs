---
title: Integrating Remote Install into Your Android App
sidebar: fling
product: Fling SDK
permalink: android-remote-install.html
reviewers: jeffersd
github: true
---

## Introduction to Remote Installation

To enable this feature in your mobile app for Fire OS or Android, implement the API and add permissions to your Android manifest. This document describes both these steps. See [Understanding the Amazon Fling Service][understanding-the-amazon-fling-service] for a high-level overview of the features and functions our SDK provides. See also the controller sample app, part of the SDK, for details on your controller implementation.  

Before you start, make sure you've set up your development environment. See [Setting Up Your Amazon Fling Development Environment][setting-up-your-amazon-fling-development-environment-for-android] for Android for more details.

## Overview

There are four steps for creating or modifying your controller mobile app to support Remote Install:

1.  Modifying your Android manifest to include network permissions.  
2.  Initializing underlying framework and discovering available Fire TV devices.
3.  For a selected Fire TV device, add logic to check if your app is installed on it.
4.  Add UI flow to allow the user to initiate install of your Fire TV app. 

## Modifying the Android Manifest

The API requires some permissions to be added to your Android manifest (`AndroidManifest.xml`). Specifically, the API needs these permissions to communicate over networks:

*  `android.permission.INTERNET`: Allows applications to open network sockets.
*  `android.permission.ACCESS_WIFI_STATE`: Allows applications to access information about Wi-Fi networks.
*  `android.permission.ACCESS_NETWORK_STATE`: Allows applications to access information about networks.
*  `android.permission.CHANGE_WIFI_MULTICAST_STATE`: Allows applications to enter Wi-Fi Multicast mode. (Android (non-Fire OS) only)

In addition, Fire OS devices require a `uses-library` element to refer to the WhisperPlay shared library. Non-Fire OS Android devices should not include this tag.

### Fire OS Devices  

Add the `uses-permission` and `uses-library` elements as in the following example:  

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="[YOUR PACKAGE NAME]"
    android:versioncode="1"
    android:versionname="1.0">
    <!-- Android Network Access Permissions -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <application>
        ...
        <uses-library android:name="com.amazon.whisperplay.contracts" android:required="true" />
        ...
        </application>
    ...
</manifest>
```

### Non-Fire OS Android Devices  

Add the `uses-permission` elements as in the following example. Do not include the `uses-library` element for the WhisperPlay shared library.  

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="[YOUR PACKAGE NAME]"
    android:versioncode="1"
    android:versionname="1.0">
    <!-- Android Network Access Permissions -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE"/>
    ...
</manifest>
```

## Initializing Underlying Framework and Discovering Amazon Fire TV

Use the `InstallDiscoveryController` class to create a controller and to discover and connect to Amazon Fire TV over the network. Get an instance of `InstallDiscoveryController` in your `onCreate()` method:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate();
    ...
    mController = new InstallDiscoveryController(this);
}
```

The `IInstallDiscoveryListener` interface allows you to receive updates when an install service on an Amazon Fire TV device (`RemoteInstallService`) is discovered, updated, or no longer reachable by your app. Implement this interface to add behavior such as adding or removing a device to your list of discovered devices, or updating your user interface. The callback methods are called from worker thread and user interface elements shouldn't be updated inside of calling.

```java
private InstallDiscoveryController.IInstallDiscoveryListener mDiscovery = new InstallDiscoveryController.IInstallDiscoveryListener() {
    @Override
    public void installServiceDiscovered(RemoteInstallService remoteInstallService) {
        //add remote install service to the application’s remote install service list.
    }
    @Override
    public void installServiceLost(RemoteInstallService remoteInstallService) {
        //remove remote install service from the application’s remote install service list.
    }
    @Override
    public void discoveryFailure() {}
};
```

Call the `start()` method in your `onResume()` method, and the `stop()` method in onPause(). The `start()` method takes one argument: an object that implements `IInstallDiscoveryListener`.  

```java
@Override
protected void onResume() {
    super.onResume();
    …
   mController.start(mDiscovery);
}
@Override
protected void onPause() {
    super.onPause();
    …
   mController.stop();
}
```

## Installing Apps with RemoteInstallService

The `RemoteInstallService` interface represents an install service running on a remote Fire TV device. Once your app discovers Fire TV  devices running this service, your app gets an object that implements this interface. Use that `RemoteInstallService` object to install an app or check the version of an app on the Fire TV device.

When checking the version of an app if the app is not install the `RemoteInstallService.PACKAGE_NOT_FOUND` string will be returned. Once your app has called the `installASIN` method the Fire TV will launch the Appstore page associated with that ASIN (Amazon Standard Identification Number).

The user will then need to finish the installation with the a Fire TV remote. If the ASIN is invalid the Fire TV will show an error popup. The ASIN of an application can be found on its product information page on [amazon.com](http://www.amazon.com "Amazon") under the Product Details section. Some applications could have different products for different regions and therefore could have multiple ASINs.  

The `RemoteInstallService.AsyncFuture` interface allows your `getAsync()` method to add a completion listener (`RemoteInstallService.FutureListener`). The `AsyncFuture` interface implements `java.util.concurrent.Future`, which means you can also check to see if the computation is complete, to wait for its completion, and to retrieve the result of that computation.

```java
RemoteInstallService mSelectedFireTV; //It needs to be set with discovered instance by discovery.

private void checkInstalledPackageVersion() {
    mSelectedFireTV.getInstallPackageVersion("com.amazon.whisperplay.fling.media.player").getAsync(
                                             new RemoteInstallService.FutureListener<string>() {
        @Override
        public void futureIsNow(Future<String> future) {
            try {
                String version = future.get();
                if (version.equals(RemoteInstallService.PACKAGE_NOT_FOUND)) {
                    Log.i ("LOG", "package not installed");
                } else {
                    Log.i ("LOG", "version = " + version);
                }
            } catch (InterruptedException e) {
                Log.e("LOG", "InterruptedException", e);
            } catch (ExecutionException e) {
                Log.e("LOG", "ExecutionException", e);
            }
        }
    });
}

private void installPackage() {
    mSelectedFireTV.installASIN("B0128SSCMO").getAsync(new RemoteInstallService.FutureListener<void>() {
        @Override
        public void futureIsNow(Future<Void> future) {
            try {
                future.get();
            } catch (InterruptedException e) {
                Log.e("LOG", "InterruptedException", e);
            } catch (ExecutionException e) {
                Log.e("LOG", "ExecutionException", e);
            }
        }
    });
}
```

{% include links.html %}
