---
title: GameController API (Deprecated)
permalink: gamecontroller-api.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Amazon Fire TV devices support up to seven Bluetooth controllers to be connected to the system at the same time, which enables you to develop multi-player apps and games. To identify controllers and manage input events, use standard Android mechanisms including the [InputDevice][1] class and Android event handlers [`onKeyUp()`][2], [`onKeyDown()`][3] or [`onGenericMotionEvent()`][4]). 

In the past the Fire TV platform included a custom API to assist with input management of game controllers. As of Fire OS 5, the GameController API is **deprecated** and will be **removed** from the platform at a later date. If your app uses the GameController API we strongly suggest you move to using the standard Android (Lollipop, API 22) [`InputDevice`][1] APIs instead.

This document describes how to migrate your code from the deprecated GameController API to standard Android input device APIs.

* TOC
{:toc}

## Controllers and Player Numbers

The GameController API is used primarily to associate player numbers with attached controllers. Much of this behavior is available in the standard Android [`InputDevice`][1] class through the [`getControllerNumber()`][6] method.

Each controller is assigned a unique number when that controller is connected to Fire TV. The number may change if the controller is disconnected and connected again.

Input devices such as remote controls that are not game controllers (specifically, that are not indicated by either [`InputDevice.SOURCE_GAMEPAD`][7] or [`InputDevice.SOURCE_JOYSTICK`][8]) have 0 as the controller number.

## Migrating from the Amazon GameController API

In Fire OS 5, calls to the Fire TV GameController API still operate as they did previously. However, the GameController API is deprecated and will be removed from the platform at a later date. We **strongly suggest** that if your app currently uses the GameController API that you migrate your code to use standard Android input device management.

To migrate your app from the GameController API:

* Remove all references to the `GameController` class, including the `com.amazon.device.gamecontroller` package.
* Remove `GameController.init()` from your onCreate() method.
* Replace `GameController` methods for getting the player ID with Android [`InputDevice.getControllerNumber()`][6].
* Replace the `GameController` event constants with standard Android KeyEvent or MotionEvent constants, as in the table below.


| GameController Input Constant |  Android Key or Motion Event Constant |
| ----- | ----- |
| `GameController.AXIS_STICK_LEFT_X` |  `MotionEvent.AXIS_Y` |
| `GameController.AXIS_STICK_LEFT_Y` |  `MotionEvent.AXIS_Y` |
| `GameController.AXIS_STICK_RIGHT_X` |  `MotionEvent.AXIS_Z` |
| `GameController.AXIS_STICK_RIGHT_Y` |  `MotionEvent.AXIS_RZ` |
| `GameController.AXIS_TRIGGER_LEFT` | `MotionEvent.AXIS_BRAKE` |
| `GameController.AXIS_TRIGGER_RIGHT` | `MotionEvent.AXIS_GAS` |
| `GameController.BUTTON_A` |  `KeyEvent.KEYCODE_BUTTON_A` |
| `GameController.BUTTON_B` |  `KeyEvent.KEYCODE_BUTTON_B` |
| `GameController.BUTTON_X` |  `KeyEvent.KEYCODE_BUTTON_X` |
| `GameController.BUTTON_Y` |  `KeyEvent.KEYCODE_BUTTON_Y` |
|  `GameController.BUTTON_SHOULDER_LEFT` |  `KeyEvent.KEYCODE_BUTTON_L1` |
|  `GameController.BUTTON_SHOULDER_RIGHT` |  `KeyEvent.KEYCODE_BUTTON_R1` |
|  `GameController.BUTTON_TRIGGER_LEFT` | `KeyEvent.KEYCODE_BUTTON_L2` |
|  `GameController.BUTTON_TRIGGER_RIGHT` | `KeyEvent.KEYCODE_BUTTON_R2` |
| `GameController.BUTTON_STICK_LEFT` |  `KeyEvent.KEYCODE_BUTTON_THUMBL` |
| `GameController.BUTTON_STICK_RIGHT` |  `KeyEvent.KEYCODE_BUTTON_THUMBR` |
| `GameController.BUTTON_DPAD_UP` | `KeyEvent.KEYCODE_DPAD_UP` |
| `GameController.BUTTON_DPAD_DOWN` |  `KeyEvent.KEYCODE_DPAD_DOWN` |
|  `GameController.BUTTON_DPAD_LEFT` |  `KeyEvent.KEYCODE_DPAD_LEFT` |
|  `GameController.BUTTON_DPAD_RIGHT` |  `KeyEvent.KEYCODE_DPAD_RIGHT` |
|  `GameController.BUTTON_DPAD_CENTER` |  `KeyEvent.KEYCODE_DPAD_CENTER` |
|  `GameController.BUTTON_AXIS_HAT_X` |  `MotionEvent.AXIS_HAT_X` |
|  `GameController.BUTTON_AXIS_HAT_Y` |  `MotionEvent.AXIS_HAT_Y` |

