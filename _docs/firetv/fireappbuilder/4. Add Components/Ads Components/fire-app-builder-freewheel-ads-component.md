---
title:  Freewheel Ads Component
permalink: fire-app-builder-freewheel-ads-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

You can implement FreeWheel video ads in the app you build with Fire App Builder. To learn more about Freewheel, see [Freewheel.tv](http://freewheel.tv/). Both preroll and midroll ads are supported in Fire App Builder.

* TOC
{:toc}

## The User Experience

Before media begins to play on a Content Renderer screen, the Freewheel video ads play.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_freewheeladdisplay" type="png" caption="FreeWheel Ads display. (This screenshot shows the filler ads track.)" %}

After the video ads end, the media that the user selected starts playing.

## Configure Freewheel

1.  Load the Freewheel component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component] for details about how to load a component into your app.

2.  Remove any other ads components that are loaded in your app (such as VastAdsComponent or PassThroughAdsComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.    
    
    {% include_relative componentnote_ads.html %}
    
2.  Go to **FreeWheelAdsComponent > java > com.amazon.ads.android.freewheel** and open the **FreeWheelAds.java** file.

    {% include note.html content="Unlike with other components, the values you must customize for the FreeWheelAdsComponent aren't extracted out into an XML file. (The extraction will be completed in an upcoming release.)" %}

3.  Customize the values for the following four strings:

    ```xml
    /**
     * FreeWheel server url.
    */
    private String mAdUrl = "http://demo.v.fwmrm.net/";

    /**
     * FreeWheel network id.
    */
    private int mNetworkId = 90750;

    /**
     * FreeWheel profile.
    */
    private String mProfile = "3pqa_android";

    /**
     * FreeWheel site section.
    */
    private String mSiteSectionId = "3pqa_section_nocbp";
    ```

   You get these values through your FreeWheel account.

{% include links.html %}
