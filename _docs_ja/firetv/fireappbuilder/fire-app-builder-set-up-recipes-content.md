---
title: コンテンツレシピをセットアップする
permalink: fire-app-builder-set-up-recipes-content.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

[カテゴリの構成][fire-app-builder-set-up-recipes-categories]では、メディアの一般的なグループを構成しました。この手順では、フィードのコンテンツ (タイトル、説明、画像、ビデオURLなど) をFire App Builderのコンテンツモデルにマップします。レシピ構成の概要については、「[レシピ構成の概要][fire-app-builder-set-up-recipes-overview]」を参照してください。

* TOC
{:toc}

## コンテンツレシピを構成する

1.  **LightCastContentsRecipe.json**ファイル (**app** > **assets** > **recipes**にあります) を開きます。
2.  次の表の説明に従って、ファイルの値を構成します。パラメータの詳細な説明が必要な場合は、表の下の各セクションを参照してください。
    
    <table class="grid">
    <colgroup>
    <col width="20%" />
    <col width="80%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>キー</th>
    <th>説明</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td markdown="1">`cooker`
    </td>
    <td markdown="1">{% include_relative recipe_cooker.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`format`
    </td>
    <td markdown="1">{% include_relative recipe_format.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`translator`
    </td>
    <td markdown="1">{% include_relative recipe_translator.md default_value="`ContentTranslator`" %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`model`
    </td>
    <td markdown="1">データのコンテンツモデルを指定します。コンテンツモデルは、コンテンツの構造を指定し、それをFire App Builder UIにマップするためのものです。これは、デフォルトの`com.amazon.android.model.content.Content`のままにしてください。
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`modelType`
    </td>
    <td markdown="1">{% include_relative recipe_modeltype.md %}
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`query`
    </td>
    <td markdown="1">{% include_relative recipe_query.md type="content" %} 
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`queryResultType`
    </td>
    <td markdown="1">{% include_relative recipe_queryresulttype.md %}
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`matchList`
    </td>
    <td markdown="1">{% include_relative recipe_matchlist.md %}
    </td>
    </tr>
    </tbody>
    </table>

### queryパラメータ {#queryparameter}

JSONフィードの構文は、XMLフィードとはかなり異なります。そのため、以降のセクションでは、この 2 つの形式を個別に説明します。

* [JSONフィード](#queryjson)
* [XMLフィード](#queryxml)

{% include tip.html content="このセクションで取り上げる`query`構文は、少し複雑に見えるかもしれませんが、Fire App Builderでは、どのようなフィード構造も使用でき、特定の順序や仕様 (メディアRSSなど) に制限されることがありません。このように柔軟性が高いため、フィード内の要素をターゲットに指定する際には高度なクエリ構文を使用する必要があります。" %}

#### JSONフィード {#queryjson}

Fire App Builderのサンプルアプリでは、`query`の値は`$[?(@.categories[0] in [$$par0$$])]`になっています。カテゴリレシピの`query`パラメータと同様、これは (大部分が) [Jayway JsonPath](https://github.com/jayway/JsonPath)構文です。この構文では、[Jayway JsonPathフィルター演算子](https://github.com/jayway/JsonPath#filter-operators)を使用して、位置 0 に 1 つ以上のアイテムがある`categories`配列内のアイテムを選択します。 

このクエリは、使用するフィード構文に合わせてカスタマイズする必要があります。そのため、前の手順に戻って、この構文についてもう少し詳しく見てみましょう。サンプルの[Lightcastフィード](http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos) (Fire App Builderのサンプルアプリで使用されるフィード) は、次のような構造になっています。

```json
[
  {
    "id": "136211",
    "title": "Legends Beach Resort, Negril Jamaica",
    "description": "Legends Beach Resort, Negril Jamaica",
    "duration": "538",
    "thumbURL": "http:\/\/l3.cdn01.net\/_thumbs\/0000136\/0136211\/0136211__009f.jpg",
    "imgURL": "http:\/\/l3.cdn01.net\/_thumbs\/0000136\/0136211\/0136211__009f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136211\/M3FA8J1MI.mp4?source=firetv&channel_id=13671",
    "categories": [
      "Jamaican Attractions"
    ],
    "channel_id": "13671"
  },
  {
    "id": "136216",
    "title": "Falmouth Jamaica Nature's Lullaby ",
    "description": "Falmouth Jamaica Nature's Lullaby",
    "duration": "1813",
    "thumbURL": "http:\/\/l4.cdn01.net\/_thumbs\/0000136\/0136216\/0136216__001f.jpg",
    "imgURL": "http:\/\/l4.cdn01.net\/_thumbs\/0000136\/0136216\/0136216__001f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136216\/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
    "categories": [
      "The Country Jamaica"
    ],
    "channel_id": "13672"
  },
  {
    "id": "136209",
    "title": "Rafting on the Rio Grande River, Jamaica",
    "description": "Rafting on the Rio Grande River, Jamaica",
    "duration": "660",
    "thumbURL": "http:\/\/l1.cdn01.net\/_thumbs\/0000136\/0136209\/0136209__010f.jpg",
    "imgURL": "http:\/\/l1.cdn01.net\/_thumbs\/0000136\/0136209\/0136209__010f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136209\/I9BDFF0IM.mp4?source=firetv&channel_id=13671",
    "categories": [
      "Jamaican Attractions"
    ],
    "channel_id": "13671"
  }
]
```

(これは完全なフィードではなく、繰り返しの構造部分を示しています。)

このフィードを[Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/)に挿入します。続いて、次のクエリを実行します。

```
$[?(@.categories in ["The Country Jamaica"]")]
```

このクエリにより、次の内容が返されます。

```json
[
   {
      "id" : "136216",
      "title" : "Falmouth Jamaica Nature's Lullaby ",
      "description" : "Falmouth Jamaica Nature's Lullaby",
      "duration" : "1813",
      "thumbURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "imgURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136216/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
      "categories" : [
         "The Country Jamaica"
      ],
      "channel_id" : "13672"
   }
]
```

このクエリ構文で取得される内容を次に示します。

| クエリ構文 | 照合する対象 |
|---|----|
| `$[` | ルートで開始し、名前のない配列を選択します。|
| `?(@.categories[0] in ["The Country Jamaica"]` | インデックス位置 `0` に `["The Country Jamaica"]` が含まれるすべての`categories`配列を取得するためのフィルターを作成します。|

Fire App Builderのサンプルアプリで使用されているクエリ構文には、1 つの相違点があります。`query`パラメータでは、`["The Country Jamaica"]` ではなく、`$$par0$$`が使用されます。

```
"query": "$[?(@.categories[0] in [$$par0$$])]"
```

`$$par0$$`は、Fire App Builderに定義されている変数で、クエリによって取得されたすべてのアイテムが格納されます。この点がJayway JsonPath構文と異なっています。このコードでは、実際には、カテゴリレシピの`keyDataType`によって、`$par0$$`変数にカテゴリが設定されます。

もう 1 つ例を見てみましょう。JSONの内容が次のようになっているとします。

```json
{
    "titles": {
        "video": [
            {
                "category": "reference",
                "publisher": "Jess",
                "title": "Video title 1",
                "price": 3.95
            },
            {
                "category": "history",
                "publisher": "John",
                "title": "Video title 2",
                "price": 2.99
            },
            {
                "publisher": "Dave",
                "title": "Video title 3",
                "price": 8.99
            },
            {
                "category": "science",
                "publisher": "Jess",
                "title": "Video title 4",
                "price": 3.99
            }
        ]
     }
}
```

カテゴリとして`science`が含まれた配列をすべて選択するには、次のクエリを使用します。

```
$.titles.video[?(@.category contains "science")]
```

これにより、次の内容が返されます。

```
[
   {
      "category" : "science",
      "publisher" : "Jess",
      "title" : "Video title 4",
      "price" : 3.99
   }
]
```

ただし、ここでは`category`が含まれた配列内の*すべての*アイテムが必要であるため、`@.category`に関する条件を削除します。

```
$.titles.video[?(@.category)]
```

これに`in [$$par0$$]`変数を追加します。

```
$.titles.video[?(@.category in [$$par0$$])]
```

要約を次に示します。

| クエリ構文 | 照合する対象 |
|---|----|
| `$.titles.video[` | `title`オブジェクトと`video`配列を選択します。|
| `?(@.category in [$$par0$$]` | `category`が含まれるすべてのアイテムに一致するよう配列をフィルタリングします。|

Jayway JsonPath Evaluatorでは「in `[$$par0$$]`」を追加しても解釈されません。この構文はJayway JsonPathにはなく、Fire App Builderに固有のものであるためです。

クエリは、使用するフィードや、コンテンツオブジェクトのマッチングに必要な構文によって異なります。たとえば、より複雑なクエリは次のようになります。

```
$.assets[?(@.type == 'movie.Container' && @.assetId in $$par0$$)]
```

このクエリでは、ルート (`$`) で開始し、1 つ目のディレクトリレベル (`.`) で`assets`という名前のオブジェクトを検索し、配列をフィルタリングして、`movie.Container`と等しく`assetId`要素が含まれた`type`オブジェクトを取得するように指定されています。 

#### XMLフィード {#queryxml}

フィードがXMLの場合は、Jayway JsonPathを使用する代わりに、[XPath式](http://www.w3schools.com/xsl/xpath_syntax.asp)を使用して、フィードの各要素をターゲットにする必要があります。XPathでは、XML文書が "ノード" と呼ばれるさまざまなアイテムを用いた構造に変換されます。XPath構文は各ノードの位置を特定するための構文です。 

Jayway JsonPath構文とは異なり、XMLフィードの構文は、はるかにシンプルです。

フィードの内容が次のようになっているとします。

```xml
<rss>
    <channel>
        <item>
        <title>Sample Title 1</title>
        <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
        <link>https://example.com/myshow/episodes/110</link>
        <author>Sample Author name</author>
        <category>Gadgets</category>
        </item>
        
        <item>
        <title>Sample Title 2</title>
        <pubDate>Mon, 24 Oct 2016 09:24:12 PDT</pubDate>
        <link>https://example.com/myshow/episodes/109</link>
        <author>Sample Author name</author>
        <category>Technology</category>
        </item>
    </channel>
</rss>
```

コンテンツに対する`query`は、次のようになります。

```
//item
```

XPathの場合、`//item`は、XML構造内の位置にかかわらず、`item`要素のすべてのインスタンスに一致します(XMLでは、`[$$par0$$]` は不要です)。

XPathクエリの作成方法の詳細については、「[XMLでクエリを実行する][fire-app-builder-querying-xml]」を参照してください。

### matchListパラメータ {#matchlistparameter}

`matchList`パラメータは、クエリの結果から特定のプロパティを選択して、Fire App Builderのコンテンツモデルにマップします。`matchList`で使用される構文は、特定の要素をターゲットにするカスタムのFire App Builder構文です。 

Fire App Builderのサンプルアプリの場合、コンテンツレシピの値は、タイトル、ID、説明、メディア、および画像をFire App Builderのコンテンツモデルにマップするプロパティマッピングの配列です。サンプルアプリの`matchlist`パラメータの記述内容を次に示します。

```json
[
    "title@title",
    "id@id",
    "description@description",
    "videoURL@url",
    "imgURL@cardImageUrl",
    "imgURL@backgroundImageUrl"
  ]
```

この構文の構成は次のとおりです。アンパサンド (`@`) の左側に、クエリの結果でターゲットにするプロパティを設定します (`title`など)。アンパサンド (`@`) の右側には、プロパティのマップ先となるFire App Builder要素を設定します (`title`など)。

次の表は、フィードのプロパティまたは要素をマップできるFire App Builderの要素を示しています。

<table class="grid">
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Fire App Builderでの名前</th>
<th>説明</th>
<th>必須か</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="1">`title`
</td>
<td markdown="1">ビデオのタイトル。
</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`id`
</td>
<td markdown="1">ビデオID。メディアオブジェクトを一意に識別するために使用されます。
</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`subtitle`
</td>
<td markdown="1">ビデオの字幕。
</td>
<td markdown="1">
省略可能
</td>
</tr>

<tr>
<td markdown="1">`description`
</td>
<td markdown="1">ビデオの説明。
</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`url`
</td>
<td markdown="1">ビデオへのリンク。
</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`cardImageUrl`
</td>
<td markdown="1">ホーム画面で他のメディアサムネイルの隣に表示される画像サムネイル。最適なサイズは、548px x 452pxです。詳細については、下の「[画像の解像度](#imageresolution)」セクションを参照してください。
</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`backgroundImageUrl`
</td>
<td markdown="1">メディアを選択したときに表示される大きな画像。最適なサイズは、1920px x 1080pxです。詳細については、下の「[画像の解像度](#imageresolution)」セクションを参照してください。</td>
<td markdown="1">
必須
</td>
</tr>

<tr>
<td markdown="1">`tags`
</td>
<td markdown="1">お勧めのコンテンツを関連付けるために使用されます。詳細については、「[お勧めのコンテンツ (タグを使用)](#tags)」を参照してください。
</td>
<td markdown="1">
省略可能
</td>
</tr>

<tr>
<td markdown="1">`live`
</td>
<td markdown="1">ライブストリームコンテンツを識別するために使用されます。ライブストリームコンテンツの場合、ユーザーが見終わってメディアに戻ったときに [Watch from Beginning] ボタンは表示されません。詳細については、「[ライブストリーミングを構成する][fire-app-builder-live-stream-configuration]」を参照してください。
</td>
<td markdown="1">
省略可能
</td>
</tr>
</tbody>
</table>

{% include warning.html content="Fire App Builderのコンテンツモデルでは、(最低限) フィードの要素をタイトル、ID、説明、URL、カード画像、および背景画像にマップする*必要があります*。画像が 1 つのみの場合は、カード画像と背景画像の両方を同じ画像にマップできます。" %}

`matchlist`パラメータの機能について理解を深めるために、Fire App Builderのサンプルアプリを詳しく見てみましょう。Fire App BuilderのLightCastメディアフィードでは、`query`パラメータの選択内容に基づいて、次の内容が返されます。

```json
[
   {
      "id" : "136211",
      "title" : "Legends Beach Resort, Negril Jamaica",
      "description" : "Legends Beach Resort, Negril Jamaica",
      "duration" : "538",
      "thumbURL" : "http://l3.cdn01.net/_thumbs/0000136/0136211/0136211__009f.jpg",
      "imgURL" : "http://l3.cdn01.net/_thumbs/0000136/0136211/0136211__009f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136211/M3FA8J1MI.mp4?source=firetv&channel_id=13671",
      "categories" : [
         "Jamaican Attractions"
      ],
      "channel_id" : "13671"
   },
   {
      "id" : "136216",
      "title" : "Falmouth Jamaica Nature's Lullaby ",
      "description" : "Falmouth Jamaica Nature's Lullaby",
      "duration" : "1813",
      "thumbURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "imgURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136216/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
      "categories" : [
         "The Country Jamaica"
      ],
      "channel_id" : "13672"
   },
   {
      "id" : "136209",
      "title" : "Rafting on the Rio Grande River, Jamaica",
      "description" : "Rafting on the Rio Grande River, Jamaica",
      "duration" : "660",
      "thumbURL" : "http://l1.cdn01.net/_thumbs/0000136/0136209/0136209__010f.jpg",
      "imgURL" : "http://l1.cdn01.net/_thumbs/0000136/0136209/0136209__010f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136209/I9BDFF0IM.mp4?source=firetv&channel_id=13671",
      "categories" : [
         "Jamaican Attractions"
      ],
      "channel_id" : "13671"
   }
]
```

`matchList`パラメータは、フィードのこれらのプロパティ名を、Fire App Builderのコンテンツモデルで使用される名前に変換します。

```json
[
    "title@title",
    "id@id",
    "description@description",
    "videoURL@url",
    "imgURL@cardImageUrl",
    "imgURL@backgroundImageUrl"
  ]
```

*  `title@title`によって`title`が`title`に変換されます
*  `id@id`によって`id`が`id`に変換されます
*  `description@description`によって`description`が`description`に変換されます
*  `videoURL@url`によって`videoURL` が`url`に変換されます
*  `imgURL@cardImageUrl`によって`imgURL`が`cardImageUrl`に変換されます
*  `imgURL@backgroundImageUrl`によって`imgURL`が`backgroundImageUrl`に変換されます

(この例では多くの名前が変換前後で同じですが、フィードによっては`videoTitle`や`videoSummary`などの語が含まれている場合があります。)

アンパサンド`@`の左側を、使用するフィードの用語に一致するようカスタマイズします。`@`の右側は、Fire App Builderのコンテンツモデルの用語に一致するようカスタマイズします。

使用するフィードによっては各要素が単純なキーと値のペアのリストになっていない場合があります。複数のレベルで入れ子になっている場合、`matchList`は、次のようになります。

```json
    "common/title@title",
    "assetId@id",
    "common/subtitle@subtitle",
    "common/description@description",
    "video/videoURL@url",
    "imgURLs/ImageSmall@cardImageUrl",
    "imgURLs/ImageLarge@backgroundImageUrl"
```

この構文では、各レベルの要素がスラッシュ (`/`) によって区切られています。たとえば、`common/title`は、`common`オブジェクトの内部を調べて`title`プロパティに対応させます。

XMLを使用する例を見てみましょう。XMLフィードの内容が次のようになっているとします。

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" … …>
  <channel>
    <title>Content Mix for US News-in-Pictures 9x16</title>
    <description>Screenfeed Content Server</description>
    <lastBuildDate>Mon, 08 Dec 2014 22:55:16 GMT</lastBuildDate>
<ttl>5</ttl>
 
    <item>
      <title>John Anderson ……</title>
      <guid isPermaLink="false">1</guid>
      <pubDate>Mon, 08 Dec 2014 22:55:16 GMT</pubDate>
      <category>News</category>
      <media:content url="http://samples.screenfeed.com/1.jpg" type="image/jpeg">
        <media:title type="plain">1080x1920 - English - with caption</media:title>
        <media:credit>Â© 2014 News Agency</media:credit>
        <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9xnRIN9CUGiTWNQBBrjOw-1080x1920h-1.jpg" />
      </media:content>
</item>
 
...

  </channel>
</rss>
```

コンテンツを取得するための`matchList`パラメータのクエリは次のようになります。

```json
  "matchList": [
    "title/#text@title",
    "guid/#text@id",
    "media:content/media:title/#text@description",
    "media:content/#attributes/url@url",
    "media:content/#attributes/url@cardImageUrl",
    "media:content/#attributes/url@backgroundImageUrl",
  ]
```

この場合、変換は次のように実行されます。

*  `title/#text@mTitle`によって`title/#text`が`title`に変換されます。
*  `guid/#text@mId`によって`guid/#text`が`id`に変換されます。
*  `media:content/media:title/#text@mDescription`によって`media:content/media:title/#text`が`description`に変換されます。
*  `media:content/#attributes/url@mUr`によって`media:content/#attributes/url`が`url`に変換されます。
*  `media:content/#attributes/url@mCardImageUrl`によって`media:content/#attributes/url`が`cardImageUrl`に変換されます。
*  `media:content/#attributes/url@mBackgroundImageUrl`によって`media:content/#attributes/url`が`backgroundImageUrl`に変換されます。

実際には、`#text`セレクターを使用している点のみが異なっています。`#text`セレクターによって要素内のテキストがターゲットに指定されています。`#text`構文は、カスタムのFire App Builder構文で、XPathの`text()` に相当します。

別の例を次に示します。フィードの内容が次のようになっているとします。

```xml
<rss>
    <channel>
        <item>
        <title>Sample Title 1</title>
        <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
        <link>https://example.com/myshow/episodes/110</link>
        <author>Sample Author name 1</author>
        <category>Technology</category>
        <category>Gadgets</category>
        </item>
        
        <item>
        <title>Sample Title 2</title>
        <pubDate>Wed, 23 Oct 2016 08:33:12 PDT</pubDate>
        <link>https://example.com/myshow/episodes/109</link>
        <author>Sample Author name 2</author>
        <category>News</category>
        </item>
    </channel>
</rss>
```

`matchlist`パラメータの内容は次のようになります。

```
"matchList": [
    "title/#text@title",
    "guid/#text@id",
    "itunes:subtitle/#text@description",
    "media:content/#attributes/url@url",
    "media:content/media:thumbnail/#attributes/url@cardImageUrl",
    "media:content/media:thumbnail/#attributes/url@backgroundImageUrl"
]
```

#### 画像の解像度 {#imageresolution}

アプリのメディアには、画像カードと背景画像という 2 つの画像を使用できます。この 2 種類の画像は異なる場所で使用され、各画像が使用されるコンテナも少し異なります。 

次のスクリーンショットは、[Content Home] 画面における 2 つの画像の違いを示しています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" caption="画像カードは、サムネイルのリストに表示されます。画像カードの 1 つを選択すると、画面右上の領域に大きい背景画像が表示されます。" %}

この 2 つの画像は、[Content Preview] 画面にも表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_imagesdemoscreen2" type="png" caption="この画面では、画像カードが少し大きく表示され、背景画像が画面の背景全体に表示されています。背景画像は、ダークグレイでオーバーレイされます。" %}

推奨の画像サイズ (幅x高さ) は次のとおりです。

* **画像カード**: 548px x 452px。もっと大きい画像も使用できますが、縮小されます。画像がトリミングされて 320px x 240pxのコンテナに格納される場合もあります。
* **背景画像**: 1920px x 1080px。もっと大きい画像も使用できますが、縮小されます。画像がトリミングされて 1120px x 800pxのコンテナに格納される場合もあります。

Fire App Builderによって画像がトリミングされる場合は、アスペクト比を維持するために、画像の両端がトリミングされます (中心がフォーカスされます)。

#### お勧めのコンテンツ (タグを使用) {#tags}

ビデオの下には [Recommended Content] セクションがあり、同じタグを持つ他のビデオが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_recommendedcontentdiagram" type="png" alt="お勧めのコンテンツ" %}

[アプリの [Recommended Content] セクション][fire-app-builder-customize-look-and-feel#recommendations]にコンテンツを追加するには、`matchList`パラメータでタグを対応させる必要があります。たとえば、次のように指定します。

```json
common/tags@tags
```

ここでは、`tags`要素が`common`要素の内部にあります。この構文によって`common/tags`が`tags`に変換されます。これにより、Fire App Builderは`tags`を読み取って、関連するメディアオブジェクトを表示できます。

Fire App BuilderのサンプルアプリのLightCastフィードには、タグが含まれていないことに注意してください。お勧めのコンテンツを表示したいが、フィードにタグが含まれていない場合は、フォールバックパラメータを`true`に設定してください。

(アプリの**assets**フォルダーにある) Navigator.jsonファイルには、`config`オブジェクトに`categoryDefaultRecommendation`という名前のプロパティが含まれています。

<pre>
  "config": {
    <span class="red">"showRecommendedContent": true</span>,
    "categoryDefaultRecommendation": true,
    "searchAlgo": "basic"
  }
</pre>

この`categoryDefaultRecommendation`を`true`に設定すると、Fire App Builderでは、(同じタグを持つコンテンツを取得する代わりに) 同じカテゴリの他のメディアアセットをお勧めのコンテンツとして使用します。

{% include note.html content="現時点では、多くのアイテムに同じタグが付けられている場合、(フィードのタグに基づいて行われる) お勧めコンテンツのマッチングにおいて、コンテンツが無制限にマッチングされます。これは既知の制限/バグです。"%}

[Recommended Content] セクションを非表示にするには、Navigator.jsonで`showRecommendedContent`をfalseに設定します。

## 次のステップ

アプリのメディアフィードのカテゴリとコンテンツを構成できたところで、続いては、フィードをアプリのUIに関連付ける必要があります。「[Navigatorの構成の概要][fire-app-builder-configure-navigator]」を参照してください。

{% include links.html %}