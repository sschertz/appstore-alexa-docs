---
title: Take an App Tour
permalink: fire-app-builder-app-tour.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

After you have successfully [built the app][fire-app-builder-download-and-build], spend some time exploring the various screens. The following sections show what each screen in the the sample app in Fire App Builder.

* TOC
{:toc}

## Screens in the app

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_splash" type="jpg" alt="Splash screen" caption="<b>Splash screen.</b> This screen appears when you first launch the app and displays only until the app finishes loading (usually less than a second)." %}

{% include note.html content="You can completely customize this screen and any other screen that shows the \"Fire App Builder\" text. For example, in place of \"Fire App Builder\" you can substitute your own company name or design." %}

After the app loads, you see the Home screen.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="jpg" alt="Home screen" caption="<b>Home screen with the <code>ContentBrowseActivity</code></b>. This view arranges the videos in various categories or groups. When you view a channel, the first video in that channel group appears as the featured background image, with its title and description in the upper-left. This is the default layout." %}

With the Home screen, you have a couple of display options, depending on the activity you select. By default, the Home screen has the `ContentBrowseActivity` activity configured. However, you can also load a more compact view by associating this screen with the `FullContentBrowseActivity` activity instead:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fullbrowse" type="jpg" alt="Home with FullContentBrowseActivity" caption="<b>Home with <code>FullContentBrowseActivity</code></b>. With this activity, all the videos appear in a more compressed grid, with the categories listed on the left. None of the videos are superimposed as large featured images in the background." %}

When you select a video, the video is highlighted with the background image appearing in the upper-right:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_selectvideo" type="jpg" alt="Selected video." caption="<b>Home screen with video selected</b>. The selected video appears with the large image in the background." %}

When you click the video again, the Content Details screen appears with play buttons:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentdetails" type="jpg" alt="Content Details" caption="<b>Content Details</b>. This screen shows the details for a specific video, including both the title and description." %}

If the video description extends beyond the display width available, a modal appears to allow users to see the additional description content.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentmoredetails" type="jpg" alt="More details" caption="<b>Content Details screen</b>. If the description doesn't fit within the alotted space on the Content Details screen, users can expand it to read more." %}

When you play media, the Renderer screen appears.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrenderer" type="jpg" alt="Renderer" caption="<b>Renderer</b>. This screen appears when you play media." %}

When the controls display on the video, Recommended Content appears below the controls below a dim overlay. If you click the down arrow on your remote, the recommended content shifts into prominent view without the dim overlay.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrecexpanded" type="jpg" alt="Recommended Content" caption="Recommended Content selected and displayed" %}

If you've already played the media, a different set of controls appears below the video when you view the content details.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_resumeplayback" type="jpg" alt="Resuming playback." caption="<b>Resuming playback on the Details</b>. If you stopped watching a video part-way through, instead of a WATCH NOW button, you see RESUME PLAYBACK and WATCH FROM BEGINNING buttons." %}

To search for a video, select the search button on the home screen. When you do, the Search screen appears and allows you to type a keyword. The search will match the keyword against the title and description elements.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_search" type="jpg" alt="Search" caption="<b>Search</b>. This screen allows users to search for media." %}

The search results screen lists the results as media cards in a grid:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_searchresultsmultiple" type="jpg" alt="Search" caption="<b>Search</b>. The search results appear as thumbnails below the search bar. Here the word <i>bay</i> matches a number of different videos." %}

## Activities Performed with Each Screen

Activities are the various functions the app can do. Each activity invokes a different screen. Fire App Builder has six available activities:

* `ContentBrowseActivity`
* `ContentDetailsActivity`
* `ContentSearchActivity`
* `FullContentBrowseActivity`
* `SplashActivity`
* `VerticalContentGridActivity`

(Both `ContentBrowseActivity` and `FullContentBrowseActivity` can be used for the Home screen.)

Each activity in Fire App Builder uses a different screen. The screen used by each activity is configured through the Navigator.json file (located in **app > assets**). The `graph` object (shown below) from Navigator.json contains key-value pairs that associate the activity with the screen and other properties:

```json
"graph": {
    "com.amazon.android.tv.tenfoot.ui.activities.SplashActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_SPLASH_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.FullContentBrowseActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_HOME_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.ContentDetailsActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_DETAILS_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.ContentSearchActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_SEARCH_SCREEN"
    },
    "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
  }
```

For example, the `SplashActivity` displays the Splash screen. The `ContentBrowseActivity` displays the Home screen, and so on.

You can associate any activity with any screen, but the only activity that makes sense to switch up is the `ContentBrowseActivity`. You can replace this activity with `FullContentBrowseActivity` to provide the more compact Home screen layout described earlier.

The other properties for each activity in this code are as follows:

| Activity property | Description |
|--------|------------|
| `verifyScreenAccess` | Require the user to authenticate to view the screen. Set this to `true` if you want to require users to log in prior to viewing content. Mostly you would verify screen access for the Content Renderer screen only, so that users can get a sense of the media first and feel more enticement to log in. |
| `verifyNetworkConnection` | Require a network connection to show the screen. (If the page contains only settings and no online media, you could set this to `false`. But for most screens, leave this at `true`. ) |
| `onAction` | The action to perform (such as display a certain screen) when the activity runs. |

The AndroidManifest.xml file initiates the `SplashActivity` activity when the app starts:

```xml
<activity
    android:name="com.amazon.android.tv.tenfoot.ui.activities.SplashActivity"
    android:screenOrientation="landscape">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
        <category android:name="android.intent.category.LEANBACK_LAUNCHER" />
    </intent-filter>
</activity>
```

This `SplashActivity` activity loads the Splash screen.

Beyond these activities and screens, you can also write your own activity and associate it with your own screen. (Details on how to make this advanced customization are beyond the scope of this documentation.)

## Explore Fire App Builder's Contents

Before you start creating your app, take a few minutes to familiarize yourself with the various libraries, modules, and components available in Fire App Builder.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_components" type="png" caption="The available components in Fire App Builder" %}

The following table briefly describes each component.



