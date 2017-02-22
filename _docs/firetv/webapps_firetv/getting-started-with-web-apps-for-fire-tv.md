---
title: Getting Started with Web Apps for Fire TV
permalink: getting-started-with-web-apps-for-fire-tv.html
sidebar: firetv
product: Fire TV
github: true
toc: false
---

Amazon Fire TV, including both Fire TV and the Fire TV Stick, supports HTML5 web apps. You can port your web app to a new platform and submit it to the Amazon Appstore with minimal effort.

* TOC
{:toc}

## Options for Submitting Apps

You have three ways of submitting your web app to the Amazon Appstore:

*  **Hosted app**: Customers run a wrapper that opens a web view to your URL.
*  **Packaged app**: Customers run the same wrapper that loads your HTML/JS/CSS/assets from local copies, submitted as a ZIP archive. 
*  **Android app**: You build your own wrapper with Cordova or create your own hybrid app and submit it like any other native app. For more information on Cordova, see [Apache Cordova](http://cordova.apache.org/). For information on using Cordova with Amazon WebView, see the Apache Cordova API topic, [Amazon Fire OS Platform Guide](http://cordova.apache.org/docs/en/3.4.0/guide_platforms_amazonfireos_index.md.html).

For a comparison of hosted apps and packaged apps, see [Differences between Packaged and Hosted Apps](https://developer.amazon.com/public/solutions/platforms/webapps/docs/differences-between-packaged-and-hosted-apps). For information on submitting both hosted and packaged apps, see [Submitting or Updating Your Web App to the Amazon Appstore](https://developer.amazon.com/public/support/submitting-your-app).

The Amazon WebView API also enables creating hybrid HTML5 apps. For more information on the Amazon WebView API, see [Building and Testing Your HTML5 Hybrid App](https://developer.amazon.com/public/solutions/platforms/android-fireos/docs/building-and-testing-your-hybrid-app).

Be aware of the issues presented in detail below as you develop your web app for Fire TV.

For more information, also see the following topics:

*   [Supporting Controllers in Web Apps][supporting-controllers-in-web-apps]
*   [The Web App Starter Kit for Fire TV][the-web-app-starter-kit-for-fire-tv]
*   [Customizing Your Fire TV Web App][customizing-your-fire-tv-web-app]
*   [Migrating Your Web App to Fire TV ][migrating-your-web-app-to-fire-tv]
*   [The Fire TV Web Apps FAQ][fire-tv-web-app-faq]

## Including the Amazon APIs

If your web app uses Amazon APIs, such as the [In-App Purchasing API](https://developer.amazon.com/public/apis/earn/in-app-purchasing), you need to include the Amazon API JavaScript library. This library initializes any Amazon plugins and raises an `amazonPlatformReady` event to to signal that the APIs are ready for use. The library is hosted at the URL `http://resources.amazonwebapps.com/v1/latest/Amazon-Web-App-API.min.js`. To include this library, add the following `<script>` tag to your web app:

```html
<script src="https://resources.amazonwebapps.com/v1/latest/Amazon-Web-App-API.min.js"></script>
```

{% include note.html content="Including the Amazon API JavaScript library is necessary only if your app uses Amazon APIs." %}

Because this library raises an `amazonPlatformReady` event when initialization is complete, wait for the `amazonPlatformReady` event before you call any Amazon APIs, as in the following example:

```java
document.addEventListener('amazonPlatformReady', function () {
    //API code goes here
});
```

## Using Web App Tester and DevTools

[Web App Tester](https://developer.amazon.com/public/solutions/platforms/webapps/docs/tester.html) is a tool that lets you test your hosted and packed apps on the Fire TV device.

Web App Tester also enables using DevTools to debug your web app. For information on getting and using DevTools, see [Debugging Your Web App](https://developer.amazon.com/public/solutions/platforms/webapps/docs/debugging-webapp.html).

To enable DevTools on Fire TV:

1.  In the Web App Tester tool, launch your web app.
2.  On the Amazon Fire TV remote, press the **Menu** button.
3.  Select the **Enable Devtools** menu item and follow the instructions in the dialog box that appears.

## Resolution and Page Scaling {#resolution_scaling}

Target a resolution of **1080p** (1920x1080) for web apps for Fire TV. For information on scaling up your app from 720p to 1080p, see [Customizing Your Fire TV Web App][customizing-your-fire-tv-web-app]. For more information about design guidelines for Fire TV web apps, see [Design and User Experience Guidelines][design-and-user-experience-guidelines]. 

## Web Audio API Support {#web_audio_support}

Fire TV and Fire TV Stick both support the [Web Audio Api](http://webaudio.github.io/web-audio-api/) for web apps. They also support the `suspend` and `resume` methods of the [`AudioContext`](http://webaudio.github.io/web-audio-api/#the-audiocontext-interface) interface. These methods allow applications to pause the audio device when needed. Suspending audio playback reduces CPU usage and prolongs battery life.

 The following example illustrates creating an `AudioContext` object and calling the `suspend` and `resume` methods. 

```java
// Create context
var context = new (window.AudioContext || window.webkitAudioContext)();

// Start context
var oscillator = context.createOscillator(); oscillator.connect(context.destination); oscillator[oscillator.start ? 'start' : 'noteOn'](0);

// Suspend
context.suspend();
// Resume
context.resume();

// Since both suspend and resume return a promise, their success/failure can be determined
context.suspend().then( function() { alert('success!'; }, function() { alert('failure!'; });
context.resume().then( function() { alert('success!'; }, function() { alert('failure!'; });
```

Coupled with the page visibility API, `suspend` and `resume` provides an ideal way to silence games and other applications that use Web Audio when they go into the background and when the user initiate a voice search. For more information and code examples, see the following [Focus Changes](#focus_changes) and [Voice Search Interruption](#voice_search_interruption) sections. 

## Focus Changes {#focus_changes}

A web app may need to track when it is moved to the background (as when the user presses the **Home** button) so that the app can preserve its state at the moment when it is moved to the background and can resume when it is brought to the foreground. The following table shows the different methods and their behavior.

| Method | Description |
|-------|---------|
| Custom Amazon events: `pause` and `resume` | `pause` event is fired when the app is sent to the background (app completely hidden) or when its partially obscured (for example, when the **Voice Search** dialog box is displayed)<br/><br/>`resume` event is fired when the app is brought to the foreground (app completely visible) including when the **Voice Search** dialog box is dismissed. 
| Page Visibility API |  `webkitvisibilitychange` event is fired: <br/><br/> &bull; when the app is sent to the background (completely hidden) <br/><br/><br/><br/> &bull; when the app is brought to the foreground (completely visible) 

### Custom Amazon Events

To use the custom Amazon events, you need to include the Amazon Webapp API script in your app and have your app listen for `pause` and `resume` events fired on the document.  

To register as a listener for the `pause` event, use the following syntax.

```java
document.addEventListener(“pause", yourCallbackFunction, false);
```

The `pause` event is fired when the app is sent to the background (app completely hidden) or when it's partially obscured (e.x Voice Search Dialog is displayed). The app should use `document.addEventListener` to attach an event listener once the `amazonPlatformReady` event fires.

To register as a listener for the resume event, use the following syntax.

```java
document.addEventListener(“resume", yourCallbackFunction, false);
```

The `resume` event is fired when the app is brought to the foreground (app completely visible) including when the Voice Search Dialog is dismissed.  The app should use `document.addEventListener` to attach an event listener once the `amazonPlatformReady` event fires.

The following code snippet illustrates registering for the `pause` and `resume` events.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Page Visibility Test Page</title>
</head>
<body>
<h2> Visibility : </h2>
<div id="appstate"> Visible </div>
<script src="https://resources.amazonwebapps.com/v1/latest/Amazon-Web-App-API.min.js"></script>
<script>
    var visibility =  document.getElementById("appstate");

    function onPause() {
        visibility.innerHTML = 'Hidden';
    }

    function onResume() {
        visibility.innerHTML = 'Visible';
    }

    function onAmazonPlatformReady() {
        document.addEventListener("pause" , onPause, false);
        document.addEventListener("resume" , onResume, false);
    }

    document.addEventListener("amazonPlatformReady" , onAmazonPlatformReady, false);
</script>
</body>
</html>
```

### The Page Visibility API

For all types of apps, the web app platform fires a visibility changed event, using the [Page Visibility API](http://www.w3.org/TR/page-visibility/), when the app loses or gains focus. (For more information on detecting loss of focus, see the [FAQ Web Apps for Fire TV][fire-tv-web-app-faq].)

For video-playback apps, the video is paused automatically when the app loses focus. However, you must ensure that your UI is in the proper state when the app regains focus. For example, it is a good idea to bring up the media controls so the user can see the state of the playback, and to ensure the app responds to **Play/Pause** button. Alternatively, you may want to resume the video automatically.

However, for audio apps, the app itself must pause the audio playback. This is especially important because presses on the media buttons are not passed to an app when it is in the background, so the user has no way to pause audio playback. 

The following code shows how to detect changes in focus. Note that these events use the "webkit" prefix. 

```java
var handleVisibilityChange = function() {
    if (document.hidden || document.webkitHidden) {
        // pause audio/video playback, pause game, adjust video control UI, etc.
    } else {
        // resume playback, adjust UI, etc.
    }
}

document.addEventListener('webkitvisibilitychange', handleVisibilityChange);
document.addEventListener('visibilitychange', handleVisibilityChange);
```

If you are using Web Audio, you can use the Page Visibility API to detect changes in focus and use the `suspend` and `resume` methods of the `AudioContext` object to pause and resume audio playback, as illustrated in the following example. (For more information on Web Audio, see the preceding [Web Audio Support](#web_audio_support) section.)

```java
// Create context
var context = new (window.AudioContext || window.webkitAudioContext)();

// Start context
var oscillator = context.createOscillator(); oscillator.connect(context.destination); oscillator[oscillator.start ? 'start' : 'noteOn'](0);

// Determine the correct visibility api names.
var hidden, visibilityChange;
if (typeof document.hidden !== "undefined") { // Opera 12.10 and Firefox 18 and later support
        hidden = "hidden";
        visibilityChange = "visibilitychange"; }
else if (typeof document.webkitHidden !== "undefined") {
        hidden = "webkitHidden";
        visibilityChange = "webkitvisibilitychange"; }

// Page visibility listener.
function handleVisibilityChange() {
   if (document[hidden]) {
     if (context.suspend) {
       context.suspend();
     } else {
       // Browser does not support suspend()
     }
   } else {
     if (context.resume) {
        context.resume();
     } else {
          // Browser does not support resume()
     }
   }
}
document.addEventListener(visibilityChange, handleVisibilityChange, false);
```

## Voice Search Interruption {#voice_search_interruption}

A requirement for applications submitted for Fire TV is that they stop or pause audio playback when the user presses the **Voice Search** button on the remote.  This is to ensure that the microphone on the remote does not pick up audio from the system and perform poorly. Your app can detect when the user presses the **Voice Search** button in two ways:

*  By responding to the custom Amazon `pause` and `resume` events
*  By using the Page Visibility API, as explained in the previous section (for platform version 1.3 only)

For more information and codes examples, see the preceding [Focus Changes](#focus_changes) section.

For video-based media applications, the web app platform will automatically handle pausing the video. You only need to ensure that your UI is in the proper state when your app regains focus. For example, ensure that the **Play/Pause** button is correctly set, or you may decide to bring up the video control overlay.

For games, you must also ensure that audio playback is muted or paused. A common solution in this situation is to put the game in a paused state.

Audio-only apps must also stop or pause playback. When you use Web Audio, you can use the `suspend` and `resume` methods of the [`AudioContext`](http://webaudio.github.io/web-audio-api/#the-audiocontext-interface "The audiocontext interface") object to properly handle voice search requests when using WebAudio.  (For more information on Web Audio, see the preceding [Web Audio Support](#web_audio_support) section.)

```html
<script src="https://resources.amazonwebapps.com/v1/latest/Amazon-Web-App-API.min.js"></script>

<script>
var context = new (window.AudioContext || window.webkitAudioContext)();

function onPause() {
    context.suspend();
}

function onResume() {
    context.resume();
}

function onAmazonPlatformReady() {
    document.addEventListener("pause" , onPause, false);
    document.addEventListener("resume" , onResume, false); }

document.addEventListener("amazonPlatformReady" , onAmazonPlatformReady, false);
</script>
```

## Testing IAP

If you use the [IAP](https://developer.amazon.com/public/apis/earn/in-app-purchasing) API 1.0 in your web app (IAP 2.0 does not currently support web apps), you can test IAP by using the SDK tester. For more information on testing IAP see [Testing In-App Purchasing](https://developer.amazon.com/public/solutions/platforms/webapps/docs/testing-iap.html "Web App Tester").

To use SDK Tester: 

1.  Download the Amazon Web App SDK from [Amazon Apps and Games Services SDKs](https://developer.amazon.com/public/resources/development-tools/sdk).
2.  Install the SDK tester apk in `App-SDk.zip/Android/In-App-Purchasing/1.0/tools/AmazonSDKTester.apk.`
3.  Create a JSON file with the following contents. (This example was developed for the YouZeek music-streaming app.) 

    ```json
    {
        "vip_unlimited" : {
            "itemType" : "ENTITLED",
            "price" : 0.99,
            "title": "VIP Subscription Parent",
            "description": "VIP Subscription Parent",
            "smallIconUrl": "Any Image Link Here"
        },
        "vip_unlimited_monthly" : {
            "itemType" : "SUBSCRIPTION",
            "price" : 0.99,
            "title": "VIP Monthly Subscription",
            "description": "VIP Monthly Subscription",
            "smallIconUrl": "Any Image Here",
            "subscriptionParent": "vip_unlimited"
        }
    }
    ```

4.  Run the following command:

    ```bash
    adb push amazon.sdktester.json /mnt/sdcard/amazon.sdktester.json
    ```

    or copy the JSON file into the root folder of the device as `amazon.sdktester.json.` 

5.  Launch web app using the Web App Tester.

## Features Not Available on Fire TV

The following web application platform features are not available on Fire TV:

*   Geolocation (GPS)
*   Popup windows
*   Multiline text boxes
*   Launching an app to an external browser
*   Uploading files  
*   Downloading files 

WebGL is supported on Fire TV, but not supported on the Fire TV stick.

{% include links.html %}
