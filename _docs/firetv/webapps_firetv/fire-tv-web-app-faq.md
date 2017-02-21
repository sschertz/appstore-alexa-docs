---
title: Fire TV Web App FAQ
permalink: fire-tv-web-app-faq.html
toc: false
sidebar: firetv
product: Fire TV 
github: true
---

The following are frequently asked questions about Fire TV Web Apps.

{: #customize_back}
Q: How can I customize the Amazon Remote Back button behavior for a web app?
:   The Back button does not raise a standard key event. The default behavior for this button is to navigate back in the history stack until the WebView is at the root page, and once there a subsequent back press closes the application without informing the web app. As a workaround, you can use the History API to capture Back button presses by pushing to the history stack in the onLoad event.

    Note that there is a bug in Chromium v25, which fires the onpopstate event on page load. This adds complexity to the following workarounds. For information about this bug, see the [Chromium documentation for Issue 63040: Add window.history.state](https://code.google.com/p/chromium/issues/detail?id=63040) and don't fire popstate after load.

    There are two possible workarounds using the History API. Only one is needed. The following example is the preferred workaround. It uses the state to ensure that it acts on the correct popstate event.

    ```java
    window.addEventListener("load", function () {
        window.addEventListener("popstate", function () {
            if (window.history.state !== "backhandler") {
                // put your back handler code here
                window.history.pushState("backhandler", null, null);
            }
        });
        window.history.pushState("backhandler", null, null);
    });
    ```

    The second, non-preferred workaround sets a timeout to delay registering the onpopstate event until right after the load. This works, but relies on timers and therefore is not the primary recommendation.

    ```java
        window.addEventListener("load", function() {
        setTimeout(function() {
            window.onpopstate = function(event) {
                showConfirmDialog();
                window.history.pushState(null, null, null);
            };
        }, 0);
        window.history.pushState(null, null, null);
    });
    ```

{: #close_webapp}
Q: How do I properly close a web app?
:   Many apps need to explicitly close or end the application.  For example, they may override the Back button behavior to show a confirmation dialog prior to closing the app, and want to ensure the window is properly closed when selected by the user.  The snippet below provides a method of doing so.

    ```
    window.open("", "_self").close();
    ```

Q: Can web apps for Fire TV support digital rights management (DRM) for media playback?
:   No, the Web App Platform does not currently support any form of DRM for media playback.  For details about DRM support for native FireTV applications, see the [DRM][device-and-platform-specifications] section of the Specifications for Fire TV Devices page.

Q: Does my web app need to disable the screen saver during video playback?
:   Yes, your app should disable the screen saver on the Fire TV during video playback to allow for long running times without user interaction. When you submit your app, select the **Prevent Sleep for Video Playback** check box.

    Otherwise, the screen saver appears after a period of inactivity, just as it would during navigation through the main menu.

Q: Can web apps for Fire TV use HTTP live streaming (HLS)?
:   Yes. [HLS](http://tools.ietf.org/html/draft-pantos-http-live-streaming) video playback is supported in the Web App Platform. However, be aware of the following known issues:

    *  Videos do not pause automatically if put in background while loading or buffering. As a workaround, use the Page Visibility API to manually pause videos. For information on using the Page Visibility API to detect focus changes, see [Getting Started with Web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv].
    *  Short video segments (~1-2 sec) may cause pauses and artifacts during playback.
    *  Seeking in an HLS stream is not supported.
    *  Getting the duration of an HLS video is not supported.
    *  When the user performs a voice search during HLS playback the video is paused, and when the playback resumes, the video starts from the beginning.
    *  An alternative to HLS that does not have these issues is VisualOn. Be aware though that VisualOn runs only if full-screen mode, and so you cannot use overlays or custom controls.

Q:  Can web apps for Fire TV use the Blob Interface API?
:   No. The [Blob APIs](http://www.w3.org/TR/FileAPI/#dfn-Blob) are currently unsupported in the Web App Platform.  Note that the the Blob object is incorrectly present in the namespace which may cause some feature detection libraries to wrongly identify the platform as being Blob-capable.

{: #unchanging_duration}
Q: Why does my web app show an unchanging duration of 1:40 seconds during audio and video playback? 
:   For both audio and video elements, the duration property incorrectly reports a time of 100 seconds (1:40) during the initialization period.  If you set your text field at this time and do not update it, it will improperly show 1:40.  The correct value is in fact made available once the media element has begun playback.  The following code example shows a workaround of using the `durationchange` event to update the element with the correct duration during initialization.


    ```java
    video.addEventListener("durationchange", function() {
        // when this event fires, the media duration should be available
        document.getElementById("duration").innerHTML = video.duration;
    });
    ```

Q: How do I allow users to control video playback using the remote?
:   Some HTML5 elements cannot be focused with the remote. For example, div and span elements cannot be focused with the remote. Also, if you rely on default video controls, the individual **Play**, **Pause**, **Forward**, and **Rewind** buttons cannot be focused. However, the video element as a whole can be focused and can be controlled with the remote keys.

Q: Can I launch the virtual keyboard and tell when text has been submitted?
:   The virtual keyboard appears when the user presses the **Select** button on an text input field. This keyboard can be used by either the Amazon Remote or the Amazon Game Controller. Currently, there is no way to launch the virtual keyboard programmatically (for example, by setting focus).

    If you want to know when the user has submitted text, you can listen for the change event, as demonstrated in the following code. Note that this event does not fire if the user clicks the **Back** button rather than the **Submit** button.

    ```java
    <input id="test" value="">

    var testInput = document.getElementById("test");
    testInput.addEventListener("change", function(e){
        console.log(this.value);
    });
    ```

Q: Why does my app show unexpected behavior with third-party authentication pages?
:   If you redirect to third-party authentication page within the same WebView, rather than using `window.open()` to create a new WebView, the authentication page replaces the original login page in history. This can cause an issue in scenarios with multiple login options (for example, Google or Facebook). The problem is that users cannot go back if they change their mind about logging in. The authentication page that replaced the app will typically just close.

    The following actions cause such navigation problems:

    *  Redirecting the window location -> `window.location.href = "google_authentication_link"`
    *  Using the `window.location.replace()` function

    Instead, use the `window.open()` function.

    {% include note.html content="Using `window.location.assign()` does not work in the current WebView." %}

Q: Can my web app support MOV Files?
:   No, currently Amazon WebView has no support for MOV files.

Q: Can I use CSS viewport units?
:   You can use the `vw/vh/vmin/vmax` units except in conjunction with the CSS `translate()` function. In the current release of AWV (v25), you cannot translate an absolutely positioned element using CSS viewport units. This is a known issue in v25 of Chromium. See [Issue 137617: vh, vw units don't work in css transforms](https://code.google.com/p/chromium/issues/detail?id=137617) in the Chromium documentation. As a workaround, you can use the following calculation in v25 to get the equivalent viewport unit value in pixels for the unsupported CSS properties.

    ```
    1vw = ( window.innerWidth/100 )
    1vh = ( window.innerHeight/100 )

    M vw = ( window.innerWidth/100 ) * M
    N vh = ( window.innerHeight/100 ) * N
    ```

Q: How can I simulate a 720p environment for my web app?
:   To simulate a 720p environment, put the following meta tag in the header of your web app page.  Please note that the viewport meta tag is technically unsupported for initial release and scales pages up on 1080p displays (potentially causing page distortion). This makes the following snippet more of a workaround than a solution.

    ```xml
    <meta name=”viewport” content=”initial-scale=1.5, user-scalable=no”>
    ```

    This meta tag sets a viewport, representing the area the web app page occupies, and sets a zoom level of 150%. The result is that the web app content looks like it would on a 720p display.

    For more information, see the "Resolution and Page Scaling" section on [Getting Started with Web Apps for Fire TV] [getting-started-with-web-apps-for-fire-tv].

Q: Can I use VisualOn for media playback?
:   You cannot force Fire TV or Fire TV Stick to use VisualOn for media playback. The platform automatically chooses the best player to handle the video type.

    {% include note.html content="The `amazon_enhanced_hls` video attribute flag was introduced on 1.4.1 (Fire TV) and 1.0.1 (Fire TV stick) for applications to route HLS playback to the VisualOn player. The downside of this attribute was that playback would take place in full-screen with built-in controls and the web developer did not have an option to override the controls." %}

    As of Fire TV 1.5 and Fire TV Stick 1.1, these platforms no longer support the `amazon_enhanced_hls` attribute because the platform automatically chooses the best player (native or VisualOn) depending on the video type. Pages that were authored to have the `amazon_enhanced_hls` attribute will continue to load on a suitable player; however, video playback will not automatically go to full screen. The page must request to go full screen in JavaScript as needed.

Q: How can I improve scaling performance?
:   When a video element gets focus, its size is often scaled up by setting the CSS property "height" and "width". This practice causes full background repaint during navigation. Use `-webkit-transform: scale()` instead for image resizing to avoid unnecessary repaints. For example, the following CSS code causes unnecessary repaints:

    ```java
    .focused .video-element-thumb {
        width: 288px;
        height: 162px;
        //...
    }
    ```

    The following CSS code avoids unnecessary repaints:

    ```java
    .focused .video-element-thumb {
        -webkit-transform: scale(1.125, 1.125);
        //...
    }
    ```

Q: Is Web Audio supported in web apps for Fire TV
:  Yes. Starting with  Fire TV 1.5 and Fire TV Stick 1.1, Fire TV supports Web Audio. For more information, See [Getting Started with Web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv].

Q: If I use the img tag for buttons, do I have to include alt text?
:   Yes. Applications using img tag as buttons (with onClick handlers) must have the “alt” attribute to have those buttons read out in accessibility mode. Otherwise, the buttons are not accessible to users in accessibility mode.

Q: Are Known Media Source Extensions (MSE) supported in web apps for Fire TV
:   No. MSE support is disabled in Web Apps for Fire TV (though not for other platforms). Because MPEG-DASH is built on MSE, MPEG-DASH is not supported as well. Without the adaptive streaming support offered by MSE, it is possible that the end user devices might take longer to recover from network congestion and that seeks may take longer. Note, however, that Web Apps on Fire TV platform does support the other adaptive streaming standard, HLS.

{% include links.html %}