## Player Numbers in the GameController API (Deprecated)

Although you can connect up to seven Bluetooth game controllers to an Amazon Fire TV device, only four of those controllers are assigned to player numbers. Note that controllers are assigned to player number slots and do not re-shuffle when an individual controller is disconnected, that is, player numbers are not consecutive.

The GameController API includes methods to access the player numbers for available game controllers. If your game supports more than four players you can always manage controllers and player numbers inside your game on your own as well.

{% include note.html content="All of the methods in this section throw a `NotInitializedException` exception if you have not called `GameController.init()` before using them." %}

### Getting Player Numbers by Device ID

Use the `GameController.getPlayerNumber()` static method with an Android device ID to get the player number for that device. You can get an Android device ID with the [`InputDevice.getDeviceIDs()`][9] method, from any key or motion event ([`InputEvent.getDeviceId()`][10]), or from a player number with `GameController.getDeviceID()`.

Player numbers are defined system-wide. Not all device IDs may be associated with a player number. If this is the case `getPlayerNumber()` throws the `PlayerNumberNotFoundException` exception.

```java
int[] ids = InputDevice.getDeviceIds();

for (int i=0; i < ids.length; i++) {
    try {
       int playerNum = GameController.getPlayerNumber(ids[i]);
    }   catch (PlayerNumberNotFoundException e) {
        // ...
    }
    // ...
}
```

### Getting Device IDs by Player Number

The `GameController.getDeviceID()` method is the reverse of `getPlayerNumber()`; given a player number (from 1 to 4), it returns the Android device ID. Not all player numbers may be associated with device IDs. If this is the case `GameController.getDeviceID()` throws the `DeviceNotFoundException` exception.

```java
for (int n = 0; n < GameController.MAX_PLAYERS; n++) {
    try {
        int devicenum = GameController.getDeviceID(n + 1);
    }
    catch (DeviceNotFoundException e) {
        // ...
    }
    // ...
}
```

### Getting the Number of Players

Use the `GameController.getPlayerCount()` static method to get the number of available players. Note that player numbers are not necessarily consecutive, for example, two players may be assigned to slots 2 and 4.

    int players = GameController.getPlayerCount();

### Getting GameController Objects by Player

Use the `GameController.getControllerByPlayer()` static method with a player number to retrieve a `GameController` object for use with frame-based event input (see Frame-Based Input Events). Player numbers are assigned by the system and can be from 1 to 4. Not all player numbers may have associated controllers. If this is the case `GameController.getControllerbyPlayer()` throws the `PlayerNumberNotFoundException` exception.

```js
for (int n = 0; n < GameController.MAX_PLAYERS; n++) {
    GameController gameController = null;
    try {
        gameController =
                GameController.getControllerByPlayer(n + 1);
    }
    catch (PlayerNumberNotFoundException e) {
        // ...
    }
}
```

## Frame-Based Input Events in the GameController API (Deprecated)

