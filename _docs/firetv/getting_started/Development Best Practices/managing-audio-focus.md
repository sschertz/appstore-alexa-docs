---
title: Managing Audio Focus on Fire TV
permalink: managing-audio-focus.html
sidebar: firetv
product: Fire TV
reviewers: Andy Haldeman, Zsolt Matyas, Stephen Whitney
toc: false
github: true
---

Audio playback on Amazon Fire TV is a shared resource. Although apps such as music players can play audio when they are not in the foreground, only one app should be playing audio at once.

To manage audio playback across apps, Android provides the concept of audio focus via the [AudioManager](https://developer.android.com/reference/android/media/AudioManager.html) APIs. This document outlines how to manage audio focus on the Fire TV platform.

* TOC
{:toc}

## General Best Practices for Managing Audio Focus

For the best user experience, do the following in your playback app:

-   **[Run Your App as a Foreground Service](#foreground-service)**. During playback, run your app as a foreground service to prevent it from being unexpectedly halted by the system.
-   **[Request Audio Focus](#audio-focus)**. At playback time, request audio focus.
-   **[Register to Receive Media Button Events](#receiving-media-button-events)**. When you have the audio focus, register to receive the controller. Release the media buttons if your app loses focus, or when you’re done playing.
-   **[Create a Focus Change Listener](#focus-change-listener)**. Listen for changes to audio focus and respond appropriately.

Each of these best practices is explained in the sections below.

### Run Your App as a Foreground Service {#foreground-service}

Services are used to perform operations in the background when the user may not be directly interacting with your app. Media players, especially those that play music, are ideal for services.

However, services may be halted unexpectedly by the Fire TV platform if system resources run low. To give your playback app higher priority on the system and prevent it from unexpectedly being killed, make sure you run your app during playback as a foreground service by extending from [`Service`][service-android] and using the [`Service.startForeground()`][startforeground] method.

The `startForeground()` method requires two parameters that indicate a notification: a notification ID you define, and a Notification object. On Android devices, that notification appears in the notification drawer to indicate to the user that a high-priority service is running.

On the Fire TV platform, heads-up notifications (high priority) display for a few seconds and fade out. You can view older notifications in the [Notifications Center][notifications-for-amazon-fire-tv]. You can use the [Android Notification API][notificationapi] to create a heads-up notification:

```java
// define the notification
Notification.Builder builder = new Notification.Builder(getApplicationContext());
builder.setSmallIcon(R.drawable.notification_icon);
builder.setContentTitle(title);
builder.setContentText(text);
builder.setPriority(Notification.[PRIORITY_HIGH][4]);
builder.setType(Notification.Builder.TYPE_MEDIA_INFO);
Notification notification = builder.build();

// start
startForeground(1, notification);
```

After you’ve stopped playing, remove your service from the foreground with [`Service.stopForeground()`][stop-foreground]. The boolean parameter indicates whether to remove the associated notification.

```java
stopForeground(true);
```

### Request Audio Focus {#audio-focus}

Audio output is a shared resource on the Fire TV platform — only one audio stream should be playing at any one time. The Android audio manager uses the concept of audio focus to enable apps to share audio playback, and to pause, stop, or lower the volume in response to other apps needing that focus.

An application playing back audio must first request the audio focus with appropriate flags and continue playback only if focus is granted. The app can continue to play audio in the background if and only if it still has audio focus.

Request the audio focus with [*AudioManager.requestAudioFocus()*][requestaudiofocus]. The request has three parameters: a focus listener (see [Creating a Focus Change Listener](#focus-change-listener), below), an audio stream type (such as [AudioManager.STREAM\_MUSIC](http://developer.android.com/reference/android/media/AudioManager.html#STREAM_MUSIC)), and a duration.

```java
AudioManager am = (AudioManager) context.getSystemService(Context.AUDIO_SERVICE);
int result = am.requestAudioFocus(focusListener, AudioManager.STREAM_MUSIC, AudioManager.AUDIOFOCUS_GAIN);

if (result == AudioManager.AUDIOFOCUS_REQUEST_GRANTED) {
    am.registerMediaButtonEventReceiver(MediaButtonReceiver);
    // start playback
}
```

The duration parameter can be one of several options:

-   [`AUDIOFOCUS_GAIN`](http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_GAIN)
    (permanent focus): Use for when you want to hold the focus and
    continue playing audio for the foreseeable future, as with a
    music player.

-   [`AUDIOFOCUS_GAIN_TRANSIENT`][audiofocus-loss-transient] (transient focus): Use for when you want to interrupt any existing
    audio playback for a short period of time, and that other playback
    should pause while you are holding the focus.

-   [`AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK`][audiofocus-gain-transient-may-duck] (transient with ducking): Same as transient, only the existing playback continues with the volume lowered, muted, or the playback paused.

-   [`AUDIOFOCUS_GAIN_TRANSIENT_EXCLUSIVE`](https://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_GAIN_TRANSIENT_EXCLUSIVE)
    (transient focus, mutes other sounds): Same as transient but mutes
    other sounds, including system sounds, in a temporary way. If you’re
    incorporating voice recognition into your app, or if you have a
    microphone input, this parameter can be useful.

After audio focus is granted to your app, you can register the handling of media events (such as Next, Fast Forward, Previous, or Play buttons on headsets, external keyboards, or remote controls) with the [`AudioManager.registerMediaButtonEventReceiver()`][mediabuttonreceiver] method (see the [next section](#media-button-events)). This is only necessary for continuous audio playback — typically you don’t need to own the media buttons for transient playback.

After you’re done playing, release the audio focus with the method and unregister the media buttons.

```java
// playback complete, give up audio focus
am.abandonAudioFocus(focusListener);

//unregister media buttons
am.unregisterMediaButtonEventReceiver(MediaButtonReceiver);
```

### Register to Receive Media Button Events {#media-button-events}

If you plan to play audio for more than a few moments (that is, you’ve requested non-transient audio focus), register a media button receiver when your audio focus request is granted.

The media button receiver enables the user to control your media playback with the media buttons on the remote control, even if your app is not running in the foreground.

Note that registering the media buttons just means your app receives those events before other apps – you must still handle those key events to actually control your media playback.

Your media button receiver extends from [`BroadcastReceiver`][broadcastreceiver] and listens for an [`ACTION_MEDIA_BUTTON`][action-media-button] intent. Register your receiver in your Android manifest (in this example, the receiver class is `MediaButtonReceiver)`:

```java
<receiver android:name="MediaButtonReceiver" >
    <intent-filter>
        <action android:name="android.intent.action.MEDIA_BUTTON" />
    </intent-filter>
</receiver>
```

Implement the [`onReceive()`][onreceive] callback in your receiver class to receive and handle media button intents:

```java
public void onReceive(Context context, Intent intent) {
    if (Intent.ACTION_MEDIA_BUTTON.equals(intent.getAction())) {
        KeyEvent key = (KeyEvent) intent.getParcelableExtra(Intent.EXTRA_KEY_EVENT);
        int keycode = key.getKeyCode();

        if (keycode == KeyEvent.KEYCODE_MEDIA_NEXT) {
            // next
        } else if (keycode == KeyEvent.KEYCODE_MEDIA_PREVIOUS) {
           // previous
        } else if (keycode == KeyEvent.KEYCODE_MEDIA_PLAY_PAUSE) {
           // play or pause
        }
    }
}
```

Once you’re done playing, or if you lose the audio focus, unregister the media buttons so that other apps can control media playback when needed.

### Create a Focus Change Listener {#focus-change-listener}

Audio focus is a cooperative resource. Once you’ve been granted audio focus, it is not yours to hold until you’re done with it. If other apps request the audio focus, you must respond to those requests to pause playback or stop playing altogether. See Android’s [Managing Audio Focus][managingaudiofocus-android] topic for more details.

{% include note.html content="The Fire TV platform does not support volume control from within your app. Although Android distinguishes between a transient loss of focus with or without ducking (lowering the volume), your app should pause playback for both these cases." %}

Listen for audio focus changes with
[`AudioManager.OnAudioFocusChangeListener`][change-listener] and the [`onAudioFocusChange()`][on-audio-focus-change] method. Create this listener as you would any listener.

```java
OnAudioFocusChangeListener focusListener = new OnAudioFocusChangeListener() {
    public void onAudioFocusChange(int focus) {
        switch (focus) {
            case AudioManager.AUDIOFOCUS_GAIN:
                // continue playback and raise volume (if it was previously lowered)
                // ...
                break;

            case AudioManager.AUDIOFOCUS_LOSS:
                // stop playback, de-register buttons, clean up
                // ...
                break;

            case AudioManager.AUDIOFOCUS_LOSS_TRANSIENT:
            case AudioManager.AUDIOFOCUS_LOSS_TRANIENT_CAN_DUCK:
                // pause playback
                // ...
                break;
        }
    }
}
```

The argument for `onAudioFocusChange()` is an integer that may be one of the following:

-   [`AUDIOFOCUS_GAIN`][audiofocus-gain]): Your app has gained or regained the audio focus. Continue playback or raise the volume.

-   [`AUDIOFOCUS_LOSS`][audiofocus-loss]: Your app has lost the audio focus (likely permanently). Halt playback and de-register the media button receiver.

-   [`AUDIOFOCUS_LOSS_TRANSIENT`][audiofocus-loss-transient]: Your app has lost audio focus for a short amount of time.
    Pause playback.

-   [`AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK`][audiofocus-transient-can-duck]: Same as transient; pause playback.

## Handling Audio Focus with Voice Interactions

When users press the Microphone button, system-wide voice capabilities start and cannot be overridden. Your app should pause (implement the `OnPause()` method) in response to voice interactions. If you need search functionality within your app, you must implement it yourself and provide your own user interface for that function.

For audio apps, the voice activity requests a **transient audio focus** (without ducking – that is, lowering the volume). As such, you should handle the `AUDIOFOCUS_LOSS_TRANSIENT` event to accommodate voice search.

In addition to handling `AUDIOFOCUS_LOSS_TRANSIENT`, your application should handle other audio focus change events as well (`AUDIOFOCUS_LOSS` and `AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK`), since any application (not just voice search) might request any kind of audio focus. In short, all audio focus use cases should be handled appropriately.

For backwards compatibility with older applications on the Fire TV platform, we send out both an `AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK` and an `AUDIOFOCUS_LOSS_TRANSIENT` when a voice search is initiated. Your applications should handle this use case correctly.

{% include tip.html content="As a best practice, implement all audio focus change events in your app." %}

See the Android documentation on [Managing Audio Focus][managingaudiofocus-android] for details on audio focus and audio ducking. For more information about voice search and the results returned from the Fire TV catalog, see [Implementing Search on Fire TV][implementing-search-fire-tv].

For video apps, your app should pause playback when voice capabilities are invoked (in your `OnPause()` method). When your app resumes, playback should remain paused until the user presses PLAY.

{% include note.html content="If you need search functionality in your app, you must implement **text search** yourself and provide your own user interface for the search function." %}

## Code Samples for Handling Audio Focus

Several Java code samples are available that demonstrate how to handle audio focus. The code samples are as follows:

-   AudioCapabilities.java
-   AudioDeviceEventReceiver.java
-   AudioEventManager.java
-   AudioEventManagerCallbackListener.java

You can download the code samples here:
[**AudioFocusHandling.zip**](https://s3.amazonaws.com/fire-tv-code-samples/AudioFocusHandling.zip).

These code samples are also available from the [SDK download page](https://developer.amazon.com/public/resources/development-tools/sdk) in the “Other downloads” section.

## Best Practices for Audio-only Applications

If you have an audio-only application, such as a streaming radio player, the following table provides guidelines about how your app should respond to various events when an audio stream is playing:

<table class="grid">
<colgroup>
<col width="15%" />
<col width="25%" />
<col width="15%" />
<col width="15%" />
<col width="30%" />
</colgroup>
<thead>
<tr>
<th>Event Type</th>
<th>AUDIOFOCUS EVENT</th>
<th>Listen to Audio Focus Events?</th>
<th>Listen to Media Controller Events?</th>
<th>Customer/App Experience</th>
</tr>
</thead>
<tbody>
<tr>
<td>Home button press</td>
<td>(none)</td>
<td>Yes</td>
<td>Yes</td>
<td>Audio stream continues to play. Media controller buttons (for example, play/pause) continue to work.</td>
</tr>
<tr>
<td>Microphone button press</td>
<td><code>AUDIOFOCUS_LOSS_TRANSIENT</code></td>
<td>Yes</td>
<td>Yes</td>
<td>Audio stream pauses. When user exits the Alexa overlay, audio stream resumes. Media controller buttons continue to work.</td>
</tr>
<tr>
<td>HDMI cable unplugged</td>
<td><code>ACTION_HDMI_AUDIO_PLUG</code></td>
<td>Yes</td>
<td>Yes</td>
<td>Audio stream pauses. When user reconnects HDMI cable, audio stream is in paused state. Media controller buttons continue to work. Audio stream resumes when Play button is pressed.</td>
</tr>
<tr>
<td>HDMI input switched</td>
<td><code>ACTION_HDMI_AUDIO_PLUG</code></td>
<td>Yes</td>
<td>Yes</td>
<td>Audio stream pauses. When user switches HDMI input back to Fire TV, audio stream is in paused state. Media controller buttons continue to work. Audio stream resumes when Play button is pressed.</td>
</tr>
<tr>
<td>New audio stream is started by different app</td>
<td><code>AUDIOFOCUS_LOSS</code></td>
<td>No</td>
<td>No</td>
<td>Audio stream stops. App stops listening to changes in audio focus and stops listening to media controller events. App cleans up resources used to listen to audio.</td>
</tr>
</tbody>
</table>

[broadcastreceiver]: http://developer.android.com/reference/android/content/BroadcastReceiver.html
[mediabuttonreceiver]: http://developer.android.com/reference/android/media/AudioManager.html#registerMediaButtonEventReceiver(android.content.ComponentName)
[requestaudiofocus]: http://developer.android.com/reference/android/media/AudioManager.html#requestAudioFocus(android.media.AudioManager.OnAudioFocusChangeListener, int, int)
[startforeground]: http://developer.android.com/reference/android/app/Service.html#startForeground(int, android.app.Notification)
[notificationapi]: http://developer.android.com/reference/android/app/Notification.html
[onreceive]: https://developer.android.com/reference/android/content/BroadcastReceiver.html#onReceive(android.content.Context, android.content.Intent)
[managingaudiofocus-android]: http://developer.android.com/training/managing-audio/audio-focus.html
[audiofocus-gain]: http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_GAIN
[audiofocus-transient-can-duck]: http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK
[audiofocus-gain-transient-may-duck]: http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK
[audiofocus-loss-transient]: http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_LOSS_TRANSIENT
[audiofocus-loss]: http://developer.android.com/reference/android/media/AudioManager.html#AUDIOFOCUS_LOSS
[service-android]: http://developer.android.com/reference/android/app/Service.html
[stop-foreground]: http://developer.android.com/reference/android/app/Service.html#stopForeground(boolean)
[action-media-button]: http://developer.android.com/reference/android/content/Intent.html#ACTION_MEDIA_BUTTON
[change-listener]: http://developer.android.com/reference/android/media/AudioManager.OnAudioFocusChangeListener.html
[on-audio-focus-change]: http://developer.android.com/reference/android/media/AudioManager.OnAudioFocusChangeListener.html#onAudioFocusChange(int)


{% include links.html %}
