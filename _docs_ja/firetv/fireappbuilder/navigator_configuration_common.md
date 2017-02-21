## ライブメディアを処理する

ライブメディアを使用する場合は、[Content Details] 画面に [Watch Now] ボタンのみが表示されるようにアプリを調整します(ライブコンテンツは、巻き戻して "最初から見る" ことができません)。 

フィード全体をライブとしてマークする場合は、**Navigator.json* *(**app** > **assets**フォルダーにあります) を開き、下記の中で赤で示されているプロパティを追加します。このオブジェクトの前後にあるコンテンツは、コンテキストの理解のために示しています。

<pre>
     ...
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
    ...
</pre>

ライブのメディアが一部のみで、他のライブでないコンテンツと混在している場合は、コンテンツのクエリを実行する際に、`matchList`パラメータでこのコンテンツを照合できます。例を次に示します。

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

この例では、`live/#text`を使用して、ライブタグが含まれたフィードの要素をターゲットに指定しています。 

## カテゴリをハードコーディングする {#hardcodingcategories}

フィード構造にカテゴリが含まれていない場合は、Navigator.jsonにカテゴリ名をハードコーディングし、そのカテゴリのメディアフィード全体を挿入します。LightCastCategoriesRecipe.jsonファイルからカテゴリをロードする代わりに、次のように、`globalRecipes`オブジェクト内にカテゴリを構成します。

```json

"globalRecipes": [
{
  "categories" : {
    "name" : "Hard-coded category name"
  },
  "contents" : {
    "dataLoader" : "recipes/YourAppNameDataLoaderRecipe1.json",
    "dynamicParser" : "recipes/YourAppNameContentsRecipe.json"
  }
 }
]
```

カテゴリレシピではなく、コンテンツレシピをロードします (フィードにカテゴリが含まれていないため)。`contents`オブジェクトは、前のセクションの説明と同様の方法でロードされます (URLファイル内のメディアURLごとに別々のDataLoaderRecipe[#]ファイルが使用されます)。

## 次のステップ

フィードを構成できたところで、続いては、アプリのルックアンドフィールのカスタマイズを始めてください。「[アプリのルックアンドフィールをカスタマイズする][fire-app-builder-customize-look-and-feel]」を参照してください。