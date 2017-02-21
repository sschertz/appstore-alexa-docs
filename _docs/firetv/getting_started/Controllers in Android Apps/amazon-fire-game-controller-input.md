---
title: Game Controller Input
permalink: amazon-fire-game-controller-input.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Amazon Fire TV game controllers (as well as other controllers that conform to the Bluetooth HID gamepad profile) have specific buttons, Android motion, and key event constants. Use this information to
capture input events in your app.

For guidelines on button behavior for all supported controllers, see [Controller Behavior Guidelines][controller-behavior-guidelines]. For information on handling input for Amazon Fire TV remote controls, see [Amazon Fire TV Remote Input][amazon-fire-tv-remote-input].

* TOC
{:toc}

## Buttons

The Amazon Fire TV (2nd Generation) Game Controller has these buttons:

{% include image.html file="firetv/getting_started/images/gamecontroller2" type="png" title="New game controller" %}

The Amazon Fire TV (1st Generation) Game Controller has these buttons:

{% include image.html file="firetv/getting_started/images/gamepad-callouts" type="png" title="Game controller" %}

{% include image.html file="firetv/getting_started/images/gamepad-callouts-second-view" type="png" title="Game controller, other buttons" %}

## Capturing Input

Game controllers used with Amazon Fire TV generate Android [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html) events for digital button presses (such as the A button), and [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html) events for analog control movement (such as a joystick action).

