---
title: Load Your Media Feed
permalink: fire-app-builder-load-media-feed.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

The heart of your app is the media feed. This feed contains your video content, including titles, descriptions, thumbnails, and other details for each media object. 

Since different media feeds have different structures (with different terms for the various properties or elements), Fire App Builder queries your media feed for the needed components and converts the queried result to a structure and terminology that aligns with Fire App Builder's content model. Before you write these queries (specifying them in [categories][fire-app-builder-set-up-recipes-categories] and [content][fire-app-builder-set-up-recipes-content] recipes), you must first load your media feed following the instructions here.

Two Fire App Builder components are used to load and configure the media feed: DataLoader and DynamicParser. The DataLoader module loads the data feed, and DynamicParser configures the structure and key values of the feed so that Fire App Builder can read it. Rather than working in Java, you configure both of these modules through JSON files. The Java code will read from the values in various JSON files.

* TOC
{:toc}

## Types of Feeds 

You can load the following types of feeds:

* [Token-based Feeds](#tokenbasedconfiguration)
* [Open Feeds](#filebasedconfiguration)

### Load Token-based Feeds {#tokenbasedconfiguration}

Use these instructions if you publish your media details in a web feed whose access is restricted by a token. 

1.  Open the **DataLoadManagerConfig.json** file (located in **app > assets > configurations**).
    
    {% include tip.html content="In Android Studio, instead of browsing folders, you can press **Shift** key twice and then type the file name to quickly find a file." %} 
    
2.  Leave the value for the **data_downloader.impl** option as is: `com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader`.
2.  If desired, update the options for these two properties:
    * `is_cache_manager_enabled`: Whether your feed is cached in the app. Caching your feeds speeds up screen loading with the media retrieved, but the cache will not reflect the latest updates to the feed until the data loader is updated or until the feed is expired. Options are `true` or `false`. Usually leave this as `true`.
    * `data_updater.duration`: The interval (in seconds) when the data loader refreshes your feed and retrieves the latest updates. When the data loader updates, the cache gets purged. The default is 2000 seconds, or 5.5 hours.
    
    {% include note.html content="Each time users start the app, your app's cache is automatically purged and the feed is refreshed." %}
    
3.  Open the **BasicHttpBasedDownloaderConfig.json** file (located in **app > assets > configurations**) and change the value from `com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator` to `com.amazon.dataloader.datadownloader.BasicTokenBasedUrlGenerator`:

    ```json
    {
      "url_generator_impl": "com.amazon.dataloader.datadownloader.BasicTokenBasedUrlGenerator"
    }
    ```
    
4.  Create a file inside **app > assets > configurations** called **BasicTokenBasedUrlGeneratorConfig.json**. Inside the file, create a JSON object that includes two key-value pairs as follows:

    ```json
    {
      "base_url" : "http://yourcompany.com/mediafeed?id=$$token$$",
      "token_generation_url" : "http://yourcompany.com/url_to_generate_token"
    }
    ```
        
5.  Customize the values for both `base_url` and `token_generation_url` with your company's actual values.

    The `base_url` is the URL to your media feed. The `token_generation_url` contains a link to a URL that generates the token to access the URL.

    In this example, the token is inserted into the URL with `$$token$$`. (Your media URL may insert the token in some other way. If so, adjust the placement of the `$$token$$` where it should appear.)

    The parameters from this BasicTokenBasedUrlGeneratorConfig.json file (`base_url` and `token_generation_url`) get passed to the `BasicTokenBasedUrlGenerator` class (located in **DataLoader > java > com.amazon.dataloader**). The `BasicTokenBasedUrlGenerator` class constructs a URL which is then consumed by the `BasicHttpBasedDataDownloader` class. The `BasicHttpBasedDataDownloader` class gets and retrieves the content from the URL.

### Load Open Feeds {#filebasedconfiguration}

Use these instructions if you publish your media details in a web feed that is open and unrestricted, that is, no token is required to access the media. 

1.  Open the **DataLoadManagerConfig.json** file (located in **app > assets > configurations**).
    
    {% include tip.html content="In Android Studio, instead of browsing folders, you can press **Shift** key twice and then type the file name to quickly find a file." %} 
    
2.  Ensure the value for the **data_downloader.impl** option is `com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader`:
    
    ```json
    {
      "data_downloader.impl": "com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader",
      "is_cache_manager_enabled": true,
      "data_updater.duration": 14400
    }
    ```
    
2.  Open the **BasicHttpBasedDownloaderConfig.json** file (located in **app > assets > configurations**) and ensure the value for `url_generator_impl` is `com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator`:
    
    ```json
    {
      "url_generator_impl" : "com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator"
    }
    ```
    
3.  Open the **BasicFileBasedUrlGeneratorConfig.json** file (located in **app > assets > configurations**) and verify the contents matches the code below. This file specifies the location of the `url_file` that will contain your media feed. To make things easiest, leave the file name as the default:

    ```json
    {
      "url_file" : "urlFile.json"
    }
    ```
    
6.  Open the **urlFile.json** (located in **app > assets**) and list your media feed URLs.
    
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
    
## Other Options for Loading the Feed

If neither of these options work for you to load your feed, you can write your own data loader by adding a class in the DataLoader folder that implements the Dataloader interface. Additionally, if your feed is generated from a REST endpoint, you will have to write your own data downloader.

## Live Feeds

If you have a live TV feed, there's nothing special you need to do to load it. You can load it the same way as your other media. However, when you configure options in Navigator.json, you can add a parameter that tells Fire App Builder to  remove the "Watch from Beginning" button for live streams. You can also identify live media through tags when the content recipe. See [Navigator Configuration Overview][fire-app-builder-configure-navigator] for more details.

## Loading a Static Feed That Is Packaged with Your App

For testing purposes, you may want to load a static feed that is packaged within your app. You wouldn't normally do this, since you wouldn't be able to update the feed without resubmitting a new version of your app. Hence this would be done for testing purposes only.

To load a static feed that is packaged inside your app:

1.  Open the **DataLoadManagerConfig.json** file (located in **app > assets > configurations**).
2.  Change the value for **data_downloader.impl** to `com.amazon.dataloader.datadownloader.BasicFileBasedDownloaderConfig`.
3.  Open the **BasicFileBasedDownloaderConfig.json** file (located in **app > assets > configurations**).
4.  If desired, you can rename the XML file:
    
    ```xml
    {
      "data_file_path": "GenericMediaData.xml"
    }
    ```
    
5.  Place your feed file into the **app > assets** folder.

## Next Steps

Loading the feed is just the first step. You have to help Fire App Builder identify the categories and content in your feed. See [Recipe Configuration Overview][fire-app-builder-set-up-recipes-overview].

{% include links.html %}