You can always handle input events from controller using the standard Android event-driven programming model, in which your code handles input events as they arrive. The `GameController` class enables you to use a frame-based input model, in which events are cached and you can query the state of the controller before rendering each frame. With this model, you use the following steps:

1. Provide your own game loop to replace the standard Android event model.
2. Forward standard input events to `GameController`.
3. For each frame within the loop:
    * Query the state of each controller for player input.
    * Render the frame using that input data.

{% include note.html content="All of the methods in this section throw a `NotInitializedException` exception if you have not called `GameController.init()` before using them." %}

### Forwarding Events to GameController

To use the `GameController` API to manage your input events, forward key and motion events to the `GameController` class to handle. Override the [`onKeyUp()`][11], [`onKeyDown()`][12] and [`onGenericMotionEvent()`][13] methods to pass those events to `GameController`:

```java
//Forward key down events to GameController so it can manage state
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    boolean handled = false;
    try {
        handled = GameController.onKeyDown(keyCode, event);
    }
    catch (DeviceNotFoundException e) {
    }
    return handled || super.onKeyDown(keyCode, event);
}
//Forward key up events to GameController so it can manage state
@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    boolean handled = false;
    try {
        handled = GameController.onKeyUp(keyCode, event);
    }
    catch (DeviceNotFoundException e) {
    }
    return handled || super.onKeyUp(keyCode, event);
}
//Forward motion events to GameController so it can manage state
@Override
public boolean onGenericMotionEvent(MotionEvent event) {
    boolean handled = false;
    try {
        handled = GameController.onGenericMotionEvent(event);
    }
    catch (DeviceNotFoundException e) {
    }
    return handled || super.onGenericMotionEvent(event);
}
```

### Implementing the Game Loop

Implement the game loop on your own thread. Inside the `run()` method for that thread, test for input and render each frame based on that input. Don't forget to stop and start the thread in your activity's [`onPause()`][14]) and [`onResume()`][15]) methods.

Within the loop, use `GameController.startFrame()` to reset the input event queue between frames. Here's a simple example:

```java
while (running){
    GameController.startFrame();

    //Draw the background

    //Draw each player
    for (int n = 0; n GameController.DEAD_ZONE * GameController.DEAD_ZONE){
    //stick angle is greater than the center dead zone
    x[n] += Math.round(deltaX * 10);
    y[n] += Math.round(deltaY * 10);
}
```

#### Button Presses

Use the `wasButtonPressed()` or `wasButtonReleased()` methods to test whether button input has occurred since the last frame:

```java
if (gameController.wasButtonPressed(GameController.BUTTON_A)
    || gameController.wasButtonPressed(GameController.BUTTON_DPAD_CENTER)){
    // draw a circle at the current player's x,y position
}
```

Use `isButtonPressed()` to test the current state of any button:

```java
//Dpad button presses, move the player's position:
x[n] += gameController.isButtonPressed(GameController.BUTTON_DPAD_RIGHT) ? +5 : 0;
x[n] += gameController.isButtonPressed(GameController.BUTTON_DPAD_LEFT) ? -5 : 0;
y[n] += gameController.isButtonPressed(GameController.BUTTON_DPAD_DOWN) ? +5 : 0;
y[n] += gameController.isButtonPressed(GameController.BUTTON_DPAD_UP) ? -5 : 0;
```

## GameController API Event Constants (Deprecated)

The GameController class includes a set of static event constants that represent button and axis event types. These constants are identical to the standard constants from the KeyEvent and MotionEvent classes, but are the same for any controller, and are often more logically named. For example, `GameController.BUTTON_A` is identical to [`KeyEvent.KEYCODE_BUTTON_A`][16] . Use these constants with the frame-based controller model described in Frame-Based Input Events.

{% include note.html content="For standard controller buttons (Back, Media Play/FF/Rewind), manage those events with standard Android [KeyEvent][17] and [MotionEvent][18] events. The Home and GameCircle buttons are system events and cannot be captured by your app." %}


