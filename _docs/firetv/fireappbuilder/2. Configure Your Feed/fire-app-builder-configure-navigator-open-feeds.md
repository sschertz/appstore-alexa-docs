---
title: Configure Navigator -- Open Feeds
permalink: fire-app-builder-configure-navigator-open-feeds.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

If your feed is openly accessible (that is, no tokens are required), follow the steps in the section below. In contrast, if your feed requires tokens, see [Configure Navigator -- Token-based Feeds][fire-app-builder-configure-navigator-token-feeds] instead. For an overview to configuring Navigator, see [Navigator Configuration Overview][fire-app-builder-configure-navigator].

* TOC
{:toc}

## Configure Navigator.json for Non-token-based Feeds {#filebasednavigator}

Use this approach for feeds that don't require tokens to access the media. 

1.  Open the **Navigator.json** file (located in **app > assets**).
2.  Look for the `globalRecipes` object:

    ```json
    "globalRecipes": [
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe1.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe1.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        },
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe2.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe2.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        }
    ]
    ```
    
3.  Make the number of `categories` and `contents` objects match the number of media URLs in your URL file. For example, if you have 4 media URLs, you would need four sets of these `categories` and `contents` objects and a different DataLoaderRecipe for each.
    
    In this scenario, your URL file (which you configured in [Load Your Media Feed][fire-app-builder-load-media-feed]) might look like this:
    
    ```json
    {
      "urls": [
        "http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=249&app_key=gtn89uj3dsw&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=267&app_key=6tgbfr4edc2x&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=273&app_key=u8jnsaq2rfgy&action=channels_videos"
      ]
    }
    ```
    
    As a result, your `globalRecipes` object in Navigator.json would look as follows:
    
    
    ```json
    "globalRecipes": [
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe1.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe1.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        },
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe2.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe2.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        },
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe3.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe3.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        },
        {
          "categories" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe4.json",
            "dynamicParser" : "recipes/LightCastCategoriesRecipe.json"
          },
          "contents" : {
            "dataLoader" : "recipes/LightCastDataLoaderRecipe4.json",
            "dynamicParser" : "recipes/LightCastContentsRecipe.json"
          }
        }
    ]
    ```
     
     {% include note.html content="Although the sample app in Fire App Builder has 4 media URLs in the URL file, you see only two DataLoaderRecipe files. This is because only 2 of the 4 media URLs are actually used &mdash; there are more than enough videos already." %}

5.  If you haven't already done so, increment the number in the DataLoaderRecipe files for each line of media in your URL file. If you have 4 media URLs, you would increment the DataLoaderRecipe by one each time (LightCastDataLoaderRecipe1.json, LightCastDataLoaderRecipe1.json, LightCastDataLoaderRecipe3.json, and LightCastDataLoaderRecipe4.json).

    {% include note.html content="Optionally, you can rename the files to a name that more closely reflects your app, or you can leave them as LightCast." %}
        
6.  As needed, create the additional DataLoaderRecipe files by duplicating the **LightCastDataLoaderRecipe1.json** file and incrementing the index number for each file:
    
    LightCastDataLoaderRecipe1.json would look like this:
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "0"
      }
    }
    ```
    
    LightCastDataLoaderRecipe2.json would look like this:
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "1"
      }
    }
    ```
    
    LightCastDataLoaderRecipe3.json would look like this:
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "2"
      }
    }
    ```
    
    LightCastDataLoaderRecipe4.json would look like this:
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "3"
      }
    }
    ```
    
    Each of these DataLoaderRecipe files will load the data from each line of your URL file (urlFile.json):
    
    ```json
    {
      "urls": [
        "http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=249&app_key=gtn89uj3dsw&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=267&app_key=6tgbfr4edc2x&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=273&app_key=u8jnsaq2rfgy&action=channels_videos"
      ]
    }
    ```

{% include note.html content="The file construction is rather manual here, but here's what's happening. Fire App Builder will first query for the categories, and then the contents. Fire App Builder can only get one URL at a time, so if your media feed consists of multiple URLs stored in the urlFile.json file (as is the case with the Fire App Builder sample app), you'll need to repeat the `categories` and `contents` objects for each URL." %}

{% include_relative navigator_configuration_common.md %}

{% include links.html %}