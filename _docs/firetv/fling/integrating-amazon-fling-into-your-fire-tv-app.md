---
title: Integrating Amazon Fling SDK into your Fire TV App
sidebar: fling
product: Fling SDK
permalink: integrating-amazon-fling-into-your-fire-tv-app.html
reviewers: jeffersd
github: true
---

A player is an application on Amazon Fire TV that acts as a server to a mobile device based controller app and allows the controller to play content on the Fire TV. You can implement a customized player that fits your banding and rendering needs. To create a new customized player or to enhance your existing Fire TV app to include flinging capability, you must use the player API, which is part of the SDK. The player application must:  

*  Identify itself uniquely to the controllers: The player app must provide a unique service identifier (SID) so that the controller app can discover the Fire TVs hosting this player app.
*  Announce itself to the framework: The underlying framework enumerates the player applications based on a XML file in the application package. The player app must include this with appropriate metadata.
*  Listen for incoming controller requests: The player app must implement a service derived from the `MediaPlayerHostService` in the player API.
*  Provide implementation of the player interface: The player app must provide an implementation of `CustomMediaPlayer` to serve incoming control requests from the controller app.

See [Understanding the Amazon Fling Service][understanding-the-amazon-fling-service] for a high-level overview of the features and functions this service provides. See also the player sample app, part of the SDK, for details on your player implementation. 

Before you start, make sure you've set up your development environment. See [Setting Up Your Amazon Fling Development Environment for Android][setting-up-your-amazon-fling-development-environment-for-android] for more details.  

## Starting Your Amazon Fire TV App Development

If you are creating a brand new Fire TV app as a customized player app, it would be good to start with the Amazon Fire TV app development guide here: [Getting Started Developing Apps and Games for Amazon Fire TV][getting-started-developing-apps-and-games-for-amazon-fire-tv].

While the latest generation of the Amazon Fire TV is based on Fire OS 5 (API 22), please target your applications to API 17, to cover older generation Amazon Fire TVs in the market, which are based on Fire OS 3. If you're an Android developer, working with the Fire TV platform is easy and familiar. Fire TV uses the same tools and APIs you're already used to for Android development.

```java
uses-sdk android:minSdkVersion="10" android:targetSdkVersion="17"/>
```

## Defining the Player SID

Your player app on Fire TV is identified by a unique service identifier (SID). This unique SID is set in your project. The underlying framework broadcasts the SID over the network for mobile device based controller apps to discover. You can make the SID unique by adding "_com.your.organization._" prefix to the SID.

To define the SID for your app and advertise it to the underlying framework, the following steps must be taken:

1.  Add a file called `Whisperplay.xml` to your app's resources, in the `res/xml/` directory. The contents of the file should look like this:
    
    ```xml
    <whisperplay>
        <services>
            <service>
                <sid>com.your.organization.custom.player</sid>
                <accessLevel>ALL</accessLevel>
                <startService>com.your.organization.example.ExamplePlayerService</startService>
            </service>
        </services>
    </whisperplay>
    ```
    
    Here the `<sid>` element defines the SID and `<startService>` element refers to the listener service (see the next section for details).
    
2.  Add a reference to the WhisperPlay XML file in your `AndroidManifest.xml` file.
    
    ```java
    meta-data android:name="whisperplay" android:resource="@xml/whisperplay" />
    ```
    
3.  Add a "uses" library in your `AndroidManifest.xml` file
    
    ```xml
    <uses-library android:name="com.amazon.whisperplay.contracts" />
    ```
    
4. Implement abstract function `getPlayerId()` in `MediaPlayerHostService`. See the next section for details.

## Implementing a Listener Service

The player app must implement a service that can be started by the underlying framework when a controller app invokes a remote procedure call. This service derives from MediaPlayerHostService, which provides most of the listener functionality. The following methods are left to you to implement:

```java
public class ExamplePlayerService extends MediaPlayerHostService {
    // Create the media player implementation that will handle remote method invocations
    @Override
    public CustomMediaPlayer createServiceImplementation() { return new ExamplePlayer(this); }
    // Communicate the unique service identifier (SID) of your player app
    @Override
    public String getPlayerId() { return "com.your.organization.custom.player"; }
    @Override
    public IBinder onBind(Intent intent) { return null; }
}
```

```java
public class ExamplePlayer implements CustomMediaPlayer {
    //Media player (e.g. Android MediaPlayer)
    private MediaPlayer mPlayer;

    @Override
    public void setMediaSource(String mediaLoc, String metadataJson,
                                     boolean autoPlay, boolean playInBg) throws IOException {
        //Implementation for your custom player.
    }
    .
    .
    .
}
```

## Creating a Media Player

Your customized player's primary function is to render the URLs that are being sent from the controller apps. Your media player must implement the `RemoteMediaPlayer` API interface so that the controller can use it as its target player. The actual implementation of APIs such as `setMediaSource()`, `play()`, `pause()`, etc. depends on your intended user experience. 

For a player app that is designed to play videos, please refer to the player sample project in the SDK and see the topics from [`MediaPlayer`](http://developer.android.com/reference/android/media/MediaPlayer.html), [`SurfaceView`](http://developer.android.com/reference/android/view/SurfaceView.html), [`SurfaceHolder`](http://developer.android.com/reference/android/view/SurfaceHolder.html), and [`AudioManager`](http://developer.android.com/reference/android/media/AudioManager.html). If your app is a picture viewer, your media player will ignore the APIs for playable media such as `seek()`.

You must also support status listeners. The controller app can subscribe to the status change notifications through the `addStatusListener()` interface. When the playback status changes, the player app must send the notification to all the current listeners. When a media (audio or video) is playing, it is a good practice to send regular status updates with current position.

{% include links.html %}