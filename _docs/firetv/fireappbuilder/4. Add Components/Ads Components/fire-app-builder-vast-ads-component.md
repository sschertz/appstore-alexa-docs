---
title: VAST Ads Component
permalink: fire-app-builder-vast-ads-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

<style>
table.small {
max-width: 300px;
}
</style>


Fire App Builder supports the VAST ads template (Video Ad Serving Template). VAST is a standard protocol that supports different video advertisers. The Interactive Advertising Bureau (IAB) describes VAST as follows:

>VAST is a Video Ad Serving Template for structuring ad tags that serve ads to video players. Using an XML schema, VAST transfers important metadata about an ad from the ad server to a video player. Initially launched in 2008, VAST has since played an important role in the growth of the digital video marketplace. [Digital Video Ad Serving Template (VAST) 4.0](http://www.iab.com/guidelines/digital-video-ad-serving-template-vast-4-0/)

If you want to show DoubleClick ads in your app, you can do so through the VAST Ads Component. However, note that Fire App Builder supports only a subset of VAST features.

* TOC
{:toc}

## Unsupported VAST Features

The Fire App Builder integration of VAST does not include support for the entire VAST specification. Additionally, the VAST integration in Fire App Builder is not officially certified by the IAB. The following VAST features are not supported in Fire App Builder:

*  Ad types other than "Linear Video Ad"
*  Any interactions within the ad
*  Click, pause, resume, or close actions within the ad
*  302 redirects

{% include note.html content="Linear ads can appear before, during, or after the video plays. However, in the current VastAdsComponent in Fire App Builder, ads can only appear before the video plays (preroll ads)." %}

## Tracking Events

During the Linear Video Ad playback, the component triggers events for the following:

*  Impressions
*  Errors
*  Start (25%, mid, 75%, and complete)

The following table lists the component's specific support for various events:

<table class="grid">
  <thead>
    <tr>
      <th>Tracking Events</th>
      <th>Supported</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="highlighter-rouge">creativeView</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">start</code></td>
      <td>Yes</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">midpoint</code></td>
      <td>Yes</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">firstQuartile</code></td>
      <td>Yes</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">thirdQuartile</code></td>
      <td>Yes</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">complete</code></td>
      <td>Yes</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">mute</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">unmute</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">pause</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">rewind</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">resume</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">fullscreen</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">expand</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">collapse</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">acceptInvitation</code></td>
      <td>No</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">close</code></td>
      <td>No</td>
    </tr>
  </tbody>
</table>

## The User Experience

After the user clicks the Watch Now button, the VAST ads template serves up ads for a short period of time.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_vastadsdisplay" type="png" caption="The VAST ads template serves up video before the user's selected media starts to play. The screenshot here is just ad filler that would be replaced by a real ad when you configure VAST ads." %}

After the video ads, the user's selected media begins to play.

## Configure VAST Ads

To configure VAST Ads:

1.  Load the VAST ads component into your app. See the [Load a Component in Your App][fire-app-builder-load-a-component] for details about how to load a component into your app.
2.  Remove any other ad components that are loaded in your app (such as FreeWheelAdsCompnent or PassThroughAdsComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.
    
    {% include_relative componentnote_ads.html %}
    
2.  Go to **VastAdsComponent > res > values** and open **strings.xml**. Copy the `vast_preroll_tag` string and paste it into your own app's **custom.xml** file (inside res > values):
    
    ```xml
    <string name="vast_preroll_tag">"https://pubads.g.doubleclick.net/gampad/ads?sz=640x480&amp;iu=/124319096/external/single_ad_samples&amp;ciu_szs=300x250&amp;impl=s&amp;gdfp_req=1&amp;env=vp&amp;output=vast&amp;unviewed_position_start=1&amp;cust_params=deployment%3Ddevsite%26sample_ct%3Dlinear&amp;correlator="</string>
    ```
    
3.  Customize value for the `vast_preroll_tag` string with your own VAST ads tag.

    Note that in your VAST tag, the following characters must be encoded: 

    <table class="small">
      <thead>
        <tr>
          <th>Character</th>
          <th>Encoding</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code class="highlighter-rouge">"</code></td>
          <td><code class="highlighter-rouge">&amp;quot;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">'</code></td>
          <td><code class="highlighter-rouge">&amp;apos;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&lt;</code></td>
          <td><code class="highlighter-rouge">&amp;lt;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&gt;</code></td>
          <td><code class="highlighter-rouge">&amp;gt;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&amp;</code></td>
          <td><code class="highlighter-rouge">&amp;amp;</code></td>
        </tr>
      </tbody>
    </table>

{% include links.html %}

