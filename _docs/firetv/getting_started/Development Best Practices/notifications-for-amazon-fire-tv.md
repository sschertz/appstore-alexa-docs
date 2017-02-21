---
title: Notifications for Amazon Fire TV
permalink: notifications-for-amazon-fire-tv.html
sidebar: firetv
product: Fire TV
SMEs: Mike Miller, David Goehring, Mustafa Hakim, Stephen Whitney. 
toc: false
github: true
---

2nd generation Fire TV devices now support standard Android notifications through the [Android Notifications API](http://developer.android.com/reference/android/app/Notification.html). These notifications appear in a \"Notification Center,\" as described below. However, for 1st generation Fire TV devices, support for standard notifications and the Notifications Center will roll out at a later date.

* TOC
{:toc}

## What Are Notifications?

A notification is a message to your users that appears outside of your app's user interface. Fire TV supports the [Android Notification API](http://developer.android.com/reference/android/app/Notification.html), with some limitations.

Typically you use notifications to let users know that you have an update available with your app. The update might be any of the following:

* New content available
* New levels in a game
* New episodes available for an existing series
* Live TV channel lineup changed
* New game packs available in your app
* New functionality
* New badges or rewards earned
* New release

You're probably used to receiving messages from the various apps on your smartphone. Notifications for your Fire TV apps can provide the same kind of user engagement. The notifications are a way of reaching out to users to encourage them to re-engage with your app in some way.

Note that notifications are not [Recommendations](https://developer.android.com/training/tv/discovery/recommendations.html). Recommendations use a different Android API (which is not currently supported on Fire TV) to recommend content to users, usually on specific rows on the home screen.

## Types of Notifications Supported on Fire TV

There several types of notifications you can create on Fire TV:

* [Heads-up notifications](../notifications-for-amazon-fire-tv.md#headsup)
* [Toasts](../notifications-for-amazon-fire-tv.md#toasts)
* [Standard Notifications](#standard notifications)

### Heads-up Notifications {#headsup}

Fire TV supports Android's [heads-up notifications](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Heads-up). Typically on Android devices, heads-up notifications are floating windows that appear at the top of the screen and allow users to interact with the window (such as receiving a call while you're in another app). 

On Fire TV, heads-up notifications appear at the bottom of the screen and fade away after a few seconds. Some interaction is allowed while the notification appears. For example, users can click a button or dismiss the notification with the Back button. 

All undismissed heads-up notifications will be displayed in the Notification Center, where users can review the notifications at their leisure. This also ensures that users will actually see the notifications. (Previously, if users missed the heads-up notification, there wasn't any way to return to it.)

When you create heads-up notifications, you must set the notification as a high priority:

```java
.setPriority(Notification.PRIORITY_HIGH) // heads up must be high priority
```

[Progress displays](https://developer.android.com/training/notify-user/display-progress.html) and [stacked notifications](https://developer.android.com/training/wearables/notifications/stacks.html) are not supported on Fire TV. Regarding layouts, heads-up notifications are limited in height to normal layouts only (there are no expanded layouts).

### Toasts {#toasts}

Though rarely used, Fire TV also supports [toasts](https://developer.android.com/guide/topics/ui/notifiers/toasts.html). Toasts are small pop-ups that appear within your app briefly and then disappear, with no ability for the user to interact with the message. Unlike heads-up notifications, toasts are not stored within the Notification Center.

### Standard Notifications {#standardnotifications}

Standard notifications are informational in nature and do not interrupt the current foreground activity (unlike heads-up notifications, which pop-up in the bottom-right corner of the screen). Notifications from your app are added to the Notification Center as soon as they are raised. 

The Notification Center appears under the Settings menu. When users have unseen notifications, a little bell appears next to Settings. 

{% include image.html file="firetv/getting_started/images/notificationbell" type="png" %}

Within Settings, users select Notifications. This opens what is referred to in the documentation as the Notification Center.

{% include image.html file="firetv/getting_started/images/notificationcenter" type="png" %}

The Notifications Center arranges the notifications in a single list ordered by most recent first. The Fire TV Appstore client itself will send notifications when your app has an update (hence you don't have to worry about pushing these kinds of notifications). In the following screenshot, there are two apps that have updates.
 
{% include image.html file="firetv/getting_started/images/notificationcards" type="png" %}

When users click the icon, they see the updates available for the app. Users can choose to update the app or not. 

{% include image.html file="firetv/getting_started/images/notificationavailableupdates" type="png" %}

Notifications should contain enough information to convey the reason for the notification. They can also include an optional intent to launch when the notification is selected. For example, your notification can allow the user to launch your app with a deep link to the specific activity related to the intent.

When the update finishes, the user is prompted to launch the app. 

{% include image.html file="firetv/getting_started/images/notificationlaunch" type="png" %}

Users can also turn app notifications on or off on a device-by-device basis. (More granular notification configurations are not possible.) Users can control app notifications by going to **Preferences > Notification Settings > App Notifications**.

{% include image.html file="firetv/getting_started/images/configurenotificationoptions" type="png" %}

Users can also select **Do Not Interrupt** to suppress heads-up notifications from appearing on the screen. (You will still see standard notifications in the Notification Center and see the bell icon on Settings on the main navigation.)

All notifications appear in the Notification Center until the user engages with the notifications, dismisses them, disables notifications for the app, or until the app removes them. 

A notification that was not dismissed while displayed as a heads-up notification will appear in the Notification Center.

Each notification indicates the time or date it was received.

As soon as a user visits the Notification Center, whether to click on a notification or not, the bell icon on "Settings" is removed.

## Requirements for Notifications

The following table lists requirements for notifications.

<table class="grid">
<colgroup>
<col width="15%" />
<col width="70%" />
<col width="15%" />
</colgroup>
<thead>
<tr>
<th>Feature</th>
<th>Description</th>
<th>Required?</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Large Image</td>
  <td markdown="span">A large image used as the tile image in the notification card. This image appears in the Notification Center. The image should be a 16:9 aspect ratio. The actual size of the image container is 228dp with x 128dp, so the image should be at least these dimensions (or larger). Larger images will be scaled down. See [setLargeIcon](https://developer.android.com/reference/android/app/Notification.Builder.html#setLargeIcon(android.graphics.drawable.Icon)) for more details. If a large image isn’t provided, Fire TV will use the large app icon. </td>
  <td>Optional</td>
</tr>

<tr>
  <td>Action</td>
  <td>Android intent to launch app or deep-link.</td>
  <td>Optional</td>
</tr>

<tr>
  <td>Title</td>
  <td>Title of the notification. </td>
  <td>Required</td>
</tr>

<tr>
  <td>Description</td>
  <td>Description of the notification.</td>
  <td>Required</td>
</tr>

<tr>
  <td>Action Text</td>
  <td>Text for the Menu button (the default is “Launch now”); this is only included in notifications marked as Urgent.</td>
  <td>Optional</td>
</tr>

<tr>
  <td>Priority</td>
  <td>Android priority for the notification (range is -2 to +2). If the priority is +1 or +2 (HIGH or MAX), the notification is considered an Urgent notification. If not included, the default priority is 0.</td>
  <td>Optional</td>
</tr>
</tbody>
</table>

## Code Samples

For code samples and technical instruction on how to create notifications, see the [Notifications](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) in the Android documentation.

## System Notifications

Fire TV also sends system notifications to users. Although third-party apps can't replicate or initiate system message notifications, it's worth mentioning them here. Common Fire TV system notifications might include the following:

* Low battery status
* Disconnected headphones
* Bluetooth pairing
* Application download/installation complete
* Other system updates

These notifications appear as small popups in a corner of the screen and can be raised over any content that is on the screen.
System notifications will also be stored in the Notification Center (unless users dismiss them in the initial display).

Fire TV also provides notifications when your app has an update. These notifications aren't something you create with your app but rather are triggered by the Fire TV appstore client. 

Fire TV creates two types of app update messages. A "Required Update" message is a visual prompt on an app icon, indicating that there is a new update available. 

Another update message is presented to users though an on-device dialogue box. When users start a new session in your app or game, they are presented with the option to “Update Now” or “Launch without Updating” along with details describing what’s new in the update.

{% include image.html file="firetv/getting_started/images/092816_badlands" type="png" caption="App Update Notification" %}

After the app has been installed, users get a quick notification letting them know it’s ready to launch: 

{% include image.html file="firetv/getting_started/images/092816_PostInstall2" type="png" caption="Post-Install Notification" %}

## Migrating from the Deprecated Fire TV Notifications API {#deprecation}

In the past, Amazon Fire TV included a custom notifications API designed for TV use. As of Fire OS 5, the Amazon Notifications API is **deprecated**. If your app uses the Fire TV Notification API, those notifications will continue to work, but that API will be **removed** from the platform at a later date. If your app uses the Amazon Notifications API, we strongly suggest you move to using the [standard Android (Lollipop) notification API](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) instead.

To migrate your app from the Amazon Notifications API:

*   Remove all references to `AmazonNotification` and `AmazonNotificationManager` as well as to the package `com.amazon.device.notification`. These can be replaced with the stock [`Notification`](https://developer.android.com/reference/android/app/Notification.html) and [`NotificationManager`](http://developer.android.com/reference/android/app/NotificationManager.html) classes in the Android notification API.
*   References to the `AmazonNotification.setType()` method and the `TYPE_INFO` and `TYPE_MEDIA_INFO` constants should be removed. Android notifications do not specify these types.

{% include links.html %}
