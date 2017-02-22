---
title: Integrating the Amazon Fling Player In Your Games
sidebar: fling
product: Fling SDK
permalink: unity-player-plugin.html
reviewers: jeffersd
github: true
toc: false
---

This page will walk you through the steps of setting up your Unity project to include the Amazon Fling Player Unity Plugin. It will also provide instruction on how to use the plugin. For additional information see the FlingPlayerSampleApp in the samples folder of the plugins SDK.

* TOC
{:toc}

## Import the Unity Package

When you import the Amazon Fling Player Unity package, Unity will override existing files in your project with those in the package. Before importing the Unity package, review the list of included files and compare this list with your current project to check for overlap.

If a file exists in both your project and the package, verify that any changes you have made locally are backed up somewhere outside of your project. After you import the package, use your favorite file merging software to compare and merge the differences between the file in the package and the file you had backed up locally.

1.  In Unity, click **Assets**, click **Import Package**, and then click **Custom Package…**  
2.  Select the plugin package.  
3.  Import the Package.

## Initialize the Player

Initialize the service using a unique service ID. This service ID will need to match the service ID in the whisperplay.xml file described in the additional steps section.

```java
IAmazonFlingPlayerService flingPlayerService;
flingPlayerService = AmazonFlingPlayerServiceImpl.Instance;
StartServiceParameters startServiceParameters = new StartServiceParameters();
startServiceParameters.PlayerId = "your.unique.service.id";
flingPlayerService.StartService(startServiceParameters);
```

## Method Calls

To initiate a method call, you must perform the following general steps:

1.  If needed, construct the object used to pass input to the operation.
2.  Call the operation, passing in any required input as arguments.

**Example**:

```java
MediaPlayerInfo mediaPlayerInfo = new MediaPlayerInfo ();
mediaPlayerInfo.Source = setMediaSourceParameters.Source;
mediaPlayerInfo.Metadata = setMediaSourceParameters.MetadataJson;
flingPlayerService.UpdateMediaPlayerInfo(mediaPlayerInfo);
```

| Method | Summary |
|------|---------|
| `void StartService(StartServiceParameters startServiceParameters)` | Starts the player service to allow controllers to interact with it. The startServiceParameters.PlayerID used will need to match the service ID of the controller trying to connect to it. |
| `void UpdateVolume(Volume volume)` | Updates the volume of the player service. |
| `void UpdateMute (Mute mute)`| Updates the mute status of the player service. This method will trigger a callback to any status listeners added by a controller.|
| `void UpdatePosition(Position position)` | Updates the position of the player service. This method will trigger a callback to any status listeners added by a controller.|
| `void UpdateDuration(Duration duration)`| Updates the duration of the player service. This method will trigger a callback to any status listeners added by a controller. |
| `void UpdateMediaState(MediaState mediaState)` |  Updates the media state of the player service. This method will trigger a callback to any status listeners added by a controller. |
| `void UpdateMediaCondition(MediaCondition mediaCondition)` | Updates the media condition of the player service. This method will trigger a callback to any status listeners added by a controller. |
| `void AddSupportedMimeType(MimeType mimeType)` | Adds a mime type to the player service’s mime type list. The list is used when a controller calls the IsMimeTypeSupported() API on the controller. |
| `void UpdateMediaPlayerInfo(MediaPlayerInfo mediaPlayerInfo)`| Updates the media player info of the player service. This method will trigger a callback to any status listeners added by a controller. |

## Handling Events

To handle an event, you must perform the following steps:

1.  Define an event handler.
2.  Register your event handler with an event listener.

**Example:**

```java
private void SetMediaSourceCallback(SetMediaSourceParameters setMediaSourceParameters) {}

flingPlayerService.AddSetMediaSourceListener(SetMediaSourceCallback);
```

| ----- |
| Method |  Summary |
|  `void AddSetVolumeListener(SetVolumeDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to set the volume. |
|  `void AddSetMuteListener(SetMuteDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to set the mute status. |
|  `void AddPauseListener(PauseDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to pause the media. |
|  `void AddPlayListener(PlayDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to play the media. |
|  `void AddStopListener(StopDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to stop the media. |
|  `void AddSeekListener(SeekDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to seek the media. |
|  `void AddSetMediaSourceListener(SetMediaSourceDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to set the media source. |
|  `void AddSetPlayerStyleListener (SetPlayerStyleDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a request to set the player style. |
|  `void AddSendCommandListener(SendCommandDelegate responseDelegate)` |   Registers the delegate with the player service to be called when a controller sends a command. |  

## Additional Step for Deployment

The player needs access permissions to communicate over the net, and the below permissions are required to be added to your AndroidManifest.xml:

*  `android.permission.INTERNET`: Allows applications to open network sockets.
*  `android.permission.ACCESS_WIFI_STATE`: Allows applications to access information about Wi-Fi networks.
*  `android.permission.ACCESS_NETWORK_STATE`: Allows applications to access information about networks.

These permissions can be added manually or by renaming the Plugins/Android/AmazonFlingPlayerServiceSampleAndroidManifest.xml file to AndroidManifest.xml.

The player also needs access to the WhisperPlay library on Fire TV and needs to tell the library what activity/service to start when a controller device tries to communicate with your app. WhisperPlay will start the activity/service for you if it is not already running. This is achieved by adding the following lines to your AndroidManifest.xml file. These permissions are also already included in the manifest file in the plugin, which you can rename and use.

```java
uses-library android:name="com.amazon.whisperplay.contracts"/>
<meta-data android:resource="@xml/whisperplay" android:name="whisperplay"/>
```

The whisperplay.xml file located in the Android/res/xml/ directory of the plugin specifies the activity/service that will be called if a controller device tries to use your service when it is not running. It also tells the player what service id to expect to connect to after it starts the application. You should update this file to have your unique service id in the `<sid>` section and the activity/service you would like to start in the `<startAction>` section (this is by default set to the activity defined in the AmazonFlingPlayerServiceSampleAndroidManifest.xml file):

```java
<whisperplay>
    <services>
        <service>
            <sid>your.unique.service.id</sid>
            <accessLevel>ALL</accessLevel>
            <startAction>com.unity3d.player.UnityPlayerNativeActivity</startAction>
        </service>
    </services>
</whisperplay>
```

{% include links.html %}
