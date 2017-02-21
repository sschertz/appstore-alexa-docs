---
title: Customizing the Mouse Pointer
permalink: customizing-mouse-pointer.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

If your app supports pointer-based input, Fire TV supports the use of a USB or Bluetooth-connected mouse that enables users to interact with your app. By default the mouse pointer appears as a large circle on the TV.

{% include image.html alt="Pointer circle" file="firetv/getting_started/images/pointer-circle-bg" type="png" %}

You can change the appearance of this mouse pointer into an arrow with an addition to the Android manifest (`AndroidManifest.xml`).

{% include image.html alt="Pointer arrow" file="firetv/getting_started/images/pointer-arrow-bg" type="png" %}

{% include note.html content="Although the Fire TV platform supports the use of a mouse, your Fire TV app must use a controller as the primary mode of navigation and user input to be accepted by the Amazon Appstore. See [Supporting Controllers on Fire TV][supporting-controllers-on-amazon-fire-tv] for more information." %}

## Modify the Android Manifest

To change the default mouse pointer to an arrow for a given activity, add a `<meta-data>` element to your manifest inside `<activity>`:

```java
<activity
        android:name=".MyActivity"
        android:label="My Activity">
    ...
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
    <meta-data android:name="com.amazon.input.cursor" android:value="pointer"/>
</activity>
```

{% include links.html %}