<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Folders</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AMZNMediaPlayerComponent</td>
      <td>Used to play streaming media.</td>
    </tr>
    <tr>
      <td>AdsInterface</td>
      <td>An interface for ads.</td>
    </tr>
    <tr>
      <td>AmazonInAppPurchaseComponent</td>
      <td>Component that implements the Purchase Interface for Amazon In-app purchasing.</td>
    </tr>
    <tr>
      <td>AnalyticsInterface</td>
      <td>An interface for analytics.</td>
    </tr>
    <tr>
      <td>AuthInterface</td>
      <td>An interface for authorization.</td>
    </tr>
    <tr>
      <td>ContentBrowser</td>
      <td>Controller (from the <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller">Model-view-controller</a> architectural pattern) that allows users to browse content within the app. It fetches the feed and controls flows, recipes, and configurations.</td>
    </tr>
    <tr>
      <td>ContentModel</td>
      <td>Model (from the <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller">Model-view-controller</a> architectural pattern) that defines the method for storing data that the browser will use to render.</td>
    </tr>
    <tr>
      <td>DataLoader</td>
      <td>Reusable module to load data from the network. Commonly used for loading the feed.</td>
    </tr>
    <tr>
      <td>DynamicParser</td>
      <td>Configurable module to parse the feed and populate the model.</td>
    </tr>
    <tr>
      <td>FacebookAuthComponent</td>
      <td>Component that implements the Auth Interface for Facebook authorization.</td>
    </tr>
    <tr>
      <td>FlurryAnalyticsComponent</td>
      <td>Component that implements the Analytics interface for Flurry analytics.</td>
    </tr>
    <tr>
      <td>ModuleInterface</td>
      <td>The core code that makes different components modular in the Fire App Builder framework.</td>
    </tr>
    <tr>
      <td>PassThroughAdsComponent</td>
      <td>Dummy implementation for the ads interface, used by Fire App Builder. If you don’t have an ad implementation in your app, you can use this as base code to start your own ads module.</td>
    </tr>
    <tr>
      <td>PurchaseInterface</td>
      <td>An interface for setting up payments.</td>
    </tr>
    <tr>
      <td>TVUIComponent</td>
      <td>Contains the TV UI code, which is based on the Leanback Support Library. Also contains the classes for the activities.</td>
    </tr>
    <tr>
      <td>UAMP</td>
      <td>Universal Android Media Player. Amazon Media Player builds on top of UAMP to extend it with additional features.</td>
    </tr>
    <tr>
      <td>Utils</td>
      <td>Contains reusable Java classes, including security classes used to encrypt and decrypt keys used with some components (such as Facebook Authorization, Adobe Pass, and Flurry Analytics components).</td>
    </tr>
    <tr>
      <td>Application</td>
      <td markdown="span">A sample app that uses the various libraries and components in the Fire App Builder framework. This is the app that you customize (see [Download Fire App Builder and Build an App][fire-app-builder-download-and-build#customize]).</td>
    </tr>
  </tbody>
</table>
 
{% include note.html content="When you open Fire App Builder in Android Studio, the \"Android\" view shows this folder as \"app\" because it's your working app. If you switch over to the \"Project\" view, you'll see that the app folder is actually named \"Application.\"" %}

## Subfolder Contents

Within each folder in Fire App Builder, you usually see the same pattern of subfolders. The following table describes each subfolder:

<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Subfolder</th>
      <th>Contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>build</td>
      <td>This subfolder, required by Android, is auto-generated when the project builds. Do not edit files in the build folder.</td>
    </tr>
    <tr>
      <td>libs</td>
      <td>If this subfolder is included, it contains external libraries required by a component or other external service. Do not edit files in this folder. Other times the libraries required by a component are referenced as dependencies in the build.gradle file and retrieved when the project builds.</td>
    </tr>
    <tr>
      <td>src</td>
      <td>Contains the actual code for the component or function. The res subfolder contains resources for the component, which you will frequently work with if using the component. When you edit files, you will primarily edit files that appear within the res subfolder.</td>
    </tr>
    <tr>
      <td>test</td>
      <td>Unit test files. The content in test folders mirrors the content in the main folder but is meant for writing unit tests. Unit tests don’t require any Android dependencies — you can run them on your own machine without a Fire TV device. You usually don’t need to do anything in this folder.</td>
    </tr>
    <tr>
      <td>androidTest</td>
      <td>Tests that require an Android dependency. These tests require a Fire TV device to run the code. You usually don’t need to do anything in this folder.</td>
    </tr>
  </tbody>
</table>

## Configurable JSON Files Overview

The basic app in Fire App Builder requires almost no Java coding. Instead, you configure the app through various JSON files that contain simple key-value pairs that allow you to specify the app options you want. The following are the JSON files you can configure.

<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>JSON or XML files to configure</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Navigator.json</td>
      <td>app &gt; assets</td>
    </tr>
    <tr>
      <td>BasicFileBasedUrlGeneratorConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>BasicHttpBasedDownloaderConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>DataLoadManagerConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>LightCastCategoriesRecipe.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastContentsRecipe.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastDataLoaderRecipe1.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastDataLoaderRecipe2.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>custom.xml</td>
      <td>app &gt; res &gt; values</td>
    </tr>
  </tbody>
</table>
(Don't worry about configuring the JSON or XML files now. The files are listed here simply to introduce you to upcoming the configuration tasks. The point is that you can configure your app by merely adjusting JSON or XML files instead of doing Java programming.)

## Next Steps

Now that you have a good feel for the basic functionality, libraries, and components in Fire App Builder, it's time to start customizing it with your own content. See [Load Your Media Feed][fire-app-builder-load-media-feed].

{% include links.html %}
