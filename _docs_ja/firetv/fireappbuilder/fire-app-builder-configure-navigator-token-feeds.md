---
title: Navigatorの構成 -- トークンベースフィード
permalink: fire-app-builder-configure-navigator-token-feeds.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

コンテンツを表示するためにトークンが必要なフィードの場合は、以下のセクションの手順に従ってください。一方、オープンにアクセスできるフィードの場合は、「[Navigatorの構成 -- オープンフィード][fire-app-builder-configure-navigator-open-feeds]」を参照してください。Navigatorの構成の概要については、「[Navigatorの構成の概要][fire-app-builder-configure-navigator]」を参照してください。

* TOC
{:toc}

## トークンベースフィードのNavigator.jsonの構成 {#tokenbasednavigator}

トークンベースのメディアフィードの場合は、この手順を使用します。この手順では、以前に作成したカテゴリレシピとコンテンツレシピのファイルを使用してNavigator.jsonを構成します。

1.  **Navigator.json**ファイル (**app > assets**にあります) を開き、`globalRecipes`オブジェクトを探します。

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

2.  メディアファイルの数だけcategoriesとcontentsのペアを複製します。この例では、メディアフィードが 2 つあるため、categoriesオブジェクトとcontentsオブジェクトのセットが 2 つあります。たとえば、メディアフィードが 4 つある場合、`globalRecipes`オブジェクトは次のようになります。

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
    
    配列内の項目はカンマで区切ってください。 
    
3. まだ、LightCastDataLoaderRecipeファイルの名前を更新して、CategoriesRecipeとContentsRecipeの各インスタンスに合わせて番号を増分させていない場合は、増分させます (LightCastDataLoaderRecipe1.json、LightCastDataLoaderRecipe2.json、LightCastDataLoaderRecipe3.json、LightCastDataLoaderRecipe4.json)。これは、DataLoaderRecipeがURLを一度に 1 つしかロードできないためです。URLファイルにURLが 4 つある場合、すべてのフィードをロードするにはDataLoaderRecipeファイルが 4 つ必要です。
    
    {% include note.html content="必要に応じて、ファイル名をアプリを反映させた名前に変更することも、LightCastのままにしておくこともできます。"%}
    
5.  各LightCastDataLoaderRecipe.jsonファイルを開き、内容を次のものに置き換えます。
    
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
    
6.  **BasicTokenBasedUrlGeneratorConfig.json** (**assets > configurations**内) で使用したのと同じ値を使用して、各DataLoaderRecipeファイルの`base_url`と`token_generation_url`を構成します。ただし、LightCastDataLoaderRecipeごとに異なるメディアフィードURLをロードするため、それぞれの`base_url`には一意の値を使用します。

{% include_relative navigator_configuration_common.md %}

{% include links.html %}