---
title: Supporting Controllers in Web Apps
permalink: supporting-controllers-in-web-apps.html
sidebar: firetv
product: Fire TV
github: true
toc: false
---

Amazon Fire TV supports user input from the Amazon Fire TV remote, the Amazon Fire game controller, and other game controllers that support the Bluetooth HID gamepad profile. These controllers give users a means of navigating in your app and selecting items.

* TOC
{:toc}

## Using Input from the Amazon Fire TV Remote {#usinginput}

The bundled Amazon Fire TV Remote has the keys shown in the following image.

{% include image.html file="firetv/getting_started/images/remote-callouts" type="png" alt="Remote control" caption="Remote control" %}

To enable users to interact with your web app by using the remote, you need to capture the key events when users press one of the keys. Most key presses can be captured just as standard keyboard events in a browser.

For keycode mappings, see the following table. The **Back**, **Home**, **Menu**, and **Voice Search** buttons cannot be captured. For a workaround that allows customizing **Back** button behavior, see the Fire TV Web Apps section of [Amazon Web Apps Frequently Asked Questions](https://developer.amazon.com/public/solutions/platforms/webapps/faq).

<table class="grid">
   <colgroup>
      <col width="25%" />
      <col width="25%" />
      <col width="50%" />
   </colgroup>
  <thead>
    <tr>
      <th>Amazon Fire TV Remote Button</th>
      <th>Key Code</th>
      <th>Standard Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Select (D-Pad Center)</td>
      <td>13</td>
      <td>Selects the user interface item with the current focus.</td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td>38</td>
      <td>Moves the focus upward in the user interface.</td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td>40</td>
      <td>Moves the focus downward in the user interface.</td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td>37</td>
      <td>Moves the focus left in the user interface.</td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td>39</td>
      <td>Moves the focus right in the user interface.</td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td>179</td>
      <td>Controls media playback. Play/Pause is a toggle.</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>227</td>
      <td>Rewinds or skips backwards in media playback contexts.</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>228</td>
      <td>Fast Forwards or skips ahead in media playback contexts. </td>
    </tr>
    <tr>
      <td>Back</td>
      <td>NA</td>
      <td>Navigates back in the history stack.</td>
    </tr>
  </tbody>
</table>

These key events are sent only to apps that are in focus.

## Using Input from the Amazon Fire Game Controller {#using_input}

Developing for the Amazon Fire Game Controller is straightforward:

*  Amazon WebView supports the W3C standard [Gamepad](https://dvcs.w3.org/hg/gamepad/raw-file/default/gamepad.html) APIs.
*  The buttons on the Amazon Fire Game Controller map to the standard gamepad format.
*  In using the Gamepad APIs with the Amazon Fire Game Controller, the **Back** button is equivalent to **Select** on a standard controller, and the **Menu** button is equivalent to **Start**. If you do not use the Gamepad APIs, the buttons behave as **Back** and **Menu** buttons.

For more information on developing for the Standard Gamepad APIs, see [Jumping the Hurdles with the Gamepad API](http://www.html5rocks.com/en/tutorials/doodles/gamepad/).

If you choose not to use the Gamepad APIs, the buttons on the Amazon Fire Game Controller generally map to the same functions as on the Amazon Fire TV Remote (other than the **B** button).

The Amazon Fire TV (2nd Generation) Game Controller has these buttons:

{% include image.html file="firetv/getting_started/images/gamecontroller2" type="png" title="New game controller" %}

The Amazon Fire TV (1st Generation) Game Controller has these buttons:

{% include image.html file="firetv/getting_started/images/gamepad-callouts" type="png" title="Game controller" %}

{% include image.html file="firetv/getting_started/images/gamepad-callouts-second-view" type="png" title="Game controller, other buttons" %}

The following table shows the key mappings.

<table class="grid">
   <colgroup>
      <col width="25%" />
      <col width="25%" />
      <col width="50%" />
   </colgroup>
  <thead>
    <tr>
      <th>Amazon Gamepad Controller Button</th>
      <th>Key Code</th>
      <th>Standard Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Up (D-Pad/Joystick)</td>
      <td>38</td>
      <td>Move the focus upward in the user interface.</td>
    </tr>
    <tr>
      <td>Down (D-Pad/Joystick)</td>
      <td>40</td>
      <td>Move the focus downward in the user interface.</td>
    </tr>
    <tr>
      <td>Left (D-Pad/Joystick)</td>
      <td>37</td>
      <td>Move the focus left in the user interface.</td>
    </tr>
    <tr>
      <td>Right (D-Pad/Joystick)</td>
      <td>39</td>
      <td>Move the focus right in the user interface.</td>
    </tr>
    <tr>
      <td>A </td>
      <td>13</td>
      <td>Select the user interface item with the current focus.</td>
    </tr>
    <tr>
      <td>B</td>
      <td>8</td>
      <td>None</td>
    </tr>
    <tr>
      <td>X</td>
      <td>13</td>
      <td>Select the user interface item with the current focus.</td>
    </tr>
    <tr>
      <td>Y</td>
      <td>13</td>
      <td>Select the user interface item with the current focus.</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>NA</td>
      <td>Navigates back in the history stack.</td>
    </tr>
  </tbody>
</table>

## Play/Pause Media Button

A requirement for all media apps submitted for Fire TV is that they handle the media **Play/Pause** key events to control media playback. All Game applications must also handle the media **Play/Pause** key events to play or pause the game. The key code for the **Play/Pause** button is 179.

{% include note.html content="The button is a toggle. If the app is paused the  **Play/Pause** button should start the app. If the app is running, the **Play/Pause** button should pause it." %}


{% include links.html %}
