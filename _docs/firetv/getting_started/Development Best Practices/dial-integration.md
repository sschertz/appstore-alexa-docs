---
title: DIAL Integration
permalink: dial-integration.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Amazon Fire TV devices support the [DIAL (Discovery-and-Launch) protocol][1] through the Whisperplay service. DIAL is an open protocol that enables your Fire TV app to be discoverable and launchable from another device via a second screen app. Both Fire TV and the second screen device must be on the same network.

Note that DIAL is not a casting or mirroring API. DIAL only enables apps on a second screen device to find and launch apps on a Fire TV. In most cases you implement both the second screen app (to send a launch message) and the corresponding Fire TV app (to receive that message).

For more information about the open DIAL protocol and to register your app with the DIAL service, see [the DIAL website][2].

* TOC
{:toc}

## Implementing DIAL

DIAL functionality does not require any changes to your Fire TV app's code, but you do need to modify your app's manifest and resources to indicate support for DIAL and to accept launch intents.

There are five steps to implementing DIAL support for your Fire TV and second screen apps:

1. In your second screen app, implement the DIAL protocol to discover and launch apps on Fire TV. See [the DIAL website][2] for more information, and specifically the information in [Details for Developers][3].
2. Register your Fire TV app with the DIAL registry. See [About the Registry][4] for details.
3. In your Fire TV app, handle DIAL launch intent payloads. This step is only necessary if your second screen app sends an intent payload. That payload is delivered as an [Intent][5] extra with the value `com.amazon.extra.DIAL_PARAM`.
4. In your Fire TV app, modify the Android manifest to support DIAL. See [Modify your Android Manifest][6].
5. In your Fire TV app, add a `Whisperplay.xml` file to your app's resources. See [Add the Whisperplay.xml File][7].

## Modify your Android Manifest

There are two changes you must make to your Android manifest (`AndroidManifest.xml`) to support DIAL:

* Add a `<meta-data>` element to `<application>` that indicates support for Whisperplay and DIAL.
* Add the `DEFAULT` category to your launch intent.

In the `<application>` portion of your manifest, add the following `<meta-data>` element:

```java
<application ... >
    <meta-data android:name="whisperplay"  android:resource="@xml/whisperplay"/>
    ...
</application>
```

Next, add the `DEFAULT` intent category to your primary (main) activity's `<intent-filter>` element:

```java
    <activity android:name=".MainActivity"
              android:label="@string/title_activity_main" >
        ...
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER"/>
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
    </activity>
```

## Add the Whisperplay.xml File

Add a file called `Whisperplay.xml` to your app's resources, in the `res/xml/` directory. The contents of the file should look like this, where `DialAppName` is the name of your app in the DIAL registry:

```java
<whisperplay>
    <dial>
        <application>
            <dialid>DialAppName</dialid>
            <startAction>android.intent.action.MAIN</startAction>
        </application>
    </dial>
</whisperplay>
```


## DIAL Payload Intent

If your app accepts a DIAL payload (information that can be passed to your app via the DIAL launch request), that payload will be delivered as an [Intent][5] extra with the value `com.amazon.extra.DIAL_PARAM`.

[1]: http://www.dial-multiscreen.org
[2]: http://www.dial-multiscreen.org/
[3]: http://www.dial-multiscreen.org/details-for-developers
[4]: http://www.dial-multiscreen.org/dial-registry
[5]: http://developer.android.com/reference/android/content/Intent.html
[6]: https://developer.amazon.com/public/solutions/devices/fire-tv/docs/dial-integration#manifest
[7]: https://developer.amazon.com/public/solutions/devices/fire-tv/docs/dial-integration#xmlfile

{% include links.html %}
