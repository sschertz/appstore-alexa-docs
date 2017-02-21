Fire OS is the operating system that runs Amazon's Fire TV, tablets, and other devices. Fire OS is a fork of Android 5.1 (Lollipop, API level 22), so if you have an Android app, it will most likely run on Amazon's Fire devices.

As a developer, you might not have to adjust your Android code at all to publish your app on Amazon's platform. You can [test your app here][app-testing-service-understanding] and see if it everything simply works.

* TOC
{:toc}

## Differences in Services

The way Fire OS differs from Android is in the services. Instead of using Google's services (for activities such as browsing, location, messaging, payments, and so on), Fire OS uses Amazon's services. Most notably, Amazon uses the Amazon Appstore to list your app while Google uses Google Play.

If your Android app connects into Google's ecosystem of services, porting your Android app to the Fire OS platform will require you to tap into Amazon's ecosystem of services instead. Additionally, Fire OS supports only Android Lollipop (API Level 22).

When you're building your app, follow the standard [Android documentation](https://developer.android.com/). Where there are differences to account for with Amazon's Fire OS platform, they're noted in the documentation on this site.

The goal is to provide as much parity as possible with Android (minus Google's services) so that you don't have to learn another development platform or make changes to your existing Android app.

## Devices and Fire OS Versions

Most Fire devices receive over-the-air updates to get the latest version of Fire OS automatically. Not every Fire device receives a push of the same Fire OS version at the same time. Sometimes the updates roll out to different devices at different times. But for the most part, Fire devices largely run the same version.

{% include tip.html content="For details on the differences between Android TV development and Fire TV development, see [Fire TV Development versus Android TV Development][amazon-fire-tv-differences-from-android-tv-development]." %}