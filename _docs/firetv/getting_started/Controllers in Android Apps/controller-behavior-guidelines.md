---
title: Controller Behavior Guidelines
permalink: controller-behavior-guidelines.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

When you develop your app for the Amazon Fire TV platform, you can support input from different kinds of controllers. These controllers include the Amazon Fire TV Remote and Voice Remote, the Amazon Fire TV Game Controller, or any other controllers that support the Bluetooth gamepad HID profile.


Use the motion and input events from the [Remote Input][amazon-fire-tv-remote-input] and [Game Controller Input][amazon-fire-game-controller-input] to implement controller input for your app.

This page provides recommendations for common app functionality across different controllers. None of these guidelines are requirements for publishing an app for Amazon Fire TV, and you may design controller input in the best way that works for your app. We suggest you follow these guidelines to enable a consistent user experience across different controllers, apps, and games.

{% include note.html content="With the exception of the Microphone button, the behavior of all Fire TV remote controls are identical. All of the guidelines in this document that refer to the Fire TV Remote also apply to the Fire TV Voice Remote." %}

* TOC
{:toc}

## Core Behavior

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="40%" />
</colgroup>
  <thead>
    <tr>
      <th>Action</th>
      <th>Amazon Fire TV Remote Button</th>
      <th>Amazon Fire TV Game Controller Button</th>
      <th>Other Game Controller Button</th>
      <th>Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>Home</td>
      <td>Home</td>
      <td>Home (if available)</td>
      <td>This is a system event and cannot be captured in your app. When pressed, the system returns the user to Home. For apps categorized as games in the Amazon Appstore, the GameCircle overlay or “Game Paused” screens appear. A second Home press returns the user to Home.<br/><br/> Implement <a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"><code>onPause()</code></a> to preserve state in your app or game in case of an unanticipated Home. Implement <a href="http://developer.android.com/reference/android/app/Activity.html#onResume()"><code>onResume()</code></a> to continue when the app resumes. <br/><br/>Audio apps may continue playing in the background by requesting the <a href="http://developer.android.com/training/managing-audio/audio-focus.html">audio focus</a>.</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>Back</td>
      <td>Back</td>
      <td>B</td>
      <td>Return to the previous operation or screen (Activity), or cancel the current operation or prompt. <br/><br/>Capture this event to provide confirmation dialogs. For example, on the main screen of your app, capture Back to provide a “Do you want to Quit” dialog.</td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>Menu</td>
      <td>Menu</td>
      <td>Y</td>
      <td>Invoke the standard Android context menu (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>). <br/><br/>Capture this event to provide your own menu, or for any other purpose. If your menu only has one option, you can use Menu as a toggle for that option. <br/><br/>In media apps, use Menu to show or hide the playback chrome.</td>
    </tr>
    <tr>
      <td>Search</td>
      <td>Microphone (Voice Remote only)</td>
      <td>N/A</td>
      <td>N/A</td>
      <td>This is a system event and cannot be captured in your app. When pressed, voice search is invoked. <br/><br/>Implement <a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"><code>onPause()</code></a> to preserve state in your app or game when voice search launches, and <a href="http://developer.android.com/reference/android/app/Activity.html#onResume()"><code>onResume()</code></a> to continue after it is complete. <br/><br/>In audio apps, pause playback or lower the volume when voice search is active. <br/><br/>In video apps, mute the audio or pause playback when voice search is active.</td>
    </tr>
    <tr>
      <td>GameCircle</td>
      <td>N/A</td>
      <td>GameCircle</td>
      <td>N/A</td>
      <td>This is a system event and cannot be captured in your app. When pressed, the system displays the GameCircle overlay (for games with GameCircle support), a “Game Paused” dialog (for games without GameCircle support), or returns to the Games page in the launcher (for all other apps). <br/><br/> <strong>Note:</strong> Sideloaded apps always return to the Games page of the Launcher even if they implement GameCircle. Only apps that have been submitted to the Amazon Appstore demonstrate correct GameCircle behavior. You can use <a href="https://developer.amazon.com/public/resources/development-tools/live-app-testing">Live App Testing</a> to test your GameCircle integration before you submit your app. <br/><br/> Implement <a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"> <code>onPause()</code></a> to preserve state in your app or game when the GameCircle button is pressed. Implement <a href="http://developer.android.com/reference/android/app/Activity.html#onResume()">onResume()</a> to continue when the user returns to your game.</td>
    </tr>
  </tbody>
</table>

## Navigation and Selection

The following table describes the recommended behavior for user interface navigation and selection. For items that show multiple buttons, provide support for both those buttons.

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="40%" />
</colgroup>
  <thead>
    <tr>
      <th>Action</th>
      <th>Amazon Fire TV Remote Button</th>
      <th>Amazon Fire TV Game Controller Button</th>
      <th>Other Game Controller Button</th>
      <th>Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Select/Main Action</td>
      <td>D-Pad Center</td>
      <td>A</td>
      <td>A</td>
      <td>Select the item in focus, confirm menu options or prompts, or perform the main game action.</td>
    </tr>
    <tr>
      <td>Cancel/Back</td>
      <td>Back</td>
      <td>Back <br/>B</td>
      <td>B</td>
      <td>Cancel the current operation, or return to the previous screen. <br/><br/>Intercept this action to provide confirmation dialogs (“Are you sure you want to quit?”)</td>
    </tr>
    <tr>
      <td>Up<br/>Down<br/>Left<br/>Right</td>
      <td>D-Pad</td>
      <td>D-Pad<br/>Left Stick</td>
      <td>D-Pad<br/>Left Stick</td>
      <td>Move the input focus in the appropriate direction. <br/> <br/>On the Amazon Fire Game Controller and other game controllers, the left stick should have the same behavior as the D-Pad.</td>
    </tr>
</tbody>
</table>


## Media Playback

The following table describes the recommended behavior for media playback. Note the following:

* If your app does not play media or use the analog sticks or shoulder buttons (L1/R1), do not capture the events for those buttons. Doing so may interfere with the system's ability to control media playing in the background.
* If your app uses a framework such as Unity, you can ignore this recommendation, since the ability to pass key events through to the system is not supported in those frameworks.
* If your app or game does use any of these buttons for other purposes, the user may access system media control from the GameCircle overlay (GameCircle button) or in the Fire TV launcher.

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="25%" />
</colgroup>
  <thead>
    <tr>
      <th>Action</th>
      <th>Amazon Fire TV Remote Button</th>
      <th>Amazon Fire TV Game Controller Button</th>
      <th>Amazon Fire Game Controller (1st Generation) Button</th>
      <th>Other Game Controller Button</th>
      <th>Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Play/Pause</td>
      <td>Play/Pause</td>
      <td>A<br/>Left/Right Stick Press</td>
      <td>Play/Pause</td>
      <td>A</td>
      <td>Toggle media play or pause.</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>Rewind<br/>Left (D-Pad)<br/>Left Shoulder (L1)</td>
      <td>Left Shoulder (L1)</td>
      <td>Rewind<br/>Left (D-Pad)<br/>Left Shoulder (L1)</td>
      <td>Left (D-Pad) <br/>Left Shoulder (L1)</td>
      <td>Rewind or skip backwards in media playback contexts. The exact behavior is dependent on the specific media: you can use this button to scrub video, to skip to the previous music track, or move to the previous photo in a slide show.</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>FF<br/>Right (D-Pad)<br/>Right Shoulder (R1)</td>
      <td>Right Shoulder (R1)</td>
      <td>FF<br/>Right (D-Pad)<br/>Right Shoulder (R1)</td>
      <td>Right (D-Pad)<br/>Right Shoulder (R1)</td>
      <td>Fast-forward in media playback contexts. The exact behavior is dependent on the specific media: you can use this button to scrub video, to skip to the next music track, or move to the next photo in a sideshow.</td>
    </tr>
  </tbody>
</table>


## Volume Control

You can stream audio to the headphone jack on the Amazon Fire TV Game Controller. Volume control for audio playback is available with the left and right trigger buttons (L2/R2). Volume control is a system function and cannot be mapped to other buttons in your app.

Note the following:

* If your app or game does not use these buttons, do not capture those input events. Doing so may interfere with the user's ability to control the volume.
* If your app or game does use those buttons for other purposes, the user may access system volume control from the GameCircle overlay or in the Fire TV launcher.

<table class="grid">
<colgroup>
<col width="30%" />
<col width="70%" />
<col width="15%" />
</colgroup>
  <thead>
    <tr>
      <th>Action</th>
      <th>Amazon Fire TV Game Controller Button</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Volume Up</td>
      <td>Left Trigger (L2)</td>
    </tr>
    <tr>
      <td>Volume Down</td>
      <td>Right Trigger (R2)</td>
    </tr>
  </tbody>
</table>

## Gameplay

Although gameplay user interfaces are highly individual, the following table describes basic recommendations.


<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="30%" />
<col width="30%" />
</colgroup>
  <thead>
    <tr>
      <th>Action</th>
      <th>Amazon Fire TV Remote Button</th>
      <th>Amazon Fire TV Game Controller Button</th>
      <th>Other Game Controller Button</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Primary Gameplay Action</td>
      <td>D-Pad Center</td>
      <td>A</td>
      <td>A</td>
    </tr>
    <tr>
      <td>Secondary Gameplay Action</td>
      <td>no recommendation</td>
      <td>B</td>
      <td>B</td>
    </tr>
  </tbody>
</table>


{% include links.html %}
