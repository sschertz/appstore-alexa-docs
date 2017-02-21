---
title: Configure Live Streams
permalink: fire-app-builder-live-stream-configuration.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

If users watch live content and then return to the content details page, two buttons appear: "Watch Now" and "Watch from Beginning":

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_resumeplayback" type="png" %}

However, with live content, you only want the "Watch Now" button even when users return to the content, because live content can't be rewound to the beginning.

* TOC
{:toc}

## Removing the "Watch from Beginning" button for Live Content

To remove the "Watch from Beginning" button for live content, you have two options:

*  [Option 1: Remove the Button Through Navigator.json](#navigator): Use this approach if all the media within a specific recipe configuration is live content, and you don't have any tags within your feed that identify the content as live.
*  [Option 2: Remove the Button by Matching a Value in the Feed](#feed): Use this approach if only some of the content in your feed is live streamed, and your feed has tags that identify the content that is live.

### Option 1: Remove the Button Through Navigator.json {#navigator}

1.  Open the **Navigator.json** file (in app > assets).
2.  Within the `categories` object that contains recipes for your live feed, add a `recipeConfig` object with a `liveContent` parameter set to `true`. Here's an example that shows some context:
    
    <pre>
        {
          "categories": {
            "name": "Hardcoded Category Name"
          },
          "contents": {
            "dataLoader": "recipes/LightCastDataLoaderRecipe1.json",
            "dynamicParser": "recipes/LightCastAllContentsRecipe.json"
          },
          <span class="red">"recipeConfig": {
            "liveContent": true
          }</span>
        }
      ],
      "graph": {
        "com.amazon.android.tv.tenfoot.ui.activities.SplashActivity": {
          "verifyScreenAccess": false,
          "verifyNetworkConnection": true,
          "onAction": "CONTENT_SPLASH_SCREEN"
        },
        "com.amazon.android.tv.tenfoot.ui.activities.ContentBrowseActivity": {
    </pre>

Now for this recipe (`LightCastAllContentsRecipe` in the above code sample), the “Watch from Beginning” button won’t be shown when users return to the media after previously watching it.

### Option 2: Remove the Button by Matching a Value in the Feed {#feed}

If your media feed includes tags that identify media as live content, you can configure the `matchList` parameter to identify these tags when you [configure your Contents recipe][fire-app-builder-set-up-recipes-content]. 

For example, suppose an item in your feed looks like this, with the `<live>true</live>` tag:

<pre>
&lt;item&gt;
  &lt;id&gt;1&lt;/id&gt;
  &lt;title&gt;Nullamtus&lt;/title&gt;
  &lt;link&gt;http://www.developer.amazon.com/&lt;/link&gt;
  &lt;pubdate&gt;Wed, 14 Jan 2015 00:36:00 +0000&lt;/pubdate&gt;
  &lt;description&gt;Sed a sagittis urna, a fermentum ligula. In sagittis sagittis libero, ut tincidunt sapien egestas.&lt;/description&gt;
  &lt;image&gt;https://raw.githubusercontent.com/amzn/web-app-starter-kit-for-fire-tv/master/src/common/assets/images/l1.jpg&lt;/image&gt;
  &lt;category&gt;Lifestyle&lt;/category&gt;
  &lt;url&gt;http://example.com./myvideos/sample.mp4&lt;/url&gt;
  <span class="red">&lt;live&gt;true&lt;/live&gt;</span>
&lt;/item&gt;
</pre>

In your Content recipe, you can now target the `live` tag and match it to `live`:

<pre>
{
  "cooker": "DynamicParser",
  "format": "xml",
  "translator":"ContentTranslator",
  "model": "com.amazon.android.model.content.Content",
  "modelType": "array",
  "query": "rss/channel/item",
  "matchList": [
    "title/#text@title",
    "id/#text@id",
    "description/#text@description",
    "url/#text@url",
    "image/#text@cardImageUrl",
    "image/#text@backgroundImageUrl",
    <span class="red">"live/#text@live"</span>
  ]
}
</pre>

Now for these content items, the "Watch from Beginning" button won't be shown when users return to the media after previously watching it.

For more details on using the `matchList` parameter, see [Configure the Content Recipe][fire-app-builder-set-up-recipes-content#matchlistparameter].

{% include links.html %}