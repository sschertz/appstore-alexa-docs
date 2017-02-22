---
title: Integrating the Amazon Fling Controller In Your Games
sidebar: fling
product: Fling SDK
permalink: unity-controller-plugin.html
reviewers: jeffersd
github: true
toc: false
---

This page will walk you through the steps of setting up your Unity project to include the Amazon Fling Controller Unity plugin. It will also provide instruction on how to use the plugin and additional steps for building for iOS, Android, and Fire OS. For additional information see the FlingSampleApp in the samples folder of the plugins SDK.  

* TOC
{:toc}

## Import the Unity Package

When you import the Amazon Fling Unity package, Unity will override existing files in your project with those in the package. Before importing the Unity package, review the list of included files and compare this list with your current project to check for overlap. If a file exists in both your project and the package, verify that any changes you have made locally are backed up somewhere outside of your project. After you import the package, use your favorite file merging software to compare and merge the differences between the file in the package and the file you had backed up locally.

1.  In Unity, click **Assets**, click **Import Package**, and then click **Custom Package…**
2.  Select the plugin package.  
3.  Import the Unity Package.

## Initialize

Initialize the service using the service ID of the service you would like to communicate with on the Fire TV. To communicate with the default media receiver, leave the service ID as null.

```java
IAmazonFlingService flingService;
flingService = AmazonFlingServiceImpl.Instance;
```

## Method Calls

Synchronized and ASynchronized methods are used to interact with the library. Since some methods make a call over a network ASync methods are provided to allow the application to continue processing while it is waiting for a response. To initiate a method call, you must perform the following general steps:

1.  If needed, construct the object used to pass input to the operation.  
2.  For asynchronous operations, define a callback function that is invoked when a response is ready.  
3.  Call the operation, passing in any required input and/or callback function as arguments.

### Synchronized Methods

**Usage Example**

```java
PlayerServiceId serviceId = new PlayerServiceId();
serviceId.Id = null;
flingService.StartDiscovery (serviceId);
```

| Method | Summary |
|------|---------|
| `void StartDiscovery(PlayerServiceId playerServiceId)` | Begins searching for players hosting a service with a service Id matching `playerServiceId.Id`. `playerServiceId.Id` can be left as null to search for the default media player. |
| `void StopDiscovery()` |  Stops searching for devices. |

### ASynchronized Methods

**Usage Example**

```java
private void GetDurationCallback(LongResult longResult) {}
private void FlingCallbackError(AmazonException e) {}
flingService.GetDuration(selectedPlayer, GetDurationCallback, FlingCallbackError);
```

| Method | Summary |
|------|---------|
| `VoidResult SetVolume(SetVolumeParameters setVolumeParameters)` | Sets the volume on the player. Note: the default media player does not implement this API. |
| `void IsMute(Player player, BooleanResult booleanResultDelegate, AmazonExceptionDelegate errorDelegate)` | Gets the mute status of the player. |
| `void SetMute(MuteParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Sets the mute status of the player. Note: the default media player does not implement this API. |
| `void GetPosition(Player player, LongResult longResultDelegate, AmazonExceptionDelegate errorDelegate)` | Returns the position of the media on the player. |
| `void GetDuration(Player player, LongResult longResultDelegate, AmazonExceptionDelegate errorDelegate)`| Returns the duration of the media on the player. |
| `void GetStatus(Player player, Status statusDelegate, AmazonExceptionDelegate errorDelegate)` | Gets the status of the player. |
| `void IsMimeTypeSupported(IsMimeTypeSupportedParameters isMimeTypeSupportedParameters, BooleanResult booleanResultDelegate, AmazonExceptionDelegate errorDelegate)` | Returns a Boolean indicating if the mime type is supported. |
| `void Pause(Player player, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Pauses the media the player. |
| `void Play(Player player, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Plays the media on the player. |
| `void Stop(Player player, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Stops the media on the player. |
| `void Seek(PlayerSeekParameters playerSeekParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)`|Seeks to the desired position on the player. |
| `void SetMediaSource (SetMediaSourceParameters setMediaSourceParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)`| Sets the URL, and MetaData on the player. Also has options to auto play the media when set and play in background. |
| `void SetPlayerStyle(SetPlayerStyleParameters setPlayerStyleParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)`| Sets the style of the media player on the device. Note: the default media player does not implement this API. |
| `void AddStatusListener(Player player, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Adds a listener to the player that will allow it to trigger the OnStatusChanged event. |
| `void RemoveStatusListener(Player player, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)`| Removes the listener to the player. |
| `void SetPositionUpdateInterval(SetPositionUpdateIntervalParameters setPositionUpdateIntervalParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)`| Sets the interval that dictates the amount of time between OnStatusChanged event calls. |
| `void SendCommand(SendCommandParameters sendCommandParameters, VoidResult voidResultDelegate, AmazonExceptionDelegate errorDelegate)` | Sends a command to the player. Note: the default media player does not implement this API. |
| `void GetMediaInfo(Player player, PlayerInfo playerInfoDelegate, AmazonExceptionDelegate errorDelegate)`| Gets the media info from the player. |

## Handling Events

To handle an event, you must perform the following steps:

1.  Define an event handler.
2.  Register your event handler with an event listener.

**Example:**

```java
private void PlayerDiscovered(Player args){}

flingService.AddPlayerDiscoveredListener(PlayerDiscovered);
```

| Method | Summary |
|------|---------|
| `void AddPlayerDiscoveredListener(PlayerDiscoveredDelegate responseDelegate)`| Registers the delegate with the service to be called when a player device is discovered. Returns a player representing a device hosting an Amazon Fling service as a parameter. |
| `void AddPlayerLostListener(PlayerLostDelegate responseDelegate)` | Registers the delegate with the service to be called when a player device is no longer available. Returns the player that is no longer available as a parameter. |
| `void AddDiscoveryFailureListener (DiscoveryFailureDelegate responseDelegate)`| Registers the delegate with the service to be called when a failure occurs during discovery. |
| `void AddOnStatusChangedListener(OnStatusChangedDelegate responseDelegate)` | Registers the delegate with the service to be called when a player has updated status information. Requires you to register a listener with that device using the AddStatusListener method. Returns the player, position and status as a parameter.|


## Additional Steps for iOS Deployment

Add Plugin SDK Frameworks to Your Exported iOS Xcode Project:

1.  In Xcode, select your Xcode project root.  
2.  Select the target **Unity-iPhone**.  
3.  Click the **Build Phases** tab.  
4.  Open the **Link Binary with Libraries** foldout.  
5.  Click the **+** at the bottom of this foldout.  
6.  Add all frameworks and libraries required by SDK  
    *  `AmazonFling.framework` (located in the framework directory of the SDK)  
    *  `Bolts.framework` (located in the framework directory of the SDK)
    *  `AdSupport.framework`  
    *  `Security.framework`  
    *  `Libc++.dylib`  
7.  Follow iOS SDK's specific guidelines for importing dependencies

## Additional Step for Android Deployment

The plugin needs access permissions to communicate over the net, and the below permissions are required to be added to your AndroidManifest.xml:

*  `android.permission.INTERNET`: Allows applications to open network sockets.  
*  `android.permission.ACCESS_WIFI_STATE`: Allows applications to access information about Wi-Fi networks.  
*  `android.permission.ACCESS_NETWORK_STATE`: Allows applications to access information about networks.  
*  `android.permission.CHANGE_WIFI_MULTICAST_STATE`: Allows applications to enter Wi-Fi Multicast mode.  

 These permissions can be added manually or by renaming the Plugins/Android/AmazonFlingServiceSampleAndroidManifest.xml file to AndroidManifest.xml.

## Additional Step for Fire OS Deployment

The plugin needs access permissions to communicate over the net, and the below permissions are required to be added to your AndroidManifest.xml:

*  `android.permission.INTERNET`: Allows applications to open network sockets.  
*  `android.permission.ACCESS_WIFI_STATE`: Allows applications to access information about Wi-Fi networks.  
*  `android.permission.ACCESS_NETWORK_STATE`: Allows applications to access information about networks.  

 These permissions can be added manually by creating a Plugins/Android/AndroidManifest.xml file or by renaming and modifying the Plugins/Android/AmazonFlingServiceSampleAndroidManifest.xml file.

Remove the Plugins/Android/WhisperPlay.jar and add the following line to the Plugins/Android/AndroidManifest.xml in the application section:

```java
uses-library android:name="com.amazon.whisperplay.contracts">
```

{% include links.html %}
