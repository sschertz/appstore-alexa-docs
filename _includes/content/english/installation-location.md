The settings in your Android Manifest file determine where your app is installed on Fire devices. Two storage locations are possible:

* External storage (such as an SD card)
* Internal storage (using the device's memory)

{% if include.device == "firetv" %}
Fire TV Stick doesn't have external storage, but Fire TV (the set-top box) does provide external storage options through a memory card slot.
{% endif %} 

{% if include.device == "firetablets" %}
Although older Fire tablets don't have external storage, the newer Fire tablets provide external storage options through memory card slots.
{% endif %} 

Generally, your app should specify external storage as the default install location.

* TOC
{:toc}

## Best Practices 

As a best practice, most apps should specify `preferExternal` for the `installLocation` in the Android Manifest file. If left unspecified, your app will be installed on internal storage.

Filling up internal storage can lead to:

* Fewer app installs
* Poor app ratings
* Negative customer experiences

Some users may have abundant space available in external storage, but if an app's Manifest does not specify `preferExternal`, the app will be installed internally. As a result, users get prompted with low storage warnings or cannot install the app at all, which leads to user frustration.

Selecting `preferExternal` helps ensure the greatest user base for your app and a better user experience on Fire devices.

## How to Specify External Storage

In the `AndroidManifest.xml` of your app, inside the `<manifest>` tag, add the `installLocation` attribute and set its value to `preferExternal`. Here's an example:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:installLocation="preferExternal" ... >
    ...
</manifest>
```

The `installLocation` parameter has several values available:

<table class="grid">
    <colgroup>
        <col width="30%" />
        <col width="70%" />
    </colgroup>
    <thead>
        <tr>
            <th>installLocation value </th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td markdown="span">
                `preferExternal`
            </td>
            <td markdown="span">
                **Recommended for most apps**. Install your app on external storage when available. If external storage is full, install the app to internal storage (if available). The user can manually move the app later by selecting it from Settings > Apps & Games > Manage All Applications.</td>
        </tr>
        <tr>
            <td markdown="span">
                `internalOnly`
            </td>
            <td align="left" markdown="span">
                Install the app only to internal storage. If there is not enough room in internal storage, the install will fail. The app cannot be manually moved later by the user. Not recommended for most apps. Choose this option only for the types of apps described in the [next section](#internalStorage).
            </td>
        </tr>
        <tr>
            <td markdown="span">
                `auto`
            </td>
            <td align="left" markdown="span">
                Install to internal storage by default. However, if internal storage is full, install the app to external storage (assuming external storage is available). The user can manually move the app later by selecting it from Settings > Apps & Games > Manage All Applications.
            </td>
        </tr>
    </tbody>
</table>

Note that even though the APK file is installed on external storage, all databases, private user data, optimized .dex files, and extracted native code are stored on internal storage.

See [App Install Location](http://developer.android.com/guide/topics/data/install-location.html) in the Android documentation for more information.

## When to Select Internal Storage {#internalStorage}

Most apps should select `preferExternal` for the `installLocation`. However, DRM-protected media apps are an exception:                                                                  

* If your app plays DRM-protected media, DRM resources may become unstable if USB storage becomes unavailable.
* If your app plays DRM-protected media, either do not include `installLocation` at all, or set its value to `internalOnly`.

In addition to DRM-protected media apps, the following types of applications should never be installed on external storage:

*  Services
*  Alarm Services
*  Input Method Engines
*  Account Managers
*  Sync Adapters
*  Device Administrators
*  Broadcast Receivers listening for “boot completed” message
*  Live Wallpapers (not supported on Fire OS)
*  App Widgets (not supported on Fire OS)

See the [App Install Location](http://developer.android.com/guide/topics/data/install-location.html) documentation in Android for more details.

{% if include.device == "firetv" %}
## Fire TV External Storage Settings

Fire TV (Generation 1) supports USB external storage and Fire TV (Generation 2) includes a microSD slot for external storage. On these devices, settings are provided for users to manage connected external storage.

{% include image.html file="firetv/getting_started/images/installlocation9" type="png" %}

Users can also move internally stored apps to the SD card:

{% include image.html file="firetv/getting_started/images/installlocation11" type="png" %}
{% endif %}

{% if include.device == "firetablets" %}
## Fire Tablets External Storage Settings

When a Fire OS 5 tablet detects an external storage card, the Storage page in Settings shows the option “Install Supported Apps on your SD card." The default is “on.”

{% include image.html file="firetv/getting_started/images/installlocation5" type="png" %}

When active, this setting effectively reverses the standard Android behavior of `installLocation="auto"` such that `auto` will act like `preferExternal` on Amazon devices. This is a step forward for customers using these devices, but the best choice for all Android devices, including those from Amazon, is to specify `preferExternal` in the manifest.

Fire OS tablets, like other current Android devices, also allow the user to move apps between internal and external storage. In this example, you can see the footprint of an app on internal storage even after moving it to external.

{% include image.html file="firetv/getting_started/images/installlocation7" type="png" %}

{% endif %}

## Handling secondary downloads in your app

For apps that perform their own secondary downloads as part of first run or at any other time, these downloaded files are usually stored where the APK file was installed on the device. If the app was installed on external storage, the APK file will be on external storage.

Some applications, particularly games, use secondary downloads containing additional textures, levels, or other asset files. If you are handling this download in your app code, such as part of first run initialization, use the Android [PackageManager getApplicationInfo](https://developer.android.com/reference/android/content/Context.html#getApplicationInfo()) method to retrieve the `ApplicationInfo` class.

The `ApplicationInfo` class contains the location of the APK (`sourceDir`) and the public parts of the source directory, including the resources and manifest (`publicSourceDir`). If the app has been installed on external storage, these paths will point to that location and indicate where users should install the additional downloaded content.

If your app uses `installLocation="preferExternal"` to install a 40MB APK onto external storage, but then stores a 1GB downloaded data file to internal storage because the app uses the `ApplicationInfo dataDir` path, this would defeat the purpose of using external storage and quickly fill up the device's internal storage, leading to customer frustration. Therefore make sure the secondary downloads use the `sourceDir` for the download path.

## Specifying Install Location with Unity

Unity is one of the most popular tools for creating games for the Amazon Appstore. Unity supports setting the install location through the Player Settings Inspector for Android. To configure your app install location on external storage:

1.  From your Unity project, click **File-Build Settings…**.
2.  From the **Build Settings** dialog, select **Android** in the Platform list and click the **Player Settings…** button to display the inspector.

    {% include image.html file="firetv/getting_started/images/installlocation1" type="png" %}

3.  Open the **Other Settings** section and look for the **Install Location** setting. `preferExternal` is usually the default -- this is almost always the best choice.

    {% include image.html file="firetv/getting_started/images/installlocation3" type="png" %}

YoYo Games’ Gamemaker:Studio also uses `preferExternal` as the default for Android projects. Although you can manually change this default, it's recommended that you leave it as is.

Other engines and development frameworks provide similar options. Consult the documentation for the tool you are using to make sure you are using are configured to set the `installLocation` to `preferExternal` in the APK manifest.
