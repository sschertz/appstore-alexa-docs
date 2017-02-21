---
title: Identifying Controllers
permalink: identifying-controllers.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

The Amazon Fire TV platform enables users to connect up to seven Bluetooth controllers at the same time. If your app or game supports input from multiple users or players, you need to be able to determine the connected controllers, identify the features of those controllers, and differentiate user input coming from different controllers.

The Fire TV platform uses standard Android features of the [`InputDevice`][1] class. The `InputDevice` class enables you to get a list of all the connected input devices (including controllers) and query the features of an input device.

You can also get an input device object from any key or motion event, which enables you to handle input from different controllers or different users as it occurs.

* TOC
{:toc}

## Getting Input Devices and Device IDs

Input devices connected to Amazon Fire TV are represented by the Android [`InputDevice`][1] class. Connected input devices are given a device ID at system boot and when a new device is added.  Input devices in the list of IDs can be actual controllers such as game controllers, or they can represent other forms of input such as the onscreen keyboard. The device IDs themselves are arbitrary and do not uniquely identify a specific controller or controller type.

You can get the list of all the available device IDs to determine the number and type of controllers available to your app or game. You can also get device IDs or input device objects from a specific key or motion event, to determine which controller was being used for that event. 

### Get All Input Device IDs

You can get an array of all the available input devices IDs with the static [`InputDevice.getDeviceIds()`][3] method, and then associate those IDs with actual input device objects with [`InputDevice.getDevice()`][4]:

```java
int[] ids = InputDevice.getDeviceIds();

for (int i=0;i < ids.length; i++) {
// get an InputDevice object based on the device ID
InputDevice device = InputDevice.getDevice(ids[i]);

// ...
}
```

Note that the list of device IDs you get from the `getDeviceIds()` method contains only those input devices that are actually _connected_ to the system.  Some Bluetooth controllers may disconnect (sleep) to save power if they have not been used in some time, or a controller may disconnect if it is out of range.

Sleeping or otherwise unavailable controllers are not considered connected and are not enumerated in the list of device IDs.  You can listen for controller connect and disconnect events by implementing the [`InputDeviceListener`][5] interface.

### Get the Input Device ID from a Key or Motion Event

You can get the ID of the device that triggered a key or motion event inside an event handler with the [`InputEvent.getDeviceId()`][6] method:

```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
int id = event.getDeviceId();
InputDevice device = InputDevice.getDevice(id);
}
```

Or, just get the input device object itself:

```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
InputDevice device = event.getDevice();
}
```

## Identifying Controller Features

Android input device IDs are arbitrary and do not uniquely identify any controller or controller type.  To determine the type of input device, query the features of that device to determine whether input came from a controller (a remote, game controller, or second screen app), or from some other device.

Use the [`InputDevice.getSources()`][7] method to get the features of an input device.  This method returns an integer bitmap that indicates device capabilities and features.  Use the constants defined the [`InputDevice`][1] class to compare that bitmap with the specific features your app is interested in.

```java
// get an InputDevice object based on the device ID
InputDevice device = InputDevice.getDevice(id);

// getSources() gives you an integer bitmap that defines the features of the device;
// compare with constants from InputDevice to find controller features
if ((device.getSources() & InputDevice.SOURCE_CLASS_JOYSTICK != 0 {
// this controller has a joystick
}
```

You can also get input device feature bitmaps directly from key and motion events with the [`InputEvent.getSource()`][8] method:

```java
public boolean onGenericMotionEvent(MotionEvent event) {
// did this event come from a joystick?
if ((event.getSource() & InputDevice.SOURCE_CLASS_JOYSTICK) != 0) {
// ... handle input events from controllers with joysticks }
// ...
}
```

### Is this Device a Game Controller?

You can identify game controllers (both the Amazon game controller and other game controllers) by testing for both the [`InputDevice.SOURCE_GAMEPAD`][9] and [`InputDevice.SOURCE_JOYSTICK`][10] constants.  Note that this code does not differentiate between an Amazon game controller and a Bluetooth controller from another manufacturer.

```java
int hasFlags = InputDevice.SOURCE_GAMEPAD | InputDevice.SOURCE_JOYSTICK;
boolean isGamepad = inputDevice.getSources() & hasFlags == hasFlags;
```

### Is this Device a Remote?

You can identify the Amazon Fire TV Remote or Voice Remote by the [`InputDevice.SOURCE_DPAD`][11] constant.  However, as some keyboards may also identify themselves as having a D-Pad, you should also test for the keyboard type ([`InputDevice.KEYBOARD_TYPE_NON_ALPHABETIC`][12]):

```java
int hasFlags =  InputDevice.SOURCE_DPAD;
bool isRemote = (inputDevice.getSources() & hasFlags == hasFlags)
&& inputDevice.getKeyboardType() == InputDevice.KEYBOARD_TYPE_NON_ALPHABETIC;
```

### Is this Device a Second Screen App?

You can identify the Amazon Second Screen app with both the [`InputDevice.SOURCE_MOUSE`][13] and [`InputDevice.SOURCE_TOUCHPAD`][14] constants.

```java
int hasFlags =  InputDevice.SOURCE_MOUSE | InputDevice.SOURCE_TOUCHPAD;
bool isRemote = inputDevice.getSources() & hasFlags == hasFlags;
```

[1]: http://developer.android.com/reference/android/view/InputDevice.html
[3]: http://developer.android.com/reference/android/view/InputDevice.html#getDeviceIds%28%29
[4]: http://developer.android.com/reference/android/view/InputDevice.html#getDevice%28int%29
[5]: http://developer.android.com/reference/android/hardware/input/InputManager.InputDeviceListener.html
[6]: http://developer.android.com/reference/android/view/InputEvent.html#getDeviceId%28%29
[7]: http://developer.android.com/reference/android/view/InputDevice.html#getSources%28%29
[8]: http://developer.android.com/reference/android/view/InputEvent.html#getSource()
[9]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_GAMEPAD
[10]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_JOYSTICK
[11]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_DPAD
[12]: http://developer.android.com/reference/android/view/InputDevice.html#KEYBOARD_TYPE_ALPHABETIC
[13]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_MOUSE
[14]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_TOUCHPAD

{% include links.html %}
