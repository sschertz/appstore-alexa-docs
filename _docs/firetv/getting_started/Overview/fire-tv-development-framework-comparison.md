---
title: Amazon Fire TV Development Framework Comparison
sidebar: firetv
product: Fire TV
permalink: fire-tv-development-framework-comparison.html
hippourl: https://developer.amazon.com/public/solutions/devices/fire-tv/docs/fire-tv-development-framework-comparison
reviewers: Russell Beattie, Mihir Choudhary, Jonathan Richardson, Pete Schwab, Mary Galvin, Chris DeNamur, Mario Mancia, Luca Sale, Stephen Whitney, Alexander Budyszewick
last_updated: 12-13-2016
toc: false
github: true
---

If you’re planning to build a media-based app for Amazon Fire TV, Amazon provides two frameworks that can help accelerate the development of your app. Each framework is oriented toward developers with particular skill sets:

* [Web App Starter Kit for Fire TV](../fire-tv-development-framework-comparison.md#wask): Intended for web developers building web apps using HTML5, CSS3, and JavaScript.
* [Fire App Builder](../fire-tv-development-framework-comparison.md#fab): Intended for Android developers building native apps using Java.

In addition to using different types of code, the two frameworks have somewhat different features. See the [Feature Comparison](../fire-tv-development-framework-comparison.md#feature_comparison) for more details.

* TOC
{:toc}

## Web App Starter Kit for Fire TV {#wask}

The Web App Starter Kit for Fire TV (available on [Github here](https://github.com/amzn/web-app-starter-kit-for-fire-tv)) is a starting point for creating media-oriented apps for Fire TV using HTML5, CSS3, and JavaScript. The web apps can then be packaged using the Amazon Developer Portal to create Fire TV apps that are available in the Amazon Appstore, indistinguishable from native apps.

With WASK, you start with a base app template that contains specific media functionality (for example, support for Media RSS or JSON feeds, or support for online video providers such as YouTube or Brightcove). You then customize this template by changing settings files or adding extended functionality using standard web technologies &mdash; JavaScript, HTML5, and CSS3.

Baked into the WASK template is the code needed to provide the large-screen experience consumers expect, as well everything needed to pass Amazon Appstore testing during the app submission process. At the bare minimum, you only need to provide a feed of media files, which the app will use to display a selectable list of categories and a rotating carousel of media content.

Here’s a screenshot of a simple layout:

{% include image.html file="firetv/getting_started/images/wask_simple_layout" type="png" %}

While customizing or extending the basic WASK template, you can test your app using the [Amazon Web App Tester](https://developer.amazon.com/public/solutions/platforms/webapps/docs/tester.html). This is a Fire TV app used to test web apps on an actual device. The Web App Tester uses the same native app wrapper and web engine that will be used when the app is published, giving you an accurate preview of your app during development.

When your app is ready, you can use the Amazon Developer Portal to submit your app to the Amazon Appstore and have it published within minutes, with no native coding needed.

After signing up online, filling in the basic app details, and uploading thumbnail and preview images, you have a choice about where to host your app. You can either host the app's asset files on your own web server and submit just the URL, or you can upload the assets to Amazon's servers, where it will be bundled into a standalone packaged app.

After you have submitted your app, it will go through an Amazon ingestion service, and you will be notified when your app is published.

To learn more, see [The Web App Starter Kit for Fire TV][the-web-app-starter-kit-for-fire-tv]. Some examples of Fire TV apps built using WASK include [Acorn TV][acorn-tv], [Urban Movie Channel][urban-movie-channel], and [Euronews][euronews].

## Fire App Builder {#fab}

Fire App Builder (available on [Github here](https://github.com/amzn/fire-app-builder)) provides a Java-based framework that you can use to easily and quickly build streaming media Android apps for Amazon Fire TV. In contrast to the HTML5/CSS3/JS used with WASK, Fire App Builder uses Java Android code.

With Fire App Builder, you work in Android Studio, connect to your Fire TV device through Android Debug Bridge (ADB), and generate an APK (Android Package Kit) file to upload to the Amazon Appstore.

Although Fire App Builder uses Android APIs (particularly the Leanback Library), you do much of the configuration and customization through JSON and XML files. For example, through JSON and XML files, you can configure more than a dozen components to add to your app. Components provide out-of-the-box functionality for analytics, ads, authorization, purchasing, and media players.

Fire App Builder minimizes a dependence on Java expertise as much as possible, but for more deep-level integration, you can build on top of Fire App Builder. You can add your own custom Java classes to extend functionality through common interfaces and other code. (If you don’t care to do any custom Java programming, though, you don’t have to.)

With Fire App Builder, your media feed can be JSON or XML, in any structure using any tag names. When you configure Fire App Builder, you’ll write query syntax (using JSON Jayway syntax or XPath expressions) to target the various elements of your feed.

Your feed can also require tokens for media protected by DRM. Support for YouTube-based feeds and other video hosting services is on the roadmap but not currently included.

You have a lot of control to adjust the colors, layout, typography, and more &mdash; all by editing XML or JSON files where these settings have been extracted.

Here’s a screenshot of a sample app built using Fire App Builder:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" %}

A more compressed homepage layout is also available.

To learn more, see the [Fire App Builder documentation][fire-app-builder-overview]. For a sample app built with Fire App Builder, see the [Hallmark app][hallmark].


## Feature Comparison {#feature_comparison}

The following table compares Fire App Builder and WASK features.

{% include note.html content="If a framework doesn’t have a feature, it doesn’t mean the framework won’t support it. It just means the feature isn’t already integrated in the code. Usually you can easily insert the third-party code needed to support these services." %}

<table class="grid">
<colgroup>
<col width="30%" />
<col width="30%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr>
<th style="text-align:center">Category</th>
<th style="text-align:center">Feature</th>
<th style="text-align:center">Fire App Builder</th>
<th style="text-align:center">WASK</th>
</tr>
</thead>
<tbody>
<tr>
  <td class="white" rowspan="2"><b>Code Base</b></td>
  <td class="white">Java/Android</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td>HTML5/CSS3/JS</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td rowspan="3" class="gray"><b>Feed Formats</b></td>
  <td class="gray">JSON feeds</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">Media RSS XML Feeds</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">Custom XML Feeds</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td rowspan="2"><b>App Delivery Options</b></td>
  <td class="white">Installed as APK on device</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Hosted app directly from URL</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="3"><b>Media Types</b></td>
  <td class="gray">HLS, DASH, Smooth Streaming, MP4</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">DRM-protected media</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Live streams</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white" rowspan="4"><b>Media Providers</b></td>
  <td class="white">YouTube</td>
  <td class="white"><div style="text-align: center"></div></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Brightcove</td>
  <td class="white"><div style="text-align: center"></div></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Kaltura</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Ooyala</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="2"><b>Media Players</b></td>
  <td class="gray">Amazon Media Player</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Brightcove</td>
  <td class="gray"><div style="text-align: center"></div></td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white"><b>Purchasing</b></td>
  <td class="white">In-App Purchasing</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="3"><b>Authentication</b></td>
  <td class="gray">Login with Amazon</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Facebook Authorization</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Adobe Primetime</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>

<tr>
  <td class="white" rowspan="2"><b>Ad Services</b></td>
  <td class="white">Freewheel Ads</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td class="white">VAST Ads</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td class="gray" rowspan="4"><b>Analytics</b></td>
  <td class="gray">Omniture Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Google Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Crashlytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Flurry Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="white"><b>Global Catalog Search</b></td>
  <td class="white" markdown="span">Integration into the [Fire TV catalog][integrating-your-catalog-with-fire-tv] for global voice search.</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
</tbody>
</table>

Again, you can add the services and features you want into either framework. The code is open (and open source), and you're free to enhance, extend, or otherwise build on top of the framework's code.

## Transitioning from Web Apps to Android Apps

Some companies prefer to start out with a web app (with WASK) and later transition to a Java Android app (such as with Fire App Builder). Note that when you submit an app to the Appstore, you select the app type (whether web app or Android app).

Once you submit an app, you can't transition from one app type to another. If you started out with a web app and wanted to upload a new version that was an Android app, you couldn't do this. You would need to upload a separate app entirely, which would mean losing any existing users.

If you're planning to make this transition from web app to Android app, consider using [Cordova](https://cordova.apache.org/) with your web app. Cordova allows you to wrap your web app as an APK and submit the web app as an Android app. If you later decide to go entirely native with an Android app, you can publish a new version of your Android app in the Appstore.

[acorn-tv]: https://www.amazon.com/RLJ-Entertainment-Acorn-TV/dp/B01EAY1XPW/ref=sr_1_1
[urban-movie-channel]: https://www.amazon.com/RLJ-Entertainment-Urban-Movie-Channel/dp/B016APTPSQ/ref=sr_1_1
[euronews]: https://www.amazon.com/Euronews-in-English/dp/B01LFEYRXA/ref=sr_1_1
[hallmark]: https://www.amazon.com/Hallmark-Channel-Everywhere-Fire-TV/dp/B018F20I42/ref=sr_1_1

{% include links.html %}
