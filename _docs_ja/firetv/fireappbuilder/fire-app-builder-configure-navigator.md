---
title: Navigatorの構成の概要
permalink: fire-app-builder-configure-navigator.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

アプリのユーザーインターフェース要素のタイプ、要素が相互に通信する方法、アプリに関連付けるアクティビティ、レイアウトなどは、Navigator.jsonファイル (**app > assets**にあります) で定義します。

[レシピの設定][fire-app-builder-set-up-recipes-overview]が完了したら、その情報を使用するようにFire App Builder UIを構成する必要があります。これを行うには、事前にカスタマイズしたカテゴリレシピとコンテンツレシピを使用して、Navigator.jsonファイル内の`globalRecipes`オブジェクトを構成します。

* TOC
{:toc}

## DataLoaderRecipe

DataLoaderRecipeファイルは、メディアフィードURLをロードします。DataLoaderRecipeでロードできるメディアフィードURLは一度に 1 つのみです。そのため、メディアフィードURLが複数ある場合は、DataLoaderRecipeファイルを複数作成し、毎回同じカテゴリレシピとコンテンツレシピを参照する必要があります。この設定には多少手間がかかりますが、いったん設定した後は変更する必要がありません。

## 2 種類のフィード

Navigatorの構成方法は、フィードがトークンベースか否かによって異なります。次のいずれかを参照してください。

* [Navigatorの構成 -- トークンベースフィード](fire-app-builder-configure-navigator-token-feeds): トークンによってアクセスが制限されているメディアフィードでメディアの詳細を公開する場合、この手順を使用します。
* [Navigatorの構成 -- オープンフィード](fire-app-builder-configure-navigator-open-feeds): オープンで、アクセス制限のないメディアフィードでメディアの詳細を公開する場合、この手順を使用します。

{% include links.html %}
