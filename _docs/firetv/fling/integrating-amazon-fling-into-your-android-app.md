---
title: Integrating Amazon Fling into Your Android App
sidebar: fling
product: Fling SDK
permalink: integrating-amazon-fling-into-your-android-app.html
reviewers: jeffersd
github: true
---

To enable this feature in your mobile app for Fire OS or Android, implement the API and add permissions to your Android manifest. This document describes both these steps. See [Understanding the Amazon Fling Service][understanding-the-amazon-fling-service] for a high-level overview of the features and functions our SDK provides. See also the controller sample app, part of the SDK, for details on your controller implementation.  

Before you start, make sure you've set up your development environment. See [Setting Up Your Amazon Fling Development Environment for Android][setting-up-your-amazon-fling-development-environment-for-android] for more details.

## Overview

There are four steps for creating or modifying your controller mobile app to support our SDK:

1.  Modifying your Android manifest to include network permissions.  
2.  Initializing underlying framework and discovering available players on Fire TV devices.
3.  Flinging content and controlling that content.
4.  Monitoring the status of that content.  

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
manifest xmlns:android="http://schemas.android.com/apk/res/android"
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
manifest xmlns:android="http://schemas.android.com/apk/res/android"
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

Use the `DiscoveryController` class to create a controller and to discover and connect to Amazon Fire TV over the network. Get an instance of `DiscoveryController` in your `onCreate()` method:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate();
    setContentView(R.layout.activity_main);
    ...
    mController = new DiscoveryController(getApplicationContext());
}
```

The `IDiscoveryListener` interface allows you to receive updates when a player on an Amazon Fire TV device (`RemoteMediaPlayer`) is discovered, updated, or no longer reachable by your app. Implement this interface to add behavior such as adding or removing a device to your list of discovered devices, or updating your user interface. The callback methods are called from worker thread and user interface elements shouldn't be updated inside of calling.

```java
private DiscoveryController.IDiscoveryListener mDiscovery = new DiscoveryController.IDiscoveryListener() {
    @Override
    public void playerDiscovered(RemoteMediaPlayer player) {
        //add media player to the application’s player list.
    }
    @Override
    public void playerLost(RemoteMediaPlayer player) {
        //remove media player from the application’s player list.
    }
    @Override
    public void discoveryFailure() {}
};
```

Call the `start()` method in your `onResume()` method, and the `stop()` method in onPause(). The `start()` method takes two arguments: a player service identifier or SID (defined by your custom player app on Fire TV), and an object that implements `IDiscoveryListener`. The service identifier (SID) must match the target player. For more information on creating the player app on Fire TV, see [Integrating Amazon Fling into your Fire TV Application][integrating-amazon-fling-into-your-fire-tv-app].

```java
@Override
protected void onResume() {
    super.onResume();
    …
   mController.start("YourReceiverServiceID", mDiscovery);
}
@Override
protected void onPause() {
    super.onPause();
    …
   mController.stop();
}
```

## Flinging Content with RemoteMediaPlayer

The `RemoteMediaPlayer` interface represents a player running on a remote Fire TV device. Once your controller app discovers target devices for flinging, your controller app gets an object that implements this interface. Use that `RemoteMediaPlayer` object to fling media from your app to the player application on the target device.

The `RemoteMediaPlayer.AsyncFuture` interface allows your `getAsync()` method to add a completion listener (`RemoteMediaPlayer.FutureListener`). The `AsyncFuture` interface implements `java.util.concurrent.Future`, which means you can also check to see if the computation is complete, to wait for its completion, and to retrieve the result of that computation.

```java
private StatusListener mListener = new Monitor();
private static final long MONITOR_INTERVAL = 1000L;

private void fling(final RemoteMediaPlayer target, final String name, final String title) {
    mCurrentDevice = target;
    mCurrentDevice.addStatusListener(mListener).getAsync(new ErrorResultHandler("Cannot set status listener"));
    mCurrentDevice.setPositionUpdateInterval(MONITOR_INTERVAL)
            .getAsync(new ErrorResultHandler("Error attempting set update interval, ignoring"));
    mCurrentDevice.setMediaSource(name, title, true, false).getAsync(new ErrorResultHandler("Error attempting to Play"));
}
private void doStop() {
     if (mCurrentDevice != null) {
         mCurrentDevice.stop().getAsync(new FutureListener<void>() {
             @Override
              public void futureIsNow(Future<void> result) {
                  try {
                      result.get();
                  } catch(ExecutionException e) {
                      //handleFailure
                  } catch(Exception e) {
                      //handleFailure
                  }
              }
         });
    }
}
private class ErrorResultHandler implements FutureListener<void> {
    ErrorResultHandler(String msg) {
        this(msg, false);
     }
    @Override
     public void futureIsNow(Future<void> result) {
         try {
             result.get();
          } catch(ExecutionException e) {
              //handleFailure
          } catch(Exception e) {
              //handleFailure
          }
    }
}
```

## Monitoring Remote Player Status with MediaPlayerStatus

Implement the `CustomMediaPlayer.StatusListener` in your controller app to monitor the status, condition, and position of the flung content on the remote Fire TV player app.  

```java
Status mStatus = new Status();

private static class Status {
    public long mPosition;
    public MediaState mState;
    public MediaCondition mCond;
}

private class Monitor implements StatusListener {
    @Override
    public void onStatusChange(MediaPlayerStatus status, long position) {
        synchronized (mStatus) {
            mStatus.mState  = status.getState();
            mStatus.mCond = status.getCondition();
            mStatus.mPosition = position;
        }
    }
}
```

The player app on Fire TV uses the `MediaPlayerStatus` class to set the content state (`MediaPlayerStatus.MediaState`) and condition (`MediaPlayerStatus.MediaCondition`). Your controller app can get this status with a state and condition through the `StatusListener`, and use this information to control the behavior of your app.

```java
public enum MediaState {
    NoSource, PreparingMedia, ReadyToPlay, Playing, Paused, Seeking, Finished, Error
};
public enum MediaCondition {
    Good, WarningContent, WarningBandwidth, ErrorContent, ErrorChannel, ErrorUnknown
};
public MediaState getState();
public MediaCondition getCondition();
```

## Next Steps

To create a custom Fling player for Fire TV, see [Integrating Amazon Fling into your Fire TV Application][integrating-amazon-fling-into-your-fire-tv-app].

If you are modifying an existing Google Cast app to support Amazon Fling, see [Integrating Amazon Fling with an Existing Android Cast App][integrating-amazon-fling-with-an-existing-android-cast-app].

If you use Android's MediaRouter in your app, see [Using Amazon Fling with Android MediaRouter][using-amazon-fling-with-android-mediarouter].

{% include links.html %}
