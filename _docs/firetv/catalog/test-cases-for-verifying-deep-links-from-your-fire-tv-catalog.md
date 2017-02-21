---
title: Test Cases for Verifying Fire TV Deep Links
permalink: test-cases-for-verifying-deep-links-from-your-fire-tv-catalog.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

This page provides a list of test cases to execute for your Fire TV apps before submitting your app to the Amazon Appstore. Performing the test workflow as described will help facilitate your app's acceptance to the Amazon Appstore when submitted. Note that these test cases apply to apps that have been integrated with Fire TV and whose media catalogs have been integrated with and uploaded to Amazon. See [understanding-fire-tv-catalog-integration]).

* TOC
{:toc}

## Introduction of the Recommended Testing Process

The following flowchart gives an overview of the recommended testing process for verifying that you have correctly integrated your app with the Fire TV Home Screen Launcher. Depending on the state (published or unpublished) and conditions of your app (login required or not, free trial or not), you will only need to perform the steps that apply to your app.

{% include image.html url="firetv/TestingFlowchart" file="catalog/catalog_TestingFlowchart" type="jpg" %}

Follow each set of steps below, as it pertains to your app. Use the flowchart as a guide to help you keep track of where you are in the overall testing process.

## Step 1: Verify that the Amazon Home Screen Launcher and Your App Are Functioning as Expected

Depending on whether or not your app is currently available from the Amazon Appstore, choose from one of the following two test cases, and execute those steps as described. These steps test the following functionality:

*   The Amazon Home Screen Launcher is working correctly with your app.
*   Your app provides the correct UX for customers.

### Test Case: Your app is currently available on Amazon Appstore

To verify launcher and app functionality for an app that is currently available from the Amazon Appstore:

1.  Without the app installed, locate one of your content items that is not also available on Prime, and navigate to the provider line in the **More Ways To Watch** (**MWTW**) button:

    {% include image.html file="firetv/catalog/images/catalog_MWTW_button" type="png" %}

    *   If there is no **MWTW** button, this is likely content that is not available with any other provider (including Prime or Amazon Instant Video).

        In this instance, on the **Episode/Movie** detail page of the content, locate the **Watch Now with [App Name] – Open App** button. Note that if your app has a free trial version, this button should be labeled **Watch with [Provider] **– [X time] Free Trial.

    {% include image.html file="firetv/catalog/images/catalog_No_MWTW_button" type="png" %}

    *   If there is a **MWTW** button, click that button, and you should see **Also Available with [App Name]** to the right of your app name.
2.  Select the app name from the **MWTW** list (or the action button from the **Movie/Episode** detail page, if there is no **MWTW** button).

    The app detail page should display, with an option to purchase/download the app.

    {% include image.html file="firetv/catalog/images/catalog_DownloadPurchase" type="png" %}

3.  Purchase/download the app.
4.  Open the app, but do not sign in. (Finishes with activation screen for app.)
5.  Go to **Step 2** of the testing process (below) and execute the appropriate testing steps for your app.

### Test Case: Your app is not yet published to the Amazon Appstore

To verify launcher and app functionality for an app that has not yet been published to the Amazon Appstore:

1.  Sideload the APK onto a Fire TV device. (See [Installing and Running Your Fire TV App][installing-and-running-your-app].)
2.  Locate one of your content items that is not also available on Prime, and navigate to the provider line in the **More Ways To Watch** (**MWTW**) button.

    {% include image.html file="firetv/catalog/images/catalog_MWTW_button" type="png" %}

    *   If there is no **MWTW** button, this is likely content that is not available with any other provider (including Prime or Amazon Instant Video).
In this instance, on the Episode/Movie detail page of the content you should see a button that says **Watch with [Provider] – Open App** or **Watch with [Provider] **– [X time] Free Trial.

    {% include image.html file="firetv/catalog/images/catalog_No_MWTW_button" type="png" %}

    *   If there is a **MWTW** button, to the right of your app name it should say **Open App**.
3.  Select the app name from the **MWTW** list (or the action button from the **Movie/Episode** detail page, if there is no **MWTW** button).
The app detail page should display, with a button to open the app. The Fire TV system will know that the app is already installed because it was sideloaded.
4.  Open the app, but do not sign in.
You should see the activation screen for your app.
5.  Go to **Step 2** of the testing process (below) and execute the appropriate testing steps for your app.

## Step 2: Verify Playback of Selected Content

This test case verifies that playback of content selected by a user begins playback directly from within Fire TV, without re-routing the user to your app as an intermediate step.

To test playback of selected content from within Fire TV:

1.  Activate your Fire TV device, if needed. This is a one-time setup step for new devices. If your device has not yet been activated, you will see prompts that you can follow to complete the activation.
2.  If your app requires login credentials and an active subscription, go through the process of signing in to your app and activating a subscription, if you have not already done so.
3.  Navigate to a content item that is available from your app:
    1.  Back out of your app via the **BACK** button on the remote and return to the app detail page.
    2.  Back out of this page to return to the **Movie/Episode** detail page.
4.  The **Buy Box** button should be updated to say **Watch Now with [Provider]**.
5.  Select and start playback of your content:
    1.  Click the **MWTW** button (if present).
    2.  Within the **MWTW** menu, your app should appear at the top of the list with nothing next to it.
    3.  Select the **Buy Box** button (or app name within **MWTW**).
Selecting this button should launch the app and attempt to start playback of the content you had selected.
    4.  Confirm that playback of the content that you selected has started.
6.  While you are still in the app, or even during video playback, repeat steps 3-5 of this section for at least one additional content item.

## Step 3: Verify Launcher Updates when Your App is Uninstalled

The next two test cases test a deep link to a content item when your app is currently not installed on a Fire TV device. Choose the appropriate set of steps depending on whether your app requires login credentials.

### Test Case: Your App Does Not Require Any Login Credentials

To verify a deep link for an uninstalled app that does not require login credentials:

1.  Press and hold the HOME button for your Fire TV device. The HUD should appear.
2.  Select **Settings** from the HUD popup
3.  Navigate right to **Applications > Manage Installed Applications.**
4.  Uninstall your app.
5.  Locate the content you used in your previous deep link test.
6.  Confirm that the **Buy Box** button has reverted to its un-activated state; it should no longer say **Watch Now with [Provider]**.

### Test Case: Your App Requires Login Credentials

To verify a deep link for an uninstalled link that does require login credentials:

1.  Sign out of your credentials from within the app, if you have not already done so.
2.  Back out of your app, and navigate to the content page that you previously used to test the deep link to your content item.
3.  Confirm that the **Buy Box** button has reverted to its un-activated state; it should no longer say **Watch Now with [Provider]**.
4.  Sign back in, or reactivate the subscription within the app.
5.  Confirm that the **Buy Box** button is updated to say **Watch Now with [Provider]**.
6.  Uninstall your app:

    1.  Press and hold the **HOME** button.

    The **Heads Up Display (HUD)** appears.

    2.  Select **Settings** from the **HUD** popup menu.
    3.  Navigate right to **Applications > Manage Installed Applications.**
    4.  Uninstall your app.
7.  Locate the content that you previously used in the deep link test.
8.  Confirm that the **Buy Box** button has reverted to its un-activated state; it should no longer say **Watch Now with [Provider]**.

## Sending an Invalid Content ID

Another test case that you should execute is sending an Invalid Content ID to your app to make sure that your app handles this condition gracefully. If your app does not handle this condition gracefully, re-visit your error handling code and make changes as appropriate.

To test sending an Invalid Content ID to your app:

1.  Download or sideload your app to your Fire TV device.
2.  Sign-in and/or activate a subscription to your app, if necessary.
3.  Use either the Test App (see [testing-launcher-integration-with-the-test-app]) or ADB commands (see [testing-launcher-integration-with-adb]) to send an invalid ID to your app. For more information on integrating your app with the Fire TV Home Screen launcher, see [launcher-integration]).
4.  Send a valid ID first to make sure the content launches properly.
5.  Test the following invalid ID conditions:   
    1.  Send an invalid ID using numerals.
    2.  Send an invalid ID using alphabetical characters.
    3.  Send an invalid ID using a mix of numbers and letters.
6.  Verify when sending invalid IDs that the app handles the intent gracefully.

The app should launch, and leave the user on the main screen within the app. If the user is not taken to the main screen within the app, modify your error handling code to do so.

{% include links.html %}
