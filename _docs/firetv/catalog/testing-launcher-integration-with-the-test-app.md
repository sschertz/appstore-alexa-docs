---
title: Testing Fire TV Launcher Integration with the Integration Test App
permalink: testing-launcher-integration-with-the-test-app.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

After integrating your app with the Fire TV Home Screen Launcher (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]), use Amazon's Integration Test app to ensure that your app responds correctly to capability requests as well as sign in and playback intents.

This test app mimics the requests the launcher makes, and is the easiest way to ensure that you have implemented launcher integration correctly in your own app. (Note that this topic applies to Fire TV apps whose media catalogs are integrated with Fire TV.)

Use this testing option when you have completed development on your app and have already implemented the code to respond to Intents from the launcher (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]). You will also need a Fire TV device to be able to test your app's integration with the Integration Test App.

To test a fully functional app or to specifically test sign-in and playback intents use the Android Debug Bridge (ADB) option. (See [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb].)

* TOC
{:toc}

## Process Overview

Use the following high-level process to test your Fire TV launcher integration with Amazon's Fire TV Integration Test App:

1.  Download the Integration Test App.
2.  Update your own app to use the correct Intent package to work with the Integration Test App.
3.  Install both your app and the Fire TV Integration Test App to a Fire TV device.
4.  Test your app with the Fire TV Integration Test App.

## Step 1: Download the Integration Test App

Before you can start testing, download the Fire TV Integration Test App to your computer:

<a class="noCrossRef" href="https://s3.amazonaws.com/android-sdk-manager/aftv-misc/IntegrationTest.apk"><button type="button" style="cursor: pointer" class="btn btn-primary navbar-btn">Download App</button>

## Step 2: Modify Your App for Testing

You will need to modify your app to work with the Fire TV Integration Test App.

To perform this modification, make the following changes to your app:

1.  If you have not already done so, make the changes to your app described in [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration].
2.  Inside of your broadcast capabilities method, change the name of the `com.amazon.tv.launcher` to `com.amazon.tv.integrationtestonly`. For example, `intent.setPackage(“com.amazon.tv.launcher”)` should become `intent.setPackage(“com.amazon.tv.integrationtestonly”)`.

{% include note.html content="The `integrationtestonly` intent package is only for testing. When you are finished testing, make sure to rename the Intent package back to its original name of `com.amazon.tv.launcher` before you submit your app to the Amazon Appstore." %}

## Step 3: Install Both Your App and the Fire TV Integration Test App to a Fire TV Device

Both your app and the Integration Test App will need to be installed to a Fire TV Device before you can start testing.

To install your app and the Fire TV Integration Test App to a Fire TV device:

1.  Using a network connection or USB cable, connect the Fire TV device to your computer using ADB. See [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device]).
2.  Sideload both your app and the integration tester app onto the device. See [Installing and Running Your App][installing-and-running-your-app]).

## Step 4: Test Your App with IntegrationTest

Now that both your app and the Integration Test App are available on your Fire TV device, you can start testing.

To test your app:

1.  Launch the Integration Test App:
    1.  From the Fire TV main menu, go to **Settings** > **Applications** > **Manage All Installed Applications**, and select the IntegrationTest app.
    2.  Select **Launch Application**.
2.  Click **Request Capabilities**.

    The IntegrationTest app sends a broadcast Intent to request capabilities from your app. If your app responds to that request, and includes all the required elements, the test app displays a success message and the values for each capability from your app.

    {% include note.html content="A successful result means only that your app responded to the broadcast. This test does not verify the correctness of any data provided by you." %}

3.  Verify that capability values from your app are correct.

    If the capability exchange was successful, Fire TV displays a text entry box and a **Send Intent** button.

4.  Enter a content URL or ID, and click **Send Intent**.

    The content reference may be a URI or a data extra name/ value pair, depending on your implementation. Enter a content reference that your app recognizes. If the content reference and your implementation is correct, your app launches and handles the Intent and the content reference.

    After you finish testing your app, remember to change the broadcast intent package back to `com.amazon.tv.launcher`.

{% include links.html %}
