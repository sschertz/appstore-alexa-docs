---
title: Amazon Fire TV Frequently Asked Questions (FAQs)
permalink: amazon-fire-tv-sdk-frequently-asked-questions.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

## General {#general}

Q: How do I submit my app for the Amazon Fire TV platform?
:   To submit your app for an Amazon Fire TV device, create an account on the [Amazon Apps & Games Developer Portal](https://developer.amazon.com/appsandservices) and submit your app through the portal.

    If you already have an app published at Amazon, update that app to include a separate binary APK for Amazon Fire TV. See [Using Device Targeting](https://developer.amazon.com/public/support/submitting-your-app/tech-docs/getting-started-with-device-targeting) for more information.

Q: Will my app work on the Amazon Fire TV platform?
:   Your app must be compatible with the specifications for Amazon Fire TV devices. See [Device Specifications][device-and-platform-specifications] for detailed device and feature specifications.

    Your app must comply the [Amazon Appstore Content Policy Requirements](https://developer.amazon.com/public/support/submitting-your-app/tech-docs/appstore-content-policy). Amazon also recommends that you test your app on your own and submit an update if you discover any problems.

Q:  How can I identify whether my app is running on an Amazon Fire TV device?
:   A: You can check for the feature `amazon.hardware.fire_tv`. If you absolutely must look for the Fire TV Gen 1 or Fire TV Gen 2 device, you can check for the MODEL `AFTB` or `AFTS` respectively, but always include a fallback for the feature. See [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices] for more information.

Q: Can I sideload my app onto Amazon Fire TV for testing?
:   Yes, with the Android Debug Bridge (ADB).  See [Getting Started Developing Apps and Games for Fire TV][getting-started-developing-apps-and-games-for-amazon-fire-tv] for more information. 

Q: Are Amazon Fire TV test devices available for developers?
:   Amazon does not provide test devices for developers.

    Amazon Fire TV devices are available for purchase on Amazon retail sites in the [US](http://www.amazon.com/Fire-TV-streaming-media-player/dp/B00CX5P8FC), [UK](http://www.amazon.co.uk/dp/B00KQEJBSW), and [Germany](http://www.amazon.de/dp/B00KQEIMY6). Check the product detail pages for country-specific availability.

Q: What specific features does the Amazon Fire TV platform support?
:   See [Device Specifications](/solutions/devices/fire-tv/docs/device-and-platform-specifications), [Fire tablets](https://developer.amazon.com/public/solutions/devices/fire-tablets "https://developer.amazon.com/sdk/fire.html") for detailed device and feature specifications.

Q: How do I get my app marketed on the Amazon Fire TV platform?
:   See [Marketing Your App](https://developer.amazon.com/public/resources/marketing-tools/marketing-your-app "Marketing Opportunities").

Q: How can I get more information about the Amazon Fire TV platform?
:   Please use the [Contact Us](https://developer.amazon.com/public/support/contact/contact-us "Contact Us") form to send us your questions.

Q:  Under what circumstances should my app pause, and how do I implement pause behavior?
:   A: Your app pauses when the Microphone (voice search), Home, or GameCircle buttons are pressed on one of the Fire TV remotes or on the Amazon Fire Game Controller. The Back button also pauses the current activity and resumes the previous activity on the stack, which may or may not be your app.

    To handle pause behavior, implement the `onPause()` method in your activity as you would in any other Android app. In your `onPause()` method, ensure that you save the user's state or position, so that when your app resumes the user is in the same place that they were before the pause.

    In addition, the following requirements apply to media apps:

    * Apps that play video should pause playback, and <b>must release all media resources such as decoders immediately on pause</b>, as these limited resources are hardware-based and memory-constrained. See [Releasing the Media Player](https://developer.android.com/guide/topics/media/mediaplayer.html#releaseplayer) and [`MediaPlayer.release()`](https://developer.android.com/reference/android/media/MediaPlayer.html#release()">) for details.
    * Apps that play audio may continue playing after a pause, but must relinquish audio focus if requested by another app. See [Managing Audio Focus](http://developer.android.com/training/managing-audio/audio-focus.html) for details.

Q:  Can I open an advertisement in the default web browser?
:   A: Amazon Fire TV does not include a browser app, so links to all URLs (advertisements or otherwise) do not work. You can use the Android [WebView](http://developer.android.com/reference/android/webkit/WebView.html) if you need to include a link to a web page in your app. Note, however, that the Android WebView requires you to manage your own cookies and authentication, as well as handle input events from controllers. (WebView in Android does support D-Pad navigation, but layout and navigation may need review.)

    See the blog post [Using WebViews on Tablets with HD Screens](https://developer.amazon.com/post/Tx3BY931C53CRLA/Using-WebViews-on-Tablets-with-HD-Screens) for tips on implementing an Android WebView. Although this post is specific to tablets, the advice can also be applied to TV.

Q:  Can I use the Amazon In-App Purchasing API in my Fire TV app?
:   A: Yes, Amazon's [In-App Purchasing (IAP)][getting-started-with-iap] API works on all Fire TV devices and with the Fire TV remotes and game controllers. Use [App Tester][testing-iap] to preview your transactions.

Q:  Can I use Amazon Device Messaging (ADM) in my Fire TV app?
:   A: You can use [Amazon Device Messaging][adm-understanding] to receive push messages and other data.

Q:  Can I use the Amazon Maps API in my Fire TV app?
:   A: There are no location services in Amazon Fire TV, so the [Amazon Maps API](https://developer.amazon.com/maps) is not supported at this time.

Q:  Can I use the Amazon Mobile Ads API in my Fire TV app? 
:   A: The [Amazon Mobile Ads API](https://developer.amazon.com/public/apis/earn/mobile-ads) is not supported on Fire TV at this time.

Q:  Can I link to my app's details page in the Amazon Appstore?
:   A: Yes, Amazon Appstore deep links work as they do for other apps. See [Linking to the Amazon Client](https://developer.amazon.com/appsandservices/apis/earn/in-app-purchasing/docs/deeplink) for more information on deep linking. Note, however, that the ability to rate apps is not currently supported by Amazon Fire TV. Your app should not prompt the user for a rating.

Q:  How do I change the onscreen keyboard to a numeric keypad?
:   A: You can use the standard Android mechanisms for configuring the IME (input method editor) for any [EditText](http://developer.android.com/reference/android/widget/EditText.html) widget. In XML, use `android:inputType="number"`:

    ```
    <EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/edittext"
    android:inputType="number"/>
    ```

Q:  How do I take screen shots on Amazon Fire TV?
:   A: To take screen shots, connect to the device with `adb` and use these commands. Substitute the name of your screen shot for `filename.png` and the directory on your development computer where you want to put the screen shot for `/tmp`:

    ```
    adb shell screencap -p /sdcard/filename.png
    adb pull /sdcard/filename.png /tmp
    adb shell rm /sdcard/filename.png
    ```
    See [Taking Screenshots of your Fire TV App](/support/submitting-your-app/tech-docs/04-taking-screenshots#firetv) for more details.

Q: My app uses Google’s in-app purchasing technology. Will it work on the Amazon Fire TV platform?
:   No. Services such as Google’s in-app purchasing API require access to Google Mobile Services, which do not work on the Amazon Fire TV platform. For in-app purchasing, Amazon offers an In-App Purchasing API that makes it easy for you to offer digital content and subscriptions in your apps.

Q: Are there any other criteria for distributing apps for Amazon Fire TV or Fire TV Stick in Germany and/or Austria?
:   Yes. We do not permit apps to be distributed on Amazon Fire TV or on Fire TV Stick in Germany and/or Austria, if the apps enable copying, recording, downloading, storing, or similar actions of any type of video or audio content onto the Amazon Fire TV or Fire TV Stick device, any SD memory card or any connected external storage (where applicable). If we determine that an app contains this functionality, we will not make it available in Germany and/or Austria.    

## Fire OS 5 (Android Lollipop) {#fireos}

Q: Does Fire OS 5 support Android TV and the v17 Leanback Library?
:   A: Fire OS 5 includes both support for Android TV functionality and the Leanback Library. However, speech recognition with the Leanback library’s [`SearchFragment`]((http://developer.android.com/reference/android/support/v17/leanback/app/SearchFragment.html)) class is currently unsupported. See [Implementing Search in Fire TV][implementing-search-fire-tv] for more details.

## Media and DRM {#mediaanddrm}

Q: What third-party media player SDKs are supported by Fire TV?
:   A: Any media player SDKs that implement the Android media playback and encryption APIs work on the Amazon Fire TV platform.  Examples of these SDKs include the [Amazon Port of the ExoPlayer](https://github.com/amzn/exoplayer-amazon-port), the [Android MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html), the [VisualOn OnStream MediaPlayer+ SDK](http://visualon.com/onstream-mediaplayer/), and the [NexStreaming NexPlayer SDK](http://www.nexstreaming.com/index.php). See [Media Players][fire-tv-media-players] for more details.

{% comment %}
Q:  Can I get a free VisualOn license from Amazon?
:   A: Free licenses for the [VisualOn OnStream Mediaplayer](http://visualon.com/onstream-mediaplayer) are no longer available from Amazon. If you previously integrated the VisualOn player in your Fire TV app using a license from Amazon, you will need to purchase your own license starting January 1, 2017, when the existing license expires. If you don't want to purchase your own VisualOn license, we recommend that you use the Amazon port of Exoplayer. For more information, see [Media Players][fire-tv-media-players].
{% endcomment %}

Q:  What are my options for implementing playback of media encrypted with Microsoft PlayReady DRM?
:   A: The Amazon Fire TV platform includes support for the Android `MediaCrypto` class for playing protected media, or you can use a third-party media player SDK that supports PlayReady. Amazon Fire TV supports PlayReady version 2.5 only. (Fire TV devices cannot support PlayReady Security Level 3000 (SL3000) because this was introduced in PlayReady 3.0, and Amazon Fire TV devices support version 2.5 only.)


Q:  Does Amazon Fire TV support PlayReady content that uses encrypted audio?
:   A: PlayReady content with encrypted video and clear (non-encrypted) audio is supported on Fire TV. Widevine DRM is also supported. See the [DRM section on the Fire TV Device Specifications page][device-and-platform-specifications] for more details. If you need to play content with both encrypted audio and video, please [contact us](https://developer.amazon.com/public/support/contact/contact-us) for further information.


Q:  How do I decode Dolby Digital audio on Fire TV devices?
:   A: Both Fire TV and Fire TV Stick contain decoders for Dolby Digital (AC3) and Dolby Digital Plus (eAC3). You should be able to play these formats with the standard Android media player libraries. Note that the DolbyAudioProcessing SDK for Amazon Fire tablets is not supported (or required) for Fire TV devices. For more information about playing Dolby, see [Dolby Integration Guidelines][amazon-fire-tv-dolby-integration-guidelines].


Q:  Does the device support Secure Boot to verify firmware and OS?
:   A: Yes.


Q:  Does the OS enforce signature checking of apps?
:   A: Yes.


Q:  Is output HDCP protected though a secure channel over HDMI?
:   A: Yes.


Q: Does Amazon Fire TV support customizable closed captioning (CEA-708)?
:   A: Fire TV has limited platform-level closed captioning support at this time. Media apps are responsible for and must implement a UI that enables viewers to customize  captions through a third-party media playback SDK or their own implementation.


Q: What fonts are available on Amazon Fire TV for use with closed captions?
:   A: Amazon Fire TV includes limited platform fonts. You must license and embed your own font files for each of the style categories defined by CEA-708.

## Media Playback {#mediaplayback}

Q:  What are best practices with media playback to preserve resources and memory?

:   A: Because the media playback resources in Amazon Fire TV are hardware-based and memory constrained, your app must be well-behaved and release the media resources when you are done with them.

    Specifically, apps that play video should pause playback and must release all media resources such as decoders immediately in your `onPause()` method. See [Releasing the Media Player][release-media-player] and <a href="https://developer.android.com/reference/android/media/MediaPlayer.html#release()"><code>MediaPlayer.release()</code></a> for details.

    If your app plays video through an Android [WebView](http://developer.android.com/reference/android/webkit/WebView.html) (with the `<video>` HTML tag), hardware media resources are not currently released by the system when your app pauses or stops. To work around this issue, explicitly release these resources by killing the processes when you implement your `onPause()` and `onStop()` methods:

    ```java
    public void onStop() {
       super.onStop();
       android.os.Process.killProcess(android.os.Process.myPid());
    }
    ```

Q: My app plays music in the background. When my app pauses, why does the audio stop playing? OR when my app starts, why does audio from another app keep playing?
:   A: Your app is not correctly managing audio focus. When your app begins playing it should both request audio focus and register for media button events. When your app relinquishes audio focus (because you're done playing or because another app has requested it), it should also unregister from media button events. Specifically:

    * When your app begins playing, request audio focus with [`AudioManager.requestAudioFocus()`](http://developer.android.com/reference/android/media/AudioManager.html#requestAudioFocus(android.media.AudioManager.OnAudioFocusChangeListener,%20int,%20int))
    * If audio focus was granted, register a media button receiver with [`AudioManager.registerMediaButtonEventReceiver()`](http://developer.android.com/reference/android/media/AudioManager.html#registerMediaButtonEventReceiver(android.content.ComponentName))
    * Listen for the loss of audio focus with [`AudioManager.onAudioFocusChangeListener()`](http://developer.android.com/reference/android/media/AudioManager.OnAudioFocusChangeListener.html)
    * If your app loses audio focus, stop playback and unregister the media buttons with [`AudioManager.unregisterMediaButtonEventReceiver()`](http://developer.android.com/reference/android/media/AudioManager.html#unregisterMediaButtonEventReceiver(android.content.ComponentName))

    See the following for more details:

    *  [Managing Audio Focus][managing-audio-focus] (Fire TV docs)
    *  [Managing Audio Focus](http://developer.android.com/training/managing-audio/audio-focus.html) (Android docs)
    *  [Allowing applications to play nice(r) with each other: Handling remote control buttons](http://android-developers.blogspot.com/2010/06/allowing-applications-to-play-nicer.html)

Q:  Why does my music app keep getting randomly killed when it's playing in the background?
:   A: Make sure you are running your music playback app as a foreground service. Background services (the default) are automatically shut down by the system when resources are low. See the Android guide for [Media Playback](http://developer.android.com/guide/topics/media/mediaplayer.html) and the [`startForeground()`](http://developer.android.com/reference/android/app/Service.html#startForeground(int,%20android.app.Notification)) method in the [Service](http://developer.android.com/reference/android/app/Service.html) class for details.

Q:  How should I handle the TV being turned off/disconnected with the HDMI cable?
:   A: The expected behavior for an HDMI disconnect is different for audio and video. See [Handling HDMI Events][fire-tv-handling-hdmi-events] for details.

Q:  During media playback, how do I prevent the device from entering standby or daydream mode (screen saver)?
:   A: To keep both Amazon Fire TV and the television awake during media playback, set the [`KEEP_SCREEN_ON`](http://developer.android.com/reference/android/view/View.html#KEEP_SCREEN_ON) flag for your activity, or acquire a wake lock from the power manager. Your app must release all wake locks when it is not actively playing audio or video so that both the device and the television can sleep and preserve power. See the [`PowerManager`](http://developer.android.com/reference/android/os/PowerManager.html) and [`PowerManager.WakeLock`](http://developer.android.com/reference/android/os/PowerManager.WakeLock.html) classes for details on using wake locks.

    Note that if your app plays audio and you set a partial wake lock for your app [`PARTIAL_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#PARTIAL_WAKE_LOCK), the television remains on and when idle the device enters daydream mode (displays the screen saver). This is different behavior from a partial wake lock on a mobile device (where it keeps the CPU on, but turns the screen off), as audio playback over HDMI requires the television to be on. Again, make sure you release the wake lock when your app stops actively playing audio so that the television can sleep.

    Setting the [`SCREEN_BRIGHT_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#SCREEN_BRIGHT_WAKE_LOCK) or [`SCREEN_DIM_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#SCREEN_DIM_WAKE_LOCK) flags has no effect on the behavior of the device.

Q:  How can I detect when the device has entered daydream mode (the screen saver is on)?
:   A: Daydream mode is an Android function. When Amazon Fire TV enters daydream mode it displays the screen saver if the TV is on. Register for `Intent.ACTION_DREAMING_STARTED` and `Intent.ACTION_DREAMING_STOPPED` to detect when the device enters or exits daydream.

    Note that if your app plays audio but is not a music app, you should pause audio playback when the device enters daydream mode.

Q:  Can I use an Android [WebView](http://developer.android.com/reference/android/webkit/WebView.html) and the `<video>` HTML tag for video playback?
:   A: Yes, with limitations. Hardware media resources are not currently released by the system when your app pauses or stops. To work around this issue implement your `onStop()` method to explicitly kill the process for your app:

    ```java
    public void onStop() {
    super.onStop();
    android.os.Process.killProcess(android.os.Process.myPid());
    }
    ```

    This issue may also cause instability in the user experience and navigation for your app. Use of a third-party media player SDK is the recommended method for media playback on the device.

Q:  Can I play 4K Ultra HD Videos in my App?

:   Yes, Fire TV (2nd Generation) can play 4K Ultra HD video when connected to a 4K-supported TV. Fire TV (1st Generation) and Fire TV Stick do not play 4K videos.

    Before playing 4K content, your app needs to check whether (1) the device in a Fire TV (2nd Generation) device and (2) whether the connected TV supports 4K. An Amazon 4K Extension Library has been developed to allow you to initiate the 4K mode switch.

    Apps that support Ultra HD video will be certified by Amazon to ensure they meet the required customer experience. Typically, certification takes a couple of weeks. See [Playing 4K Ultra HD Videos][fire-tv-4k-ultra-hd] for more details.


## Controllers {#controllers}

Q:  The Amazon Fire TV Game Controller does not have media buttons like the previous version did. How do I handle media playback?
:   A: Amazon Fire TV generates media input events for the analog stick presses (Play/Pause) and for the left (Rewind) and right (Fast Forward) shoulder buttons (L1/R1). If you do not use those buttons in your app or game, make sure you do not capture or throw away those button events so the user can control media playing in the background.

    If you repurposed the media buttons on the Amazon Fire Game Controller for other purposes in your app, users of the new Amazon Fire TV Game Controller cannot use that functionality without the buttons. Consider updating your app to use buttons common to both game controllers, and updating your on-screen hints.

Q: How do I manage volume control from the Amazon Fire TV Game Controller?
:   A: Amazon Fire TV can stream audio to the headphone jack on the Amazon Fire TV Game Controller (current generation devices only). The left and right trigger buttons (L2/R2) are used to control the volume.

    Volume control is a system function and cannot be mapped to other buttons in your app. However, if you do not use the trigger buttons in your app or game, make sure you do not capture or throw away those button events so the user can control media playing in the background. If you do use these buttons for other purposes in your app or game, consider providing an on-screen hint that the user can control the volume from the GameCircle screen or the system launcher.

Q:  Can I override the Microphone button on the Fire TV Voice Remote in my app?
:   A: The Microphone button launches system-wide voice capabilities (requesting a **transient audio focus**) and cannot be overridden. Your application should handle this audio focus change event (`AUDIOFOCUS_LOSS_TRANSIENT`) as well as other audio focus change events too (`AUDIOFOCUS_LOSS` and `AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK`), since any application (not just voice) might request any kind of audio focus. In short, all audio focus use cases should be handled appropriately. See [Handling Audio Focus with Voice Search][managing-audio-focus#audiofocusvoice] for more details.

Q:  What does the Menu button do, and can I override it in my app?
:   A: By default, the Menu button invokes the Android context menu, which appears as a list of menu items centered on the screen. You can override the menu button to provide your own custom menu user interface, or for any other purpose.

    If you only have one menu item, consider using the Menu button as a simple toggle (for example, to turn closed captions on or off), and provide an onscreen hint to expose that feature to your users.

Q:  Why doesn't the GameCircle overlay for my game appear when I press the GameCircle button on the Fire Game Controller?
:   A: The GameCircle overlay appears only if:

    * Your game has implemented the GameCircle API.
    * Your game has been submitted to the Amazon Appstore, and is categorized as a game.
    * Your game has been installed on to a Fire TV device from the Amazon Appstore.


    The GameCircle overlay does not appear if you have sideloaded your game onto a Fire TV device for testing. You can test your GameCircle implementation in your app before submitting it with [Live App Testing](https://developer.amazon.com/public/resources/development-tools/live-app-testing).

    The GameCircle overlay may also not appear if you have not yet published any leaderboards or achievements AND you're not using a test account. In this scenario either publish at least one achievement or leaderboard in the Amazon Developer Console, or set up a test account (also in the Developer Console), and use that account to view draft achievements and leaderboards in the GameCircle overlay.

Q:  Why is my activity restarted from scratch when a controller is connected, disconnected or sleeps?
:   A: These events are handled as runtime configuration changes by Android. To ignore these events in your app, modify your `AndroidManifest.xml` to include the `android:configChanges` attribute, and include keys for `keyboard`, `keyboardHidden`, and `navigation`:

    ```java
    <activity android:name="MyActivity"
    android:configChanges="keyboard|keyboardHidden|navigation">
    ```

    See the Android Guide [Handling Runtime Changes](http://developer.android.com/guide/topics/resources/runtime-changes.html) for information on the `configChanges` attribute and how to handle configuration changes in your app, if necessary.

Q:  How do I handle game controller disconnects in my app or game?
:   A: The Amazon Fire TV Game Controller may disconnect from the system if it is idle or if either stick is held at an angle for more than five minutes (to preserve battery life). Other controllers may also disconnect when idle or when battery life runs out. Use the Android [`OnInputDeviceRemoved`](http://developer.android.com/reference/android/hardware/input/InputManager.InputDeviceListener.html#onInputDeviceRemoved(int)) listener to handle controller disconnection events. Consider pausing the game or displaying a dialog to let the user know the controller is no longer available.

## Amazon Fire TV Stick {#amazonfiretvstick}

Q:  What are the differences between Amazon Fire TV and Fire TV Stick?
:   A: [Fire TV Device Specifications][device-and-platform-specifications] lists the specifications for all Fire TV devices.


Q:  How do I adapt my Amazon Fire TV app to Amazon Fire TV Stick?
:   A: Both Amazon Fire TV and Fire TV Stick run the same platform software. However, because of the more limited hardware on the Fire TV Stick, optimizing your app for performance and stability are critical. Make sure you follow Android best practices for hardware acceleration and [performance](http://developer.android.com/training/best-performance.html). Watch out in particular for OpenGL and textures; the Fire TV Stick's GPU supports OpenGL 2.0 with a `MAX_TEXTURE_SIZE` of 2048x2048.

Q:  How can I identify a Fire TV Stick device?
:   A: You can check for the feature `amazon.hardware.fire_tv`. If you absolutely must look for the Fire TV Stick, you can check for the MODEL `AFTM`. See [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices] for more information.

Q:  Some of the images/backgrounds in my app are not appearing, or I'm getting grey boxes for those images.
:   A: This is usually a result of bitmap images or textures that are too large. Fire TV Stick supports textures up to 2048x2048. You may see an error in the logs like this if your app has this problem:

    ```
    W/OpenGLRenderer( 8941): Bitmap too large to be uploaded in a texture (3840x2160, max=2048x2048)
    ```

    Also, make sure your images for Fire TV are in the `drawables-xhdpi/` folder and not in `drawables/`. Platform scaling of the default drawables may result in large images that exceed the texture limit.


[release-media-player]: https://developer.android.com/guide/topics/media/mediaplayer.html#releaseplayer
[mediaplayer-android]: https://developer.android.com/reference/android/media/MediaPlayer.html#release()

{% include links.html %}
