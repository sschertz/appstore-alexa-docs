---
title: ライブストリーミングを構成する
permalink: fire-app-builder-live-stream-configuration.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

ユーザーがライブコンテンツを視聴してからコンテンツの詳細ページに戻ると、次のように [Watch Now] と [Watch from Beginning] という 2 つのボタンが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_resumeplayback" type="png" %}

しかし、ライブコンテンツの場合は、先頭への早戻しができないので、ユーザーがコンテンツに戻ってきた場合でも、表示するボタンは [Watch Now] だけで問題ありません。

* TOC
{:toc}

## ライブコンテンツの [Watch from Beginning] ボタンを削除する

ライブコンテンツの [Watch from Beginning] ボタンは、以下の 2 つの方法で削除できます。

*  [方法 1: Navigator.jsonを使ってボタンを削除する](#navigator): この方法は、対象のレシピ構成内のすべてのメディアがライブコンテンツであり、コンテンツをライブコンテンツとして識別するためのタグがフィードに含まれていない場合に使用します。
*  [方法 2: フィード内の値を照合してボタンを削除する](#feed): この方法は、フィード内の一部のコンテンツのみがライブストリーミングされ、コンテンツをライブコンテンツとして識別するためのタグがフィードに含まれている場合に使用します。

### 方法 1: Navigator.jsonを使ってボタンを削除する {#navigator}

1.  **Navigator.json**ファイル (app > assetsにあります) を開きます。
2.  ライブフィードのレシピが含まれている`categories`オブジェクト内に`recipeConfig`オブジェクトを追加し、`liveContent`パラメータを`true`に設定します。以下に、前後関係を表した例を示します。
    
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

これで、このレシピ (上記コードサンプル内の`LightCastAllContentsRecipe`) に関しては、ユーザーが 1 度視聴したメディアに戻ってきたときに [Watch from Beginning] ボタンが表示されなくなります。

### 方法 2: フィード内の値を照合してボタンを削除する {#feed}

メディアをライブコンテンツとして識別するためのタグがメディアフィードに含まれている場合は、[コンテンツレシピを構成する][fire-app-builder-set-up-recipes-content]ときに、これらのタグを識別するように`matchList`パラメータを構成できます。 

たとえば、フィード内のアイテムが、`<live>true</live>` タグを含む次のような内容だとします。

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

コンテンツレシピで、`live`タグをターゲットに指定して`live`と照合することができます。

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

これで、これらのコンテンツアイテムに関しては、ユーザーが 1 度視聴したメディアに戻ってきたときに [Watch from Beginning] ボタンが表示されなくなります。

`matchList`パラメータの詳細については、「[コンテンツレシピを構成する][fire-app-builder-set-up-recipes-content#matchlistparameter]」を参照してください。

{% include links.html %}