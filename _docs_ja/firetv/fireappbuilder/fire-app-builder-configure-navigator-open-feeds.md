---
title: Navigatorの構成 -- オープンフィード
permalink: fire-app-builder-configure-navigator-open-feeds.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

オープンにアクセスできる (つまり、トークンが必要ない) フィードの場合は、以下のセクションの手順に従ってください。一方、トークンが必要なフィードの場合は、「[Navigatorの構成 -- トークンベースフィード][fire-app-builder-configure-navigator-token-feeds]」を参照してください。Navigatorの構成の概要については、「[Navigatorの構成の概要][fire-app-builder-configure-navigator]」を参照してください。

* TOC
{:toc}

## 非トークンベースフィードのNavigator.jsonの構成 {#filebasednavigator}

メディアにアクセスするのにトークンが必要ないフィードの場合は、この方法を使用します。 

1.  **Navigator.json**ファイル (**app > assets**にあります) を開きます。
2.  `globalRecipes`オブジェクトを探します。

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
    
3.  `categories`オブジェクトと`contents`オブジェクトの数をURLファイル内のメディアURLの数と一致させます。たとえば、メディアURLが 4 つある場合は、`categories`オブジェクトと`contents`オブジェクトのセットが 4 つと、それぞれに異なるDataLoaderRecipeが必要です。
    
    このシナリオでは、(「[メディアフィードをロードする][fire-app-builder-load-media-feed]」で構成した) URLファイルは次のようになっています。
    
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
    
    その結果、Navigator.json内の`globalRecipes`オブジェクトは次のようになります。
    
    
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
     
     {% include note.html content="Fire App Builderに用意されたサンプルアプリでは、URLファイルに 4 つのメディアURLが含まれていますが、DataLoaderRecipeファイルは 2 つしかありません。これは、すでに十分な数のビデオがあり、実際には 4 つあるメディアURLのうちの 2 つしか使用されないためです。" %}

5.  まだURLファイル内のメディアの行に合わせてDataLoaderRecipeファイルの番号を増分させていない場合は、増分させます。4 つのメディアURLがある場合は、DataLoaderRecipeの番号を 1 ずつ増分させます (LightCastDataLoaderRecipe1.json、LightCastDataLoaderRecipe2.json、LightCastDataLoaderRecipe3.json、LightCastDataLoaderRecipe4.json)。

    {% include note.html content="必要に応じて、ファイル名をアプリを反映させた名前に変更することも、LightCastのままにしておくこともできます。" %}
        
6.  必要に応じて、追加のDataLoaderRecipeファイルを作成します。そのためには、**LightCastDataLoaderRecipe1.json**ファイルを複製し、各ファイルのインデックス番号を増分させます。
    
    LightCastDataLoaderRecipe1.jsonは次のようになります。
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "0"
      }
    }
    ```
    
    LightCastDataLoaderRecipe2.jsonは次のようになります。
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "1"
      }
    }
    ```
    
    LightCastDataLoaderRecipe3.jsonは次のようになります。
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "2"
      }
    }
    ```
    
    LightCastDataLoaderRecipe4.jsonは次のようになります。
    
    ```
    {
      "task": "load_data",
      "url_generator": {
        "url_index": "3"
      }
    }
    ```
    
    これらの各DataLoaderRecipeファイルは、URLファイル (urlFile.json) の各行からデータをロードします。
    
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

{% include note.html content="このファイルの作成には多少手間がかかりますが、処理の仕組みは次のようになっています。Fire App Builderは、まずカテゴリを問い合わせ、次にコンテンツを問い合わせます。Fire App Builderは一度に 1 つのURLしか取得できないため、(Fire App Builderサンプルアプリのように) urlFile.jsonファイルに格納されている複数のURLでメディアフィードが構成されている場合は、URLごとに`categories`と`contents`オブジェクトを作成しておく必要があります。" %}

{% include_relative navigator_configuration_common.md %}

{% include links.html %}