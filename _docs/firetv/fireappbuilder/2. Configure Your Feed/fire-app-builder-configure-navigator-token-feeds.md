---
title: Configure Navigator -- Token-based Feeds
permalink: fire-app-builder-configure-navigator-token-feeds.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

If your feed requires a token to view the content, follow the steps in the section below. In contrast, if your feed is openly accessible, see [Configure Navigator -- Open Feeds][fire-app-builder-configure-navigator-open-feeds]. For an overview in configuring Navigator, see [Navigator Configuration Overview][fire-app-builder-configure-navigator].

* TOC
{:toc}

## Configure Navigator.json for Token-based Feeds {#tokenbasednavigator}

Use these instructions if you have media feeds that are token-based. In this step, you will configure Navigator.json with the category and content recipe files you created earlier.

1.  Open the **Navigator.json** file (located in **app > assets**) and look for the `globalRecipes` object:

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

2.  Duplicate the categories and contents pairs for as many media feeds as you have. In this example, there are two media feeds, so there are two sets of categories and contents objects. Suppose you have 4 media feeds. Then your `globalRecipes` object would look like this:

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
    
    Remember to separate items in the array with commas. 
    
3.  If you haven't already done so, update the LightCastDataLoaderRecipe file names to increment each time (LightCastDataLoaderRecipe1.json, LightCastDataLoaderRecipe2.json, LightCastDataLoaderRecipe3.json, LightCastDataLoaderRecipe4.json) for each CategoriesRecipe and ContentsRecipe instance. This is because the DataLoaderRecipe can only load one URL at a time, so if you have 4 URLs in your URL file, you need 4 DataLoaderRecipe files to load all the feeds.
    
    {% include note.html content="Optionally, you can rename the files to a name that more closely reflects your app, or you can leave them as LightCast." %}
    
5.  Open each of your LightCastDataLoaderRecipe.json files and replace the contents with the following:
    
    ```json
    {
      "task" : "cache_data",
      "data_file_path" : "/",
      "url_generator" : {
          "base_url" : "http://yourcompany.com/mediafeed?id=$$token$$",
          "token_generation_url" : "http://yourcompany.com/url_to_generate_token"
      }
    }
    ```
    
6.  In each of your DataLoaderRecipe files, configure the `base_url` and `token_generation_url` with the same values you used in **BasicTokenBasedUrlGeneratorConfig.json** (inside **assets > configurations**). However, for each LightCastDataLoaderRecipe, load a different media feed URL, so that each `base_url` value is unique.

{% include_relative navigator_configuration_common.md %}

{% include links.html %}