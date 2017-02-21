---
title: Remote Input
permalink: amazon-fire-tv-remote-input.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

All buttons, Android events, and behavior guidelines are the same for all remotes &mdash; with the exception of the voice search (microphone) button, which is only available on some remotes.

For suggested guidelines on button behavior for all supported controllers, see [Controller Behavior Guidelines][controller-behavior-guidelines]. For information on handling input from the Amazon Fire Game Controller, see [Amazon Fire TV Game Controller Input][amazon-fire-game-controller-input].

* TOC
{:toc}

## Buttons

Most Amazon Fire TV remote controls have these buttons. Some Fire TV remotes do not include the microphone or voice search buttons.

{% include image.html file="firetv/getting_started/images/remote-callouts" type="png" alt="Remote control" caption="Remote control" %}


## Capturing Input

All Amazon Fire TV remote controls generate [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html) events for button presses, as any Android input device does. You can handle controller button input with standard Android event listener interfaces and callbacks ([`onClick()`](http://developer.android.com/reference/android/view/View.OnClickListener.html#onClick%28android.view.View%29), [`onFocusChange()`](http://developer.android.com/reference/android/view/View.OnFocusChangeListener.html#onFocusChange%28android.view.View,%20boolean%29), and so on). Neither the Amazon Fire TV Remote nor the Voice Remote raises motion events (from the Android [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html) class).

To capture specific button press events in your [`View`](http://developer.android.com/reference/android/view/View.html), override input event handlers such as [`onKeyDown()`](http://developer.android.com/reference/android/view/View.html#onKeyDown%28int,%20android.view.KeyEvent%29). Test for the input constants from the [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html) class to capture specific keys.

For example, to capture the LEFT, RIGHT, and CENTER D-Pad button (as well as the A button on a game controller), use this code:

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event){
boolean handled = false;

switch (keyCode){
case KeyEvent.KEYCODE_DPAD_CENTER:
case KeyEvent.KEYCODE_BUTTON_A:
        // ... handle selections
        handled = true;
        break;
case KeyEvent.KEYCODE_DPAD_LEFT:
        // ... handle left action
        handled = true;
        break;
case KeyEvent.KEYCODE_DPAD_RIGHT:
        // ... handle right action
        handled = true;
        break;
}
return handled || super.onKeyDown(keyCode, event);
}
```

As with all input events, your listener method should return `true` to capture the event and handle it, or pass that event on to `super.onKeyDown()` so that other controls can manage it.

## Input Event Reference

The following table describes the buttons, the Android [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html) constants, and the default behavior of those buttons. None of the Amazon Fire TV remotes raises motion events (from the Android [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html) class).

If you do not capture a specific input event the default behavior occurs.

<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Button</th>
      <th>KeyEvent</th>
      <th>Default Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>none</td>
      <td>Return the user to the Home screen. This is a system event and cannot be intercepted.</td>
    </tr>
    <tr>
      <td>Back</td>
      <td><code>KEYCODE_BACK</code></td>
      <td>Return the user to the previous operation or screen (Activity).</td>
    </tr>
    <tr>
      <td>Menu</td>
      <td><code>KEYCODE_MENU</code></td>
      <td>Invoke the Android context menu (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>).</td>
    </tr>
    <tr>
      <td>Microphone (Search) (Voice Remote only)</td>
      <td>none</td>
      <td>Invoke the system voice search. This is a system event and cannot be intercepted.</td>
    </tr>
    <tr>
      <td>Select (D-Pad Center)</td>
      <td><code>KEYCODE_DPAD_CENTER</code></td>
      <td>Select the user interface item with the current focus.</td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td><code>KEYCODE_DPAD_UP</code></td>
      <td>Move the focus upward in the user interface.</td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td><code>KEYCODE_DPAD_DOWN</code></td>
      <td>Move the focus downward in the user interface.</td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td><code>KEYCODE_DPAD_LEFT</code></td>
      <td>Move the focus left in the user interface.</td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td><code>KEYCODE_DPAD_RIGHT</code></td>
      <td>Move the focus right in the user interface.</td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td><code>KEYCODE_MEDIA_PLAY_PAUSE</code></td>
      <td>Control media playback. Play/Pause is a toggle.</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td><code>KEYCODE_MEDIA_REWIND</code></td>
      <td>Rewind or skip backwards in media playback contexts.</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td><code>KEYCODE_MEDIA_FAST_FORWARD</code></td>
      <td>Fast Forward or skip ahead in media playback contexts.</td>
    </tr>
  </tbody>
</table>

{% include links.html %}
