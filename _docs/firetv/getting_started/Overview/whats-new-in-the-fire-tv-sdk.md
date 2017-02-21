---
title: What's New in Fire TV Development
permalink: whats-new-in-the-fire-tv-sdk.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

* TOC
{:toc}

### Fire TV User Interface Update

Fire TV Gen 2 devices received an update to the interface. The update provides a more cinematic experience that 
offers video trailers and content screenshots to help customers access the content they want more quickly. Third-party apps are also more visible on the homepage in recommended rows. Accessibility improvements were included that allow customers to use Fire TV and any compatible apps with the VoiceView screen reader. (See the [original announcement](http://phx.corporate-ir.net/phoenix.zhtml?c=176060&p=irol-newsArticle&ID=2206525).)

To upload the new Fire TV assets required for the new UI, see the instruction in [Image Asset Guidelines for Appstore Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets). The user interface update will be applied to generation 1 devices in a later phase.

### Fire App Builder

[Fire App Builder][fire-app-builder-overview] was released on September 29, 2016. Fire App Builder provides a Java-based framework that you can use to easily and quickly build streaming media Android apps for Amazon Fire TV. Fire App Builder lets you build an engaging, high-quality media experience on Fire TV following best practices and techniques — without having to develop all the code yourself. Fire App Builder’s code is Java-based and uses Android Studio, Gradle, and other tools common to Android app development. Fire App Builder is released as an open source project on Github ([github.com/amzn/fire-app-builder](github.com/amzn/fire-app-builder)) under the Apache 2.0 license.

### Fire TV Stick (Generation 2)

[Fire TV Stick with Alexa Voice Remote](https://www.amazon.com/dp/B00ZV9RDKK), referred to as Fire TV Stick (Generation 2) in the documentation, was released on September 28, 2016. This next-generation Fire TV Stick is 30 percent faster than the original best-selling version, offers access to more than 300,000 movies and TV episodes, and includes the Alexa Voice Remote—for only $39.99. Now sporting a quad-core processor, it has the horsepower to let your high-performance apps and games shine. Read more about it here: [Introducing the All-New Fire TV Stick with Alexa Voice Remote](https://developer.amazon.com/public/community/post/Tx1W8FCB4UA5KIB/Introducing-the-All-New-Fire-TV-Stick-with-Alexa-Voice-Remote). You can see the specs for Fire TV Stick (Generation 2) on [Fire TV Device Specifications][device-and-platform-specifications].

## Fire TV Documentation Updates

The following updates have been made to the Fire TV documentation:

{: .grid}
| Date | Page | Details |
|-----|-----|------|
| Jan 20, 2016 | [System X-Ray Overview][fire-tv-system-xray], [Developer Tool Options][fire-tv-system-xray-developer-tools], [Customize System X-Ray][fire-tv-system-xray-customized-metrics] | Updated details about System X-Ray and the Developer Tools Options menu. |
| Dec. 14, 2016 | [Amazon Fire TV Development Framework Comparison][fire-tv-development-framework-comparison] | Detailed comparison between Web App Starter Kit for Fire TV (WASK) and Fire App Builder. |
| Nov 29, 2016 | [Notifications for Amazon Fire TV][notifications-for-amazon-fire-tv] | Updated to explain upcoming the Notification Center functionality that will be included in the Fire TV UI refresh release. |
| Nov 23, 2016 | [Specifying Your App's Installation Location][fire-tv-installation-location] | Update with emphasis on preferring external storage on app install. Also includes details for app installs across platforms. |
| Oct 25, 2016 | [Image Asset Guidelines for Appstore Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets) | Updated graphics and instruction for the Fire TV Assets section. Instead of generic diagrams, the graphics include real samples with more gradations shown in the content safe areas. |
| Oct 20, 2016 | [Getting Started Developing Apps and Games for Amazon Fire TV][getting-started-developing-apps-and-games-for-amazon-fire-tv]  | New sidebar navigation on the right with expandable/collapsible sections |
| Oct 2, 2016 | [Fire App Builder: A Starter Kit for Java-based Amazon Fire TV and Android Apps][fire-app-builder-overview] | Extensive documentation (more than 30 pages) about how to use Fire App Builder to create Java-based Android TV apps for Fire TV. |
| Sep 29, 2016 | [Identifying Fire TV Devices][identifying-amazon-fire-tv-devices] | Contains feature identifiers for Fire TV Stick (Generation 2) |
| Sep 28, 2016 | [Image Asset Guidelines for Appstore Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets) | Contains image asset guidelines for the Fire TV Assets section of the Images & Multimedia tab used during app submission. |
| Sep 28, 2016 | [Fire TV Device Specifications][device-and-platform-specifications] | Includes specifications for Fire TV Stick (Generation 2). Also contains content from Media Specifications on the same page (for consolidated and centralized information). Information is organized through navtabs.|
| Sep 28, 2016 | [Image Asset Guidelines for App Submission][asset-guidelines-for-app-submission] | New section that explains Fire TV image assets for upcoming Fire TV UI refresh.|
| Sep 16, 2016 | [Android TV Development versus Fire TV Development][amazon-fire-tv-differences-from-android-tv-development] | New page that explains what to account for when developing for Fire TV. |
| Sep 14, 2016 | [Getting Started Developing Apps and Games for Amazon Fire TV][getting-started-developing-apps-and-games-for-amazon-fire-tv] | Revision based on new site workflow |
| Sep 10, 2016 | [Using System X-Ray on Fire TV][fire-tv-system-xray] | New page that provides details for troubleshooting app issues through a System Xray overlay.|
| Sep 5, 2016 | [Getting the Advertising ID on Fire TV][fire-tv-advertising-id] | New page that provides details for getting the advertising ID. |
| Sep 1, 2016 | [Playing 4K Ultra HD Videos on Fire TV][fire-tv-4k-ultra-hd]. | New page with guidelines for developing 4K apps along with Extension support library for Fire TV. |
| Aug 30, 2016 | [Amazon Fire TV homepage](/solutions/devices/fire-tv) | The homepage was migrated to the new version of the Developer Portal site (which is rolling out incrementally). |
| Aug 5, 2016 | [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices].|  New methods for identifying Fire TV devices -- look for the feature instead of the model + manufacturer.|
| July 23, 2016 | [Fire TV Device Specifications][device-and-platform-specifications] | Updated with H.265 details, Widevine support, and other details. |
| July 22, 2016 | [Dolby Integration Guidelines][amazon-fire-tv-dolby-integration-guidelines] | New page that provides technical details for supporting Dolby audio. |
| July 1, 2016 | [Media Players][fire-tv-media-players] | Updated info about supported media players, with an emphasis on the Amazon port of Exoplayer. |
| June 10, 2016 | ["Handling Audio Focus with Voice Interactions"][managing-audio-focus#audiofocusvoice] | New section on how to handle audio focus when voice interactions start.|
| June 16, 2016 | ["Voice Search"][implementing-search-fire-tv#voicesearch] | New section on how voice search versus text search works, and what returns for both searches.|
| June 1, 2016 | Persistent sidebar navigation | Right sidebar appears throughout [Fire TV](/solutions/devices/fire-tv) docs to allow easier navigation within the Fire TV documentation. |


## Fire TV Blog Updates

You can follow the latest news about Fire TV by reading the Fire TV category on the blog.

<a href="https://developer.amazon.com/blogs/tag/Fire+TV"><button class="feedbackButton">Read the Fire TV blog</button></a>


{% include links.html %}
