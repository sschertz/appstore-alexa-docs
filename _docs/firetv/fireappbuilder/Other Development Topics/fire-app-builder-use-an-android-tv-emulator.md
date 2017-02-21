---
title: "Use an Android TV Emulator to Run Your App"
permalink: fire-app-builder-use-an-android-tv-emulator.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

When you develop your app with Fire App Builder, you should use an actual Fire TV device to test your app. See [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device] for details. However, if you're in a situation where you can only use an emulator, you can get by if you accept some limitations with the emulator. The emulator will work, but you can't click the media player buttons with your mouse.

Mouse clicks generate motion events, which aren't supported by media played in Fire App Builder (you'll see an error in logcat that says "java.lang.ClassCastException: android.view.MotionEvent cannot be cast to android.view.KeyEvent"). As a result, the app will crash on the emulator if you use your mouse to click the media player's buttons.

Instead of using your mouse on the media playback screen, to return to the previous screen after playing media, click the **Back** button on the right of the emulator (as indicated by the arrow in the following screenshot).

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_emulator" type="jpg" %}

Don't click the media player's buttons with your mouse. Outside of media playback, you can use your mouse to click wherever you want.

**To configure an emulator:**

When you configure the Android TV emulator, you must select at least API Level 23 or 24. You have flexibility with the other settings (resolution, size, and so on). (If you choose API Level 24, you'll be prompted to install Instant Run, which is a requirement for this API level.)

To set up an Android TV emulator for your app:

1.  Go to **Tools > Android > AVD Manager**, or click the **AVD Manager** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_avdmanager" type="png" %} on the top navigation bar.
2.  Click the **+ Create Virtual Device** button.

    {% include note.html content="You can select one of the default TV profiles, or you can customize the settings by following the steps below. If you select a default TV profile, skip ahead to step 12 where you select a system image." %}

3.  In the **Category** column, select **TV**.
4.  Click the **New Hardware Profile** button.
5.  In the **Device Name**, type something like **fire_tv_emulator**. (Avoid using parentheses in the name, as this may cause errors.)
6.  For the **Device Type**, select **Android TV**.
7.  For the **Screen size**, type the screen size you want (for example, **40**).
8.  For the **Resolution**, type the resolution you want (for example, **1280 x 720**).
9.  For the **Supported device states**, select only **Landscape** (clear the Portrait check box).
10. Click **Finish**.
11. In the "Choose a device definition" dialog box, select the emulator you just created and click **Next**.
12. In the Release Name column, select at least **Marshmallow API Level 23** or higher. If you haven't downloaded this system image yet, click **Download** to download it. (If you select API Level 22 or lower, media playback will fail in the emulator.)
13. Click **Next** and then click **Finish**.

The emulator is now listed as an option in your virtual devices.

{% capture avdname_note %}If you chose a default TV profile, you must rename the device to remove any parentheses in the name. These parentheses cause an error when you run your app. (You'll see an error that says the \"virtual device name contains invalid characters.\") To rename your virtual device, while viewing your virtual devices, click the **Edit** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_editthisavd" type="png" %} under the Actions column on the right. Then rename the device.{% endcapture %}

{% include note.html content=avdname_note %}

Run your app by clicking the **Run 'app'** button {% include inline_image.html alt="Run 'app' button" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %}. Select the virtual device you created:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_selectingemulator" type="png" max-width="80%" %}

Now you can use the emulator as usual. Just be careful when you play media. When you play media, don't click the buttons on the media player with your mouse. Instead, either use your keys or use the buttons to the right of the emulator as shown in the earlier screenshot.


{% include links.html %}