| Game Controller Control |  Analog Movement: `wasAxisChanged() getAxisValue()`  |  Button Presses: <br/> `wasButtonPressed()` <br/> `wasButtonReleased()`  <br />`isButtonPressed()` |  Default Behavior (if the event is not handled by your code) |
| ----- | ----- | ----- |----- | ----- |
| D-Pad  (Left/Right) |  `AXIS_HAT_X`  (>0 is right) |  none |  Move the focus in the user interface in the given direction. |
| D-Pad  (Up/Down) |  `AXIS_HAT_Y`  (>0 is down) |  none |  Move the focus in the user interface in the given direction. |
| Left Stick  (Left/Right) |  `AXIS_STICK_LEFT_X`  (>0 is right) |  none |  Move the focus in the user interface in the given direction. |
| Left Stick  (Up/Down) |  `AXIS_STICK_LEFT_Y`  (>0 is down) |  none |  Move the focus in the user interface in the given direction. |
| Left Stick Press |  none |  `BUTTON_STICK_LEFT` |  Do nothing. |
| Right Stick  (Left/Right) |  `AXIS_STICK_RIGHT_X` (>0 is right) |  none |  Do nothing. |
| Right Stick  (Up/Down) |  `AXIS_STICK_RIGHT_Y`  (>0 is down) |  none |  Do nothing. |
| Right Stick Press |  none |  `BUTTON_STICK_RIGHT` |  Do nothing. |
| A |  none |  `BUTTON_A` |  Select the item with the current focus. |
| B |  none |  `BUTTON_B` |  Go back to the previous screen (Activity) (Same as Back). |
| X |  none |  `BUTTON_X` |  Do nothing. |
| Y |  none |  `BUTTON_Y` |  Do nothing. |
| Left Trigger (L2) |  `AXIS_TRIGGER_LEFT` |  none |  Select the item with the current focus. |
| Left Shoulder (L1) |  none |  `BUTTON_SHOULDER_LEFT` |  Do nothing. |
| Right Trigger (R2) |  `AXIS_TRIGGER_RIGHT` |  none |  Select the item with the current focus. |
| Right Shoulder (R1) |  none |  `BUTTON_SHOULDER_RIGHT` |  Do nothing. |

[1]: http://developer.android.com/reference/android/view/InputDevice.html
[2]: http://developer.android.com/reference/android/view/View.html#onKeyUp%28int,%20android.view.KeyEvent%29
[3]: http://developer.android.com/reference/android/view/View.html#onKeyDown%28int,%20android.view.KeyEvent%29
[4]: http://developer.android.com/reference/android/view/View.html#onGenericMotionEvent%28android.view.MotionEvent%29
[6]: http://developer.android.com/reference/android/view/InputDevice.html#getControllerNumber()
[7]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_GAMEPAD
[8]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_JOYSTICK
[9]: http://developer.android.com/reference/android/view/InputDevice.html#getDeviceIds%28%29
[10]: http://developer.android.com/reference/android/view/InputEvent.html#getDeviceId()
[11]: http://developer.android.com/reference/android/app/Activity.html#onKeyUp(int,%20android.view.KeyEvent)
[12]: http://developer.android.com/reference/android/app/Activity.html#onKeyDown(int,%20android.view.KeyEvent)
[13]: http://developer.android.com/reference/android/app/Activity.html#onGenericMotionEvent(android.view.MotionEvent)
[14]: http://developer.android.com/reference/android/app/Activity.html#onPause()
[15]: http://developer.android.com/reference/android/app/Activity.html#onResume()
[16]: http://developer.android.com/reference/android/view/KeyEvent.html#KEYCODE_BUTTON_A
[17]: http://developer.android.com/reference/android/view/KeyEvent.html
[18]: http://developer.android.com/reference/android/view/MotionEvent.html

{% include links.html %}
