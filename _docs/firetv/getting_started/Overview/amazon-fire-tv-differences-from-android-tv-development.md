---
title: How Fire TV Development Differs from Android TV Development
permalink: amazon-fire-tv-differences-from-android-tv-development.html
sidebar: firetv
product: Fire TV
toc: false
reviewers: Stephen Whitney, Andy Prunicki
github: true
---

Because both Fire TV and Android TV use Android, you can push your same Android app out to both the Amazon Appstore and Google Play Store. Making your app available on both Amazon and Google stores can significantly increase the visibility and downloads of your app.

However, there are some differences with Fire TV that you need to account for in your code. Some of the differences are due to unique elements in the hardware or services. In some cases one service has features the other doesn't have, or they use different but equivalent services.

As you code for these differences, be aware that you can [identify Amazon Fire TV devices][identifying-amazon-fire-tv-devices] and conditionalize your code to target different behavior accordingly.

* TOC
{:toc}

## What is Fire TV and Android TV?

First, let's clarify what we mean by Android TV and Fire TV:

* **Android TV** refers to the Android operating system that has been optimized for TV (starting at Lollipop). Android Lollipop and the [Leanback Support Library][leanback] provide features that optimize Android for TV platforms. TV devices themselves can run Android TV as their native OS, or TVs can run Android through set-top boxes. You can learn more about [Android TV on Wikipedia](https://en.wikipedia.org/wiki/Android_TV).
* **Fire TV** refers to the Fire TV set-top box or stick that run the Fire operating system (OS) on your TV. Fire OS is a fork of Android 5.1 (Lollipop, API level 22) that accommodates Amazon hardware and services. Fire tablets also run Fire OS but do not leverage the features typically used for the "10-foot media experiences" on TV platforms. (Learn more about [Fire OS][fire-tv-fire-os-overview] or see the [Fire TV Device Specifications][device-and-platform-specifications].)

The important point is that both Android TV and Fire TV are Android-based, so the techniques you implement for your app share far more similarities than differences.

The following sections list the differences you need to account for in your code when planning for Fire TV.

## Google Services

Any APIs that rely on Google-specific services, such as Google location services, aren't available on Fire TV. Although there is an [Amazon Maps API](https://developer.amazon.com/public/apis/experience/maps) as well as an [Amazon Mobile Ads API](https://developer.amazon.com/public/apis/earn/mobile-ads), they are not yet supported on Fire TV.

## LeanBack Support Library

Fire TV supports some but not all of Android's Leanback Support Library. For example, Fire TV uses TV-specific UI components from Leanback, Leanback widgets will work, and Fire TV will honor intents tagged for the `LEANBACK_LAUNCHER`. But Leanback's `SearchFragment` (described in the next section) is not supported.

## Voice Search

For voice search, Android TV uses *app controls* that rely on Leanback APIs (for example, speech recognition with the [`SearchFragment`][searchfragment]). However, voice search on Fire TV does not use Leanback's `SearchFragment`. On Fire TV, voice search uses Amazon-specific *system controls*.

No matter where users are on Fire TV (whether on the Launcher or in an app), when users press the microphone button on a voice-enabled remote and say the TV show or Alexa actions they want, this action initiates a *global search* using the Alexa cloud service instead speech recognition APIs in the Leanback library.

Media requests through voice always return content from the Fire TV catalog. See [Implementing Search in Fire TV][implementing-search-fire-tv] for more details.

## Global Search

On Android TV, to integrate your content into global search, you can do so locally through your app using a search results `ContentProvider`.

With Fire TV, to make your content appear in global search results, you must [integrate your media content with the Fire TV Catalog][integrating-your-catalog-with-fire-tv]. Submission to the Catalog is done through a cloud-based model (rather than locally within your app).

## Audio Focus

If a user started playing music from a music app prior to starting your app, Fire TV will continue to play music over your app. The Play/Pause buttons will control the music instead of the video in your app.

To receive the audio focus, your app must register a [`MediaButtonReceiver`][1] in your the manifest. The `MediaButtonReceiver` will transfer the audio focus to your app's media service when your app starts. See [Managing Audio Focus][managing-audio-focus] for more details and code samples.

## Fast-forward, Rewind, and Menu Buttons

Both Android TV and Fire TV have 4-way directional pad (dpad), dpad_center/select, back, and play/pause buttons. However, Fire TV also offers rewind, fast-forward, and menu buttons that you can optionally use.

The Menu button on Fire TV invokes the Android context menu, which appears as a list of menu items centered on the screen. You can override the menu button to provide your own custom menu user interface, or for any other purpose. 

If you only have one menu item, consider using the Menu button as a simple toggle &mdash; for example, to turn closed captions on or off. If you do this, consider providing an onscreen hint to expose this feature to your users.

## In-app Purchasing

Android TV often uses Google In-App Billing for in-app purchases. For in-app purchases on Fire TV, you use Amazon's [In-App Purchasing (IAP) API](https://developer.amazon.com/public/apis/earn/in-app-purchasing). For more information, you can [see a detailed comparison of the two](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/migrating-from-googles-iab-to-amazons-iap).

## Analytics

Android TV uses Firebase for Analytics. With Fire TV, you can use [Amazon Mobile Analytics](https://aws.amazon.com/mobileanalytics/) or another analytics package (Google Analytics, Flurry Analytics, Crashlytics, and so on). Many of these analytics packages are configurable as modules if you build your app with [Fire App Builder][fire-app-builder-overview].

## SDK Level

While Android TV can use the latest SDK (Nougat), Fire TV uses only Lollipop (API level 22) as the minimum SDK level. (Some APIs from Marshmallow have been backported into Fire OS to support certain apps.)

## Recommendations

Android TV lets apps [make recommendations](https://developer.android.com/training/tv/discovery/recommendations.html) on the home screen. This same recommendations functionality is coming to Fire TV and will be available soon (most likely Q2 2017 or sooner).

## Emulators

When testing out your Fire TV app code, you use an actual Fire TV device (either the set-top box or stick) instead of a virtual emulator. See [Connect to Fire TV Through ADB][fire-app-builder-connecting-adb-to-fire-tv] for more details.

## Notifications API

You use the standard [Android Notifications API](http://developer.android.com/reference/android/app/Notification.html) for creating notifications for your Fire TV app. Fire TV provides the same toast notifications and persistence model as Android TV. However, in addition to toasts, Fire TV also provides Heads up notifications, which allow interactive buttons.

Additionally, instead of putting old notifications in a notification drawer, on Fire TV notifications are stored in a Notification Center. Learn more at [Notifications for Amazon Fire TV][notifications-for-amazon-fire-tv].

## Accessibility 

Fire TV provides [VoiceView](https://www.amazon.com/b?node=14100715011) to make your app accessible to the visually impaired. You can learn more about VoiceView and accessibility here: 
 
*  [Understanding Assistive Technologies for Fire OS](https://developer.amazon.com/appsandservices/solutions/platforms/fire-os/docs/implementing-accessibility-in-fireos) 
*  [Implementing Accessibility in Fire OS](https://developer.amazon.com/appsandservices/solutions/platforms/fire-os/docs/implementing-accessibility-in-fireos)

## Appstore 

Android TV devices use the Google Play Store. In contrast, Fire TV uses the Amazon Appstore. Any links you have pointing to the Google Play store will need links to the Amazon Appstore.

## Testing Your App

You can test your Android app's compatibility with Amazon by sideloading your app onto an Amazon Fire TV device. See [Connecting to Fire TV Through ADB][connecting-adb-to-fire-tv-device]. You can also test your app with the [App Testing Service][app-test].

When you connect to a Fire TV device through ADB and run your app with Android Studio, a successful app will load and play. If you close your sideloaded app, you can find it by going to **Settings > Applications > Manage Installed Applications**.

{% include tip.html content="See the [Test Criteria for Amazon Appstore Apps][appstore-test-criteria] for more details on requirements your app needs to meet for the Amazon Appstore." %}

[leanback]: https://developer.android.com/reference/android/support/v17/leanback/package-summary.html
[searchfragment]: https://developer.android.com/reference/android/support/v17/leanback/app/SearchFragment.html
[app-test]: https://developer.amazon.com/app-testing-service

{% include links.html %}

[1]: https://developer.android.com/reference/android/support/v4/media/session/MediaButtonReceiver.html