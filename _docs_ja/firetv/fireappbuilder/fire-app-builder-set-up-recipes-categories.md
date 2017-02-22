---
タイトル: カテゴリレシピをセットアップする
permalink: fire-app-builder-set-up-recipes-categories.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

ここでは、フィードにカテゴリを問い合わせる手順を説明します。カテゴリを使用すると、メディアをさまざまなコレクションやチャネルにグループ化できます。カテゴリでメディアを整理すれば、すべてのビデオを 1 つの長いリストで保持せずに済みます。レシピ構成の全般的な概要については、「[レシピ構成の概要][fire-app-builder-set-up-recipes-overview]」を参照してください。

* TOC
{:toc}

## カテゴリレシピを構成する

1.  **LightCastCategoriesRecipe.json**ファイル (**app > assets > recipes**にあります) を開きます。
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
    <td markdown="1">{% include_relative recipe_translator.md default_value="`ContentContainerTranslator`" %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`model`
    </td>
    <td markdown="1">データのコンテンツモデルを指定します。コンテンツモデルは、コンテンツの構造を指定し、それをFire App Builder UIにマップするためのものです。これは、デフォルトの`com.amazon.android.model.content.ContentContainer`のままにしてください。
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
    <td markdown="1">{% include_relative recipe_query.md type="categories" %} 
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
    <tr>
    <td markdown="1">`keyDataType` <br/>
    </td>
    <td markdown="1">カテゴリに関連付けられるメディアオブジェクト。どのメディアをどのカテゴリにグループ化するかを指定する必要があります。`matchList`と同様、ターゲットとするアイテムの後に`@`と`keyDataPath`を付加して指定することで、メディアオブジェクトをカテゴリにマップし、識別します。例: `StringKey@keyDataPath`。詳細については、「[keyDataTypeパラメータ](#keydatatypeparameter)」を参照してください。
    </td>
    </tr>
    </tbody>
    </table>

### queryパラメータ{#queryparameter}

使用する構文は、JSONとXMLで異なります。実際のフィードに該当するセクションを参照してください。

* [JSONフィード](#categoryjson)
* [XMLフィード](#categoryxml)

#### JSONフィード {#categoryjson}

Fire App Builderのサンプルアプリでは、JSON形式を使用する汎用のLightCastメディアフィードから読み取りを行います。カテゴリレシピでは、`query`パラメータの`$..categories[*]` という値を使用して、フィードからカテゴリのリストを返します。これは、[Jayway JsonPath](https://github.com/jayway/JsonPath)構文です。この構文の意味を次に示します。

| クエリ構文 | 照合する対象 |
|----|----|
| `$` | 検索の開始位置としてルートディレクトリを指定します。| 
| `..` | ルートのすべてのディレクトリとサブディレクトリを対象に再帰検索を行うことを指定します。| 
| `categories[]` | "categories" という名前の配列を検索することを指定します。| 
| `*` | すべての内容に一致します (ワイルドカード)。| 

これらをまとめると、`$..categories[*]` になります。ルート (`$`) で開始し、すべてのディレクトリとサブディレクトリを再帰的に検索し (`..`)、`categories`という名前の配列 (内容は任意 (`*`)) に一致します。

クエリは、[Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/)を使用してテストできます。

Fire App Builderのサンプルアプリで、このクエリ (`$..categories[*]`) を[LightCastフィードURL](http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos)のいずれかに対して実行すると、次に示すように、カテゴリ名が文字列として返されます。

```
[
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "The Country Jamaica",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "Jamaican History",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Wildlife",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Wildlife",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican Wildlife",
   "Jamaican Attractions",
   "Jamaican Wildlife"
]
```

(Fire App Builderでは、クエリ結果から重複項目が削除されます。)

Fire App Builderは、オブジェクトの配列を取得してハッシュマップに変換し、Javaクラスで処理できるようにする必要があります。そのため、次のパラメータ (`queryResultType`) を使用して、文字列の配列をオブジェクトの配列に変換します。 

```
"queryResultType": "[]$",
```

Jayway JsonPathクエリの作成方法の詳細については、「[JSONでクエリを実行する][fire-app-builder-querying-json]」を参照してください。また、Jayway JsonPath全般の詳細については、「[Jayway JsonPath](https://github.com/jayway/JsonPath)」を参照してください。

#### XMLフィード{#categoryxml}

フィードがXMLの場合は、Jayway JsonPathを使用する代わりに、[XPath式](http://www.w3schools.com/xsl/xpath_syntax.asp)を使用して、フィードの各要素をターゲットに指定する必要があります。XPathでは、XML文書が "ノード" と呼ばれるさまざまなアイテムを用いた構造に変換されます。XPath構文は各ノードの位置を指定するための構文です。 

XMLフィードに次のような構造が含まれているとします。

```xml
<rss>
    <channel>
        <item>
        <title>Sample Title</title>
        <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
        <link>https://example.com/myshow/episodes/110</link>
        <author>Sample Author name</author>
        <category>Technology</category>
        <category>Gadgets</category>
        </item>
    </channel>
</rss>
```

このXMLフィードのカテゴリを取得するためのXPath式の例は次のようになります。

```
//category/text()
```

`//`はすべてのノードを再帰的に検索して一致がないかを調べます。これにより、ノードが階層内のどこであっても検索できます。`category`でカテゴリノードを指定し、`text()`でそのノードのテキストを指定します。

これをテストするには、[XPath Tester/Evaluator](http://www.freeformatter.com/xpath-tester.html)を使用します。式からの応答は、次のようになります。

```
Text='Technology'
Text='Gadgets'
```

(`Text=`の部分は無視してください。これは単にXPath Tester/Evaluatorの表示の一部で、クエリで一致した内容ではありません。)

iTunesフィードの場合は、フィードの一般カテゴリ (`<itunes:category text="Technology">`など) と、各アイテムのカテゴリ (`<category>Technology</category>`) があることに注意してください。レシピのカテゴリをターゲットにする際は、フィードの一般カテゴリではなく、フィード内の各アイテムのカテゴリを指定する必要があります。 

XMLフィードを[XPath Tester/Evaluator](http://www.freeformatter.com/xpath-tester.html)にコピーし、同じ構文を使用してカテゴリを選択してみてください。

各種XPath式を示す他の例については、「[XMLでクエリを実行する][fire-app-builder-querying-xml]」を参照してください。また、XPath式の全般的な情報については、「[XPath syntax](http://www.w3schools.com/xsl/xpath_syntax.asp)」と「[XPath Examples](http://www.freeformatter.com/xpath-tester.html#xpath-examples)」の例のセクションを参照してください。

{% include note.html content="フィードの個々のアイテムが複数のカテゴリに関連付けられている場合、アプリの表示では、関連付けられているそれぞれのカテゴリにアイテムが表示されます。たとえば、Cool VideoがTechnologyとGadgetsの両方のカテゴリにある場合、このビデオはアプリ内で 2 か所に表示されます。" %}

### matchListパラメータ {#matchlistparameter}

`matchList`パラメータの目的は、カテゴリクエリの結果から特定のプロパティを選択して、Fire App Builderコンテンツモデルにマップすることです。

`matchList`で使用される構文は、Jayway JsonPath式やXPath式ではなく、クエリ結果内の特定の要素をターゲットにするカスタムのFire App Builder構文です(そのため、JSONとXMLの手順は、同じセクションに集約されています)。

Fire App Builderのサンプルアプリでは、カテゴリレシピの値は`StringKey@name`です。 

この構文の構成は次のとおりです。アンパサンド (`@`) の左側に、クエリの結果でターゲットにするプロパティを設定します (`StringKey`は文字列のリストを選択します)。アンパサンド (`@`) の右側には、プロパティのマップ先となるFire App Builder要素を設定します (`name`)。 

**カテゴリレシピ**の場合は、`matchList`パラメータでフィードのカテゴリを`name`にマップする必要があります。
 
Fire App Builderのサンプルアプリでは、クエリの結果が文字列のリストであるため、`StringKey`を使用して文字列を照合します。ただし、クエリの結果セットに次のようなJSONオブジェクトが含まれている場合:

```json
"list": { "title" : "My category title" }
```

`My category title`に対して照合を行い、`name`に変換するには、次の値を使用します。
 
```
list/title@name
```

下位のオブジェクトに移動するには、スラッシュ (`/`) を使用します (XPathの場合と同様)。この場合、`title`は`list`の 1 つ下のオブジェクトで、`list`はルートの 1 つ下のオブジェクトです。これらの 2 つのレベルで照合した後、その結果がカテゴリ名になります。

XML要素を照合する場合も、同様の方法を使用します。結果が文字列のリストであると想定します。

```
Text='Technology'
Text='Gadgets'
```

テキストコンテンツを照合して、Fire App Builder UIのカテゴリ要素にマップするには、次の構文を使用します。

```xml
StringKey@name
```

クエリ結果が次のような場合:

```xml
<list>
<title>My category title</title>
</list>
```

カテゴリタイトルを次のようにマップします。

```
/list/title@name
```

スラッシュ (`/`) は 1 レベル下のXMLノードを意味します。

### keyDataTypeパラメータ {#keydatatypeparameter}

選択したカテゴリでグループ化するメディアアイテムをFire App Builderに伝える必要があります。`keyDataType`パラメータは、カテゴリに関連付けられたメディアアイテムを示すためのパラメータです。このパラメータは、カテゴリレシピで使用され、その後コンテンツレシピの変数に渡されます。  

Fire App Builderのクエリの結果は文字列のリストです。そのため、`StringKey@keyDataPath`を使用してメディアアイテムをターゲットにし、カテゴリに関連付けます。自分でクエリを実行した結果も文字列のリストである場合は、デフォルト値を使用します。

```
"keyDataType": "StringKey@keyDataPath"
```

しかし、次のように、メディアオブジェクトが`assets`ノード内でリストになっており、`assets`ノードが`container`ノード内で入れ子になっている場合、

```json
"container": {
    "assets": [
    "5825652561001",
    "5825652558001",
    "5825652569001",
    "5873045223001",
    ]
}
```

メディアオブジェクトを取得するには、次のようなクエリを作成します。

```
container/assets@keyDataPath
```

`matchList`パラメータと同様、`keyDataType`クエリでもJayway JsonPath構文は使用しません。代わりに、ノード名の後に `/` を付けて次のレベルに移動し、各ノードを照合します。`container/assets`では、このレベルのすべてのアイテムが照合されます。 

アンパサンド`@`の右側にある`keyDataPath`キーは、Fire App Builderでこれらのメディアオブジェクトを保存および使用する方法を示しています。`@keyDataPath`を使用することで、アイテムをFire App Builderコンテンツモデルと一致させることができます。

## カテゴリを持たないフィード

フィード内にカテゴリがなく、カテゴリごとに個別のフィードを用意している場合は、Navigator.jsonの構成時にカテゴリ名をハードコーディングできます。これにより、各フィード内のすべてのビデオが特定のカテゴリでグループ化されます。この方法の詳細については、「["Hardcoding Your Categories"][fire-app-builder-configure-navigator-open-feeds#hardcodingcategories]」を参照してください。

## 次のステップ

アプリのメディアフィードのカテゴリを構成できたところで、続いては、コンテンツを構成する必要があります。「[コンテンツレシピをセットアップする][fire-app-builder-set-up-recipes-content]」を参照してください。

{% include links.html %}

