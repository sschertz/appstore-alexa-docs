---
title: Integrate Your Media into the Fire TV Catalog
permalink: fire-app-builder-catalog-integration.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

When users press the microphone button on a voice-enabled remote, regardless of where users are in Fire TV, this action initiates a global search using the Alexa cloud service (instead of the Leanback library in your app).

* TOC
{:toc}

## Media Requests Through Voice

Media requests through voice (such as saying the TV show you want to watch) always return content from the Fire TV catalog. The search within the catalog takes place in the cloud, not in your app. 

You can learn more about [how search is implemented on Fire TV here][implementing-search-fire-tv]. To learn more about Alexa voice capabilities on Fire TV from an end-user's perspective, see [Alexa on Fire TV](https://www.amazon.com/gp/help/customer/display.html?nodeId=201859020).

If you want your app's media to appear in these global search results, you must integrate your app's media into the Fire TV catalog. The Fire TV catalog contains an index of all media content on Fire TV. 

The process of getting your app's content into the catalog is called "catalog integration." Catalog integration involves regularly pushing your content to the catalog service (the catalog service does not read from a feed). This is a task a developer will need to configure &mdash; Fire App Builder does not do catalog integration for you. You can learn more about [Fire TV catalog integration here][integrating-your-catalog-with-fire-tv].)

When a search is initiated, Fire TV sends your app a broadcast for an intent. An intent (short for "intention") is a message for your app to perform a desired action. Your app must have an intent filter declared in its manifest file that listens for this intent and then acts on it. (You can learn about [intent filters](https://developer.android.com/guide/topics/manifest/manifest-intro.html#ifs) in Android's documentation.) Fire App Builder does have the code that allows your app to listen for the broadcast intents -- it just needs to be uncommented when you're ready to start listening for the intents. (See [Configure Your App to Listen for Broadcast Intents](#intents) below.

After you integrate your media into the Fire TV catalog and configure your app's manifest with intent filtering, if users already have your app installed, your app's content will appear directly in the search results. If a user doesn't have your app, a "More Ways to Watch" option appears for users to get your app and view the content.


## a. Integrate Your App's Media with Fire TV Catalog

For instructions getting your app's media into the Fire TV Catalog, see [Integrating Your Catalog with Fire TV][integrating-your-catalog-with-fire-tv].

The Fire App Builder project has already completed the steps described in [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]. You just need to uncomment some code in manifest files as described in the following section.

## b. Include a Content ID in Your Feed and Catalog Submission

Your catalog details must have a unique ID for each content item. This unique content ID must correspond with the content IDs in your media feed. If your media feed does not contain unique IDs for each media content item, you must add it. 

Additionally, your catalog details in the cloud and your media feed (as integrated into your app) must always be in sync.

## Configure Your App to Listen for Broadcast Intents {#intents}

To make your app listen for broadcast intents:

1.  In your app's folder in Android Studio, expand **manifests**, open **AndroidManifest.xml**. 
2.  Uncomment the "Launcher Integration intents" section:
    
    ```xml
    <!-- Launcher integration intents -->
    <!-- Uncomment the below intent filters to enable launcher integration -->
    
    <intent-filter>
        <action android:name="PLAY_CONTENT_FROM_LAUNCHER"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
    <intent-filter>
        <action android:name="SIGN_IN_FROM_LAUNCHER"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
    ```
    
3.  Go to **ContentBrowser > manifests** and open the **AndroidManifest.xml** file.
4.  Uncomment the launcher integration section:
    
    ```xml
    <!-- Uncomment the below receiver to enable launcher integration -->
    <receiver android:name="com.amazon.android.contentbrowser.helper.LauncherIntegrationBroadcastReceiver" >
        <intent-filter>
            <action android:name="com.amazon.device.REQUEST_CAPABILITIES" >
            </action>
        </intent-filter>
    </receiver>
    ```

## Testing Catalog Integration

After you have integrated your app with Fire TV’s Home Screen Launcher, you will need to validate your launcher integration before submitting your app to the Amazon Appstore. More details about launcher integration testing are available in [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]. Specifically, see the section "Step 6: Test Your Launcher Integration." To enable this testing for your unpublished app:

1.  Go to **ContentBrowser > java > com.amazon.android.contentbrowser > helper** and open **LauncherIntegrationManager.java**.
2.  Replace the value for `COM_AMAZON_TV_LAUNCHER` with `com.amazon.tv.integrationtestonly`:

    ```java
    //Replace COM_AMAZON_TV_LAUNCHER value with "com.amazon.tv.integrationtestonly" when testing
    // with integration test app.
    private static final String COM_AMAZON_TV_LAUNCHER = "com.amazon.tv.integrationtestonly";
    ```

{% include links.html %}