You can handle simple button input with standard Android event listener interfaces and callbacks ([`onClick()`](http://developer.android.com/reference/android/view/View.OnClickListener.html#onClick%28android.view.View%29), [`onFocusChange()`](http://developer.android.com/reference/android/view/View.OnFocusChangeListener.html#onFocusChange%28android.view.View,%20boolean%29), and so on).

To capture specific button press events in your [`View`](http://developer.android.com/reference/android/view/View.html), override input event handlers such as [`onKeyDown()`](http://developer.android.com/reference/android/view/View.html#onKeyDown%28int,%20android.view.KeyEvent%29).

Test for the input constants from the [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html) class to capture specific keys. For example, to capture a press from the A button, use this code:

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event){
    boolean handled = false;

    switch (keyCode){
        case KeyEvent.KEYCODE_BUTTON_A:
                // ... handle selections
                handled = true;
                break;
     }
     return handled || super.onKeyDown(keyCode, event);
}
```

To capture motion events, override the [`onGenericMotionEvent()`](http://developer.android.com/reference/android/view/View.html#onGenericMotionEvent(android.view.MotionEvent)) event in your [`View`](http://developer.android.com/reference/android/view/View.html). Use the input constants from the [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html) class to determine which control generated the movement, and the events in the [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html) class such as [`getAxisValue()`](http://developer.android.com/reference/android/view/MotionEvent.html#getAxisValue(int)) to determine the values for the motion:

```java
@Override
public boolean onGenericMotionEvent(MotionEvent event){
    // get changes in value from left joystick
    float deltaX = event.getAxisValue(MotionEvent.AXIS_X);
    float deltaY = event.getAxisValue(MotionEvent.AXIS_Y);

    if (deltaX > 0.5 && deltaY > 0.5) {
        // do something
        handled = true;
    }

    return handled || super.onGenericMotionEvent(event);
}
```

## Primary and Secondary Input Events {#primary_and_secondary}

Some game controller actions on Fire TV devices may raise more than one input event for a single action. For example, the D-Pad on the Amazon Fire TV game controller is an *analog directional control* (producing motion events), but a *digital control* on the Fire TV remote controls (producing key events).

Similarly, the selection action is the *A button* on a game controller, but it is the *D-Pad center button* on Fire TV remotes. Some game controller actions on Amazon Fire TV first raise a primary input event (usually a motion event). Then, if those events are not handled by your app, they raise a second input event (usually a key event). Both of the primary and secondary input events are listed in the [Input Event Reference](../amazon-fire-game-controller-input.md#inputreference) table below.

Secondary input events can help you simplify the process of handling game controller input. If your app is interested only in button and D-Pad events from a game controller, the secondary events enable you to ignore motion events altogether and only deal with key events.

Similarly, because the A button generates both `KEYCODE_BUTTON_A` and `KEYCODE_DPAD_CENTER`, if your app supports the center D-Pad button on the Fire TV remotes, you do not have to also test for the A button.

Note that your app may behave as if it is receiving double input if you do not properly handle the primary input events. Make sure your input event handlers return `true` if you have captured and handled an event. The secondary input event is not generated if the first has been captured.

## Input Event Reference {#inputreference}

The following table describes the motion and key event constants for each game controller button, the suggested user experience behavior for those buttons, and the default behavior of those buttons in Amazon Fire TV user interface.

Digital buttons report key events ([`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)), and analog controls report motion events ([`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)). See [controller-behavior-guidelines] for information on suggested behavior for controller input in your app.

{% include note.html content="Do not capture or throw away input events for any buttons you do not use in your app. Allowing the system to handle unused events enables background behavior such as media playback and volume control." %}

The events listed in the Secondary Event column are raised **in addition to** the events in the `MotionEvent` or `KeyEvent` columns, if your app does not handle that primary event. See [Primary and Secondary Input Events](../amazon-fire-game-controller-input.md#primary_and_secondary) (above) for information on these secondary events.

In this table, a *game* is an app that was submitted to the Amazon Appstore in the games category and installed onto the device from the Amazon Appstore.

<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
  <thead>
    <tr>
      <th>Game Controller Button</th>
      <th>MotionEvent</th>
      <th>KeyEvent</th>
      <th>Secondary Event</th>
      <th>Default Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>none</td>
      <td>none</td>
      <td>none</td>
      <td>For games with GameCircle support, launch the GameCircle overlay. For games without GameCircle support, display a “Game Paused” dialog. For all other apps, return the user to Home.      </td>
    </tr>
    <tr>
      <td>Back</td>
      <td>none</td>
      <td><code>KEYCODE_BACK</code></td>
      <td>none</td>
      <td>Return the user to the previous operation or screen (Activity).</td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>none</td>
      <td><code>KEYCODE_MENU</code></td>
      <td>none</td>
      <td>Invoke the Android context menu (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>).</td>
    </tr>
    <tr>
      <td>GameCircle (Fire Game Controller 1st Generation Only)</td>
      <td>none</td>
      <td><code>none</code></td>
      <td>none</td>
      <td>For games with GameCircle support, launch the GameCircle overlay. For games without GameCircle support, display a “Game Paused” dialog. For all other apps, return the user to the Games screen of the launcher.</td>
    </tr>
    <tr>
      <td>A</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_A</code></td>
      <td><code>KEYCODE_DPAD_CENTER</code></td>
      <td>Select the item with the current focus.</td>
    </tr>
    <tr>
      <td>B</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_B</code></td>
      <td><code>KEYCODE_BACK</code></td>
      <td>Go back to the previous screen (Activity) (Same as Back).</td>
    </tr>
    <tr>
      <td>X</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_X</code></td>
      <td>none</td>
      <td>Do nothing.</td>
    </tr>
    <tr>
      <td>Y</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_Y</code></td>
      <td>none</td>
      <td>Do nothing.</td>
    </tr>
    <tr>
      <td>Left (D-Pad) Right (D-Pad)</td>
      <td><code>AXIS_HAT_X</code> (&gt;0 is right)</td>
      <td>none</td>
      <td><code>KEYCODE_DPAD_LEFT</code> <code>KEYCODE_DPAD_RIGHT</code></td>
      <td>Move the focus left or right in the user interface.</td>
    </tr>
    <tr>
      <td>Up (D-Pad) Down (D-Pad)</td>
      <td><code>AXIS_HAT_Y</code> (&gt;0 is down)</td>
      <td>none</td>
      <td><code>KEYCODE_DPAD_UP</code> <code>KEYCODE_DPAD_DOWN</code></td>
      <td>Move the focus upward or downward in the user interface.</td>
    </tr>
    <tr>
      <td>Left Stick (Left/Right)</td>
      <td><code>AXIS_X</code> (&gt;0 is right)</td>
      <td>none</td>
      <td><code>KEYCODE_DPAD_LEFT</code> <code>KEYCODE_DPAD_RIGHT</code> (if movement is &gt;.5)</td>
      <td>Move the focus in the user interface in the given direction.</td>
    </tr>
    <tr>
      <td>Left Stick (Up/Down)</td>
      <td><code>AXIS_Y</code> (&gt;0 is down)</td>
      <td>none</td>
      <td><code>KEYCODE_DPAD_UP</code> <code>KEYCODE_DPAD_DOWN</code> (if movement is &gt;.5)</td>
      <td>Move the focus in the user interface in the given direction.</td>
    </tr>
    <tr>
      <td>Left Stick Press</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_THUMBL</code></td>
      <td>none</td>
      <td>Play/Pause.</td>
    </tr>
    <tr>
      <td>Right Stick (Left/Right)</td>
      <td><code>AXIS_Z</code> (&gt;0 is right)</td>
      <td>none</td>
      <td>none</td>
      <td>Do nothing.</td>
    </tr>
    <tr>
      <td>Right Stick (Up/Down)</td>
      <td><code>AXIS_RZ</code> (&gt;0 is down)</td>
      <td>none</td>
      <td>none</td>
      <td>Do nothing.</td>
    </tr>
    <tr>
      <td>Right Stick Press</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_THUMBR</code></td>
      <td>none</td>
      <td>Play/Pause.</td>
    </tr>
    <tr>
      <td>Play/Pause (Fire Game Controller 1st Generation Only)</td>
      <td>none</td>
      <td><code>KEYCODE_MEDIA_PLAY_PAUSE</code></td>
      <td>none</td>
      <td>Play/Pause.</td>
    </tr>
    <tr>
      <td>Rewind (Fire Game Controller 1st Generation Only)</td>
      <td>none</td>
      <td><code>KEYCODE_MEDIA_REWIND</code></td>
      <td>none</td>
      <td>Rewind.</td>
    </tr>
    <tr>
      <td>Fast Forward (Fire Game Controller 1st Generation Only)</td>
      <td>none</td>
      <td><code>KEYCODE_MEDIA_FAST_FORWARD</code></td>
      <td>none</td>
      <td>Fast forward.</td>
    </tr>
    <tr>
      <td>Left Trigger (L2)</td>
      <td><code>AXIS_BRAKE</code></td>
      <td>none</td>
      <td>none</td>
      <td>Volume Up.</td>
    </tr>
    <tr>
      <td>Left Shoulder (L1)</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_L1</code></td>
      <td>none</td>
      <td>Rewind.</td>
    </tr>
    <tr>
      <td>Right Trigger (R2)</td>
      <td><code>AXIS_GAS</code></td>
      <td>none</td>
      <td>none</td>
      <td>Volume Down.</td>
    </tr>
    <tr>
      <td>Right Shoulder (R1)</td>
      <td>none</td>
      <td><code>KEYCODE_BUTTON_R1</code></td>
      <td>none</td>
      <td>Fast Forward.</td>
    </tr>
  </tbody>
</table>


{% include links.html %}
