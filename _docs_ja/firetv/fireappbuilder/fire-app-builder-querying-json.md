---
title: JSONでクエリを実行する
permalink: fire-app-builder-querying-json.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

[レシピのセットアップ][fire-app-builder-set-up-recipes-overview]では、`query`パラメータにJayway JsonPath構文を使用しました。JSONクエリ構文の詳細については、[Jayway JsonPath](https://github.com/jayway/JsonPath)を参照してください。

クエリ構文の記述方法について理解を深めるために、[Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/)を使用することもできます。各種の構文 (`$, @, *, .., .<name>, [?(<expression>)]`) でJSON内の特定の要素をターゲットにする具体的な方法については、「[Operators](https://github.com/jayway/JsonPath#operators)」セクションを参照してください。

以下のセクションでは、Jayway JsonPathクエリを作成する方法の例を示します。

* TOC
{:toc}

### JSON Pathクエリの例 1

この例では、Jayway JsonPathクエリについて詳しく説明します。 

1.  **urlFile.json** (app > assetsにあります) を開きます。
2.  最初のURLをコピーします。
    
    ```html
    http://www.lightcast.com/api/firetv/channels.php?app_id=257&app_key=0ojbgtfcsq12&action=channels_videos
    ```
    
3.  このURLをブラウザにコピーし、**Return**キーを押して内容を表示します。
4.  内容をコピーし、[jsonprettyprint.com](http://jsonprettyprint.com/)などのツールを使用してJSONを整形します。
5.  整形されたJSONをコピーし、[Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/)に貼り付けます。
6.  [**Go!**] ボックスに、すべてのカテゴリを取得するクエリを入力します。

    ```
    $..categories[*]
    ```
    
7.  [**Go!**] をクリックします。

    フィードのすべてのカテゴリが結果に表示されます。 
    
    ```json
    [
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Weddings",
       "Bahamas Weddings",
       "Bahamas Attractions",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Weddings",
       "Bahamas Islands",
       "Bahamas Underwater"
    ]
    ```
    
    このフィードの各メディアオブジェクトには、カテゴリのリストがあります。そのため、クエリを実行すると「Bahamas Islands」という結果が多数 (このカテゴリが含まれているメディアオブジェクトごとに 1 つずつ) 表示されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_jsonpath_evaluator" type="png" alt="JsonPath Evaluatorの例" caption="Jayway JsonPathクエリ構文を使用した、フィードのすべてのカテゴリの抽出" %}

    `$..categories[*]` という構文は、ルートディレクトリ (`$`) から開始し、すべてのサブディレクトリ (`..`) でキー`categories`を検索することを意味しています。このキーには配列 (`[]`) が含まれている必要がありますが、配列の内容は任意 (`*`) です。

    クエリを`$..title[*]` に変更すると何も一致しません。フィード内のtitleは配列を含んでいるキーではないためです。すべてのtitleを取得するには、`$..title`を使用します。
    
    Fire App BuilderコードのDynamicParserクラスによって、これらのカテゴリがマップに配置される際に重複が排除されます。

    {% include tip.html content="フィード内の特定の要素をターゲットにする際に使用できるさまざまなクエリの詳細については、JsonPathドキュメントの「[Path Examples](https://github.com/jayway/JsonPath#path-examples)」を参照してください。" %}
    
### JSON Pathクエリの例 2

メディアのフィード構造の別の例を以下に示します。


```json
{  
   "total":"13",
   "results":[  
      {  
         "id":"45454333",
         "title":"'Sample video title1",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo1.mp4",
         "pubDate":"Tue, 01 Oct 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage1.jpg",
         "media:duration":"1913",
         "description":"This is a sample description1",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo1.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail1.jpg",
         "media:keywords":"some, sample, keywords,random"
      },
      {  
         "id":"44545333",
         "title":"'Sample video title 2",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo2.mp4",
         "pubDate":"Tue, 01 Sep 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage2.jpg",
         "media:duration":"1945",
         "description":"This is a sample description 2",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo2.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail2.jpg",
         "media:keywords":"some, sample, keywords,random"
      },
           {  
         "id":"4543434333",
         "title":"'Sample video title3",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo3.mp4",
         "pubDate":"Tue, 01 Aug 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage3.jpg",
         "media:duration":"1955",
         "description":"This is a sample description",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo3.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail3.jpg",
         "media:keywords":"some, sample, keywords,random"
      }
      
   ]
}
```

すべてのアイテムのmedia:thumbnailの値を選択したいとします。この場合は、次の構文を使用できます。


```
$..results[*].media:thumbnail
```

この構文は、ルートから開始してすべてのアイテムを再帰的に検索したうえで、結果の配列内のすべてを照合して`media:thumbnail`要素を選択します。以下の結果が返されます。

```
[
   "http://example.com/assets/some/someimage1.jpg",
   "http://example.com/assets/some/someimage2.jpg",
   "http://example.com/assets/some/someimage3.jpg"
]
```

### JSON Pathクエリの例 3

この例では、Fire App Builderのフィードを使用する代わりに、より複雑なフィード構造から内容を取得します。

1.  以下のフィードをコピーし、[Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/)に挿入します。

    ```json
    {
    "brand": "myChannel",
      "version": "2.0",
      "creationDate": 1462590933,

        "containers": [
            {
                "type": "movie.Container",
                "subtype": "movies",
                "common": {
                    "title": "Movies"
                },
                "movies": [
                    {
                        "subtype": "movie",
                        "cat": "romance",
                        "availableDate": 1351631300,
                        "expirationDate": 1364764900,
                        "common": {
                            "authId": "1234",
                            "title": "Sample Movie Title",
                            "subtitle": "My sample subtitle for the movie",
                            "description": "This is a description for the movie. It describes the basic plot and story.",
                            "tags": [
                                "Some",
                                "Tags",
                                "Here"
                            ],
                            "promotionalText": "",
                            "theshareURL": "http://yourcompany.com/mymovieurl1",
                            "imageUrls": {
                                "1000x500_portal": "http://www.yourcompany.com/moviethumbl.jpg"
                            }
                        }
                    }
                ]
            },

            {
                "type": "movie.Container",
                "subtype": "movies",
                "common": {
                    "title": "Movies"
                },
                "movies": [
                    {
                        "subtype": "movie",
                        "cat": "adventure",
                        "availableDate": 1351631400,
                        "expirationDate": 1364765900,
                        "common": {
                            "authId": "5678",
                            "title": "Sample Movie Title 2",
                            "subtitle": "Another sample subtitle for the movie",
                            "description": "This is another sample movie title.",
                            "tags": [
                                "Some",
                                "Tags",
                                "Here"
                            ],
                            "promotionalText": "",
                            "theshareURL": "http://yourcompany.com/mymovieurl2",
                            "imageUrls": {
                                "1000x500_portal": "http://www.yourcompany.com/moviethumb2.jpg"
                            }
                        }
                    }
                ]
            }
        ]
    }
    ```

2.  [**Go!**] ボックスに、以下を入力します。

    ```
    $.containers[?(@.type == 'movie.Container')].movies[*].cat
    ```

    このクエリの意味は以下のとおりです。
    
    | クエリ構文 | 意味 |
    |----|----|
    | `$`| ルートレベルから開始する|
    |`.containers[`| ルートレベルで (特定の内容を持つ) 配列が含まれたキー「containers」のすべてのインスタンスに一致する|
    |`?(@.type == 'movie.Container')]`: タイプが`movie.Container`に等しいキー`type`に一致するフィルターを作成する|
    |`.movies[*]`| 配列`movies`内のすべての内容に一致する|
    |`.cat`| 配列`movies`内の`cat`に一致する|

3. [**Go!**] をクリックします。

    結果として以下のcatが取得されます。
    
    ```
    [
       "romance",
       "adventure"
    ]
    ```
    
    
## 例 4


フィード構造が以下のようなものであったとします。

```
[
  {
    "ChannelLang": "English",
    "VideoId": "239870",
    "VidTitle": "Top Stories",
    "videoKeywords": "news, stories, crime,",
    "videoDescription": "This is my video description.",
    "slug": "top-news-stories",
    "duration": 4318,
    "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
    "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
    "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
    "created": "2016-11-02 23:08:42",
    "vidcategory": "Adventure"
  },
  {
    "ChannelLang": "English",
    "VideoId": "239870",
    "VidTitle": "Top Stories",
    "videoKeywords": "news, stories, crime,",
    "videoDescription": "This is my video description.",
    "slug": "top-news-stories",
    "duration": 4318,
    "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
    "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
    "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
    "created": "2016-11-02 23:08:42",
    "vidcategory": "Adventure"
  },
   {
     "ChannelLang": "English",
     "VideoId": "239870",
     "VidTitle": "Top Stories",
     "videoKeywords": "news, stories, crime,",
     "videoDescription": "This is my video description.",
     "slug": "top-news-stories",
     "duration": 4318,
     "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
     "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
     "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
     "created": "2016-11-02 23:08:42",
     "vidcategory": "Adventure"
   }
]
```

カテゴリをターゲットにするクエリは以下のようになります。

```
$..vidcategory
```
 

{% include links.html %}
