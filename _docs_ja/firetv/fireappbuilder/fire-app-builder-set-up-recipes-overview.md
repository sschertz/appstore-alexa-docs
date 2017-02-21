---
title: レシピ構成の概要
permalink: fire-app-builder-set-up-recipes-overview.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

[メディアフィードをロード][fire-app-builder-load-media-feed]したら、フィードからメディアのカテゴリとコンテンツを取得する際に使用される 2 つのレシピを構成する必要があります。"レシピ" とは、アプリの作成時にFire App Builderで使用される設定が (キーと値の形式で) 記述されたシンプルなファイルです。構成する必要のある 2 つのレシピは次のとおりです。

*  **[カテゴリレシピ][fire-app-builder-set-up-recipes-categories]**: メディアをグループ化するための各種のコンテナを提供します。
*  **[コンテンツレシピ][fire-app-builder-set-up-recipes-categories]**: メディアフィードのプロパティ (タイトル、説明、ビデオURL、画像など) をFire App Builderのコンテンツモデルにマップします。

デフォルトのLightcastフィードを使用したホーム画面を見ると、メディアが複数の行に表示されていることがわかります。デフォルトのホーム画面では、"Jamaica Attractions" や "The Country Jamaica" などのグループでメディアがグループ化されています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="ホーム画面の代替" caption="メディアをグループ化するカテゴリ。" %}

通常は、メディアフィードごとに構造が異なります (たとえば、"categories" の代わりに "channels" 、"image" の代わりに "img" が使用されていることもあれば、階層や構造が異なっている場合もあります)。そのため、使用するフィードのカテゴリとコンテンツをターゲットにするクエリ構文を作成する必要があります。 

フィードがJSONの場合は、[Jayway JsonPath構文](https://github.com/jayway/JsonPath)を使用してクエリを作成します。フィードがXMLの場合は、[XPath構文](http://www.w3schools.com/xsl/xpath_syntax.asp)を使用してクエリを作成します。

カテゴリとコンテンツに対してクエリを実行した後、セレクターを適用して、結果からカテゴリまたはコンテンツのテキストを取得します。このセレクターは、`matchList`というFire App Builderのカスタム構文で、Jayway JsonPath式やXPath式は使用されません。 

## 次のステップ

「[カテゴリレシピをセットアップする][fire-app-builder-set-up-recipes-categories]」を参照して、フィードのカテゴリの特定を始めてください。Fire App Builderでは、これらのカテゴリに基づいて、メディアをグループ化します。

{% include links.html %}