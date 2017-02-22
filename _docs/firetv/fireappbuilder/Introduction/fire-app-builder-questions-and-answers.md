---
title: Miscellaneous Questions
permalink: fire-app-builder-questions-and-answers.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

The following are common questions about Fire App Builder. 

Do apps created with Fire App Builder work on smartphone devices and tablets too, or just TV?
:   Currently, apps built with Fire App Builder work only on Fire TV and other Android TV platforms. A future enhancement will allow apps built with Fire App Builder to work on other devices as well. This is why the toolkit's name is "Fire App Builder" and not "Fire TV App Builder."

What kind of TVs do users need to have to use Fire TV?
:   Users need a TV with an HDMI port.

How can I implement Google Ads?
:   You can implement Google ads through the [VAST Component][fire-app-builder-vast-ads-component], which supports Google ads.

Does Fire App Builder support live feeds?
:   Yes, you can display any media from your feed. As long as your feed has valid media (live or pre-recorded), it can be played through your app built with Fire App Builder. See [Configure Live Streams][fire-app-builder-live-stream-configuration].

Do I need an actual Fire TV device to develop my app?
:   Using an actual device during development and testing is highly recommended, but you can also [use an emulator][fire-app-builder-use-an-android-tv-emulator] with some limitations.

What's the difference between Fire App Builder and Web Application Starter Kit (WASK)?
:   Fire App Builder uses Java with Android to create native Java apps, with Android Studio as your IDE. Fire App Builder is intended more for developers who want to build robust streaming media apps (without doing all the programming). WASK is a mobile web app toolkit (using HTML, CSS, JavaScript instead of Java), intended more towards content creators than developers.

Can I submit an app built with Fire App Builder to Google's Play Store as well as Amazon's Appstore?
:   Yes, since both app stores play Android apps, apps built with Fire App Builder work on both the Amazon Appstore and Google Play Store. However, services unique to Amazon or Google will only work in the respective platforms.

{: #translator}
How do I use reflection rather than translation when Fire App Builder maps the objects in my feed to its content model?
:   Reflection is actually used by default, so if you want to select this approach, remove the `translator` parameter from the [categories recipe][fire-app-builder-set-up-recipes-categories] and the [contents recipe][fire-app-builder-set-up-recipes-content]. 
    
    Additionally, when you configure the `matchlist` parameter with your recipes, you must map your feed element names to slightly different names. Instead of using @title, @subtitle, @id, @description, @url, @cardImageUrl, @backgroundImageUrl, and @tags, put an "m" before each word and then follow camelcase styling. Specifically, use the following: @mTitle, @mSubtitle, @mId, @description, @mUrl, @mCardImageUrl, @mBackgroundImageUrl, and @mTags.

{: #captions608}
How can I enable the CEA-608 captions that are available in my HLS live stream?
: No, Fire App Builder does not support CEA-608. Fire App Builder passes the responsibility to render captions to [AMZNMediaPlayer][fire-app-builder-amazon-media-player-component], which supports CEA-608 (see the [relevant code lines here][github-code-reference]). However, Fire App Builder only does so for `vtt` and `xml` extensions. CEA captions have a `m3u8` extension, so Fire App Builder ignores this extension and does not inform AMZNMediaPlayer to render the captions.

[github-code-reference]: https://github.com/amzn/fire-app-builder/blob/master/TVUIComponent/lib/src/main/java/com/amazon/android/uamp/ui/PlaybackActivity.java#L1070-L1080

{% include links.html %}
