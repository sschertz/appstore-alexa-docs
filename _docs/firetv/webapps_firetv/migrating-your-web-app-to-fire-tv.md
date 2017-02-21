---
title: Migrate Your Web App to Fire TV
permalink: migrating-your-web-app-to-fire-tv.html
sidebar: firetv
product: Fire TV 
---

Amazon Fire TV supports HTML5 web apps. If you have an existing web app that you want to make available on Fire TV, review the following checklist for migrating your web app to the Fire TV platform.

If you are developing a new web app instead of migrating an existing app, see [Getting Started with Web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv].

Before you begin, review the [Design and User Experience Guidelines][design-and-user-experience-guidelines] for details about designing your app for the 10-foot User Interface experience.

## Web App Migration Checklist

<table class="grid">
  <thead>
    <tr>
      <th>If your web app…</th>
      <th>You need to…</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Plays video</td>
      <td markdown="span">Make sure your UI is the proper state when your app regains focus. Note that the video playback is automatically paused when your app loses focus. <br /><br /> You can use the <a href="http://www.w3.org/TR/page-visibility/">Page Visibility API</a> to detect focus changes and respond appropriately. See "Focus Changes" in [Getting Started with web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv]. In addition, be sure to select the <b>Prevent Sleep for Video Playback</b> check box when submitting your app. This disables the Fire TV screensaver while your app plays videos.</td>
    </tr>
    <tr>
      <td>Plays audio</td>
      <td markdown="span">Pause the audio playback when your app loses focus. The audio is not automatically paused, and the user cannot pause the audio with the remote once the app is in the background. <br /><br />You can use the <a href="http://www.w3.org/TR/page-visibility/" title="Page Visibility API">Page Visibility API</a> to detect focus changes and respond appropriately. See "Focus Changes" in [Getting Started with web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv] for more details.</td>
    </tr>
    <tr>
      <td>Shows audio or video duration during playback</td>
      <td markdown="span">Listen for the <code>durationchange</code> events to show the appropriate duration. Note that the duration property reports an incorrect duration during initialization, so you need to update the duration once playback begins. See "Why does my web app show an unchanging duration of 1:40 seconds during audio and video playback?" in the [Fire TV Web App FAQ][fire-tv-web-app-faq] in the FAQ.</td>
    </tr>
    <tr>
      <td>Uses custom playback controls</td>
      <td markdown="span">Capture the key presses to use input from the Amazon Fire TV Remote and Amazon Fire TV game controller. See [Supporting Controllers in Web Apps][supporting-controllers-in-web-apps].</td>
    </tr>
    <tr>
      <td>Has an exit button</td>
      <td markdown="span">Properly close your web app. See "How do I properly close a web app?" in the [Fire TV Web App FAQ][fire-tv-web-app-faq].</td>
    </tr>
    <tr>
      <td>Is a single-page application</td>
      <td markdown="span">Use the W3C history to move through the content and respond to the back button correctly. See "How can I customize the Amazon Remote Back button behavior for a web app?" in the [Fire TV Web App FAQ][fire-tv-web-app-faq].</td>
    </tr>
    <tr>
      <td>Uses the viewport API to control scaling</td>
      <td markdown="span">Make sure your app targets 1080p. See "Resolution and Page Scaling" in [Getting Started with web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv][getting-started-with-web-apps-for-fire-tv] in Getting Started.</td>
    </tr>
    <tr>
      <td>Explicitly targets 720p resolution</td>
      <td markdown="span">Change your app to target 1080p. If the device is set to 720p, your app is automatically scaled down. See "Resolution and Page Scaling" in [Getting Started with web Apps for Fire TV][getting-started-with-web-apps-for-fire-tv][getting-started-with-web-apps-for-fire-tv] in Getting Started.</td>
    </tr>
    <tr>
      <td>Relies on touch or click events for navigating between app components</td>
      <td markdown="span">Review your UI and make sure it will work with remote and game controller input instead of touch and click input. All selectable UI elements should be reachable using the up, down, left, and right navigation buttons available on remotes and game controllers. See [Design and User Experience Guidelines][design-and-user-experience-guidelines] and [Supporting Controllers in Web Apps][supporting-controllers-in-web-apps].</td>
    </tr>
    <tr>
      <td>Is a game</td>
      <td markdown="span">Review the information about supporting game controllers and handling focus changes. See "Using Input from the Amazon Fire Game Controller" in [Supporting Controllers in Web Apps][supporting-controllers-in-web-apps] and "Focus Changes" in [Getting Started][getting-started-with-web-apps-for-fire-tv].</td>
    </tr>
  </tbody>
</table>

{% include links.html %}
