---
タイトル: アプリの概要
permalink: fire-app-builder-app-tour.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

[アプリの作成][fire-app-builder-download-and-build]が無事終了したら、少し時間を割いて、さまざまな画面を見てみましょう。以降のセクションでは、Fire App Builderのサンプルアプリの各画面について説明します。

* TOC
{:toc}

## アプリの画面

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_splash" type="jpg" alt="スプラッシュ画面" caption="<b>スプラッシュ画面。</b>この画面は、最初にアプリを起動したときに表示され、アプリのロードが終了すると消えます (通常は、1 秒以内に消えます)。" %}

{% include note.html content="この画面を含め、\"Fire App Builder\" というテキストが表示される画面は、自由にカスタマイズできます。たとえば、\"Fire App Builder\" の部分を会社の名前や独自のデザインに置き換えることができます。" %}

アプリのロードが終了すると、ホーム画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="jpg" alt="ホーム画面" caption="<b><code>ContentBrowseActivity</code></b>が構成されたホーム画面。この画面では、ビデオがカテゴリ別またはグループ別に整理されて表示されます。チャネルを表示すると、チャネルグループの先頭のビデオが背景画像として使用され、左上にそのタイトルと説明が表示されます。これは、デフォルトのレイアウトです。" %}

ホーム画面では、選択したアクティビティに応じて、いくつかの表示オプションが表示されます。デフォルトでは、ホーム画面に`ContentBrowseActivity`アクティビティが構成されています。代わりに`FullContentBrowseActivity`アクティビティを関連付けて、次のようなコンパクトな表示をロードすることもできます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fullbrowse" type="jpg" alt="FullContentBrowseActivityが関連付けられたホーム" caption="<b><code>FullContentBrowseActivity</code>が関連付けられたホーム</b>。このアクティビティを使用すると、圧縮されたグリッドにすべてのビデオが表示され、左側にカテゴリの一覧が表示されます。大きな画像で背景に重ね合わせて表示されるビデオはありません。" %}

ビデオを選択すると、そのビデオがハイライトされ、右上に背景画像として表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_selectvideo" type="jpg" alt="選択されたビデオ。" caption="<b>ビデオが選択されたホーム画面</b>。選択したビデオは背景に大きな画像で表示されます。" %}

ビデオを再度クリックすると、[Content Details] 画面が再生ボタンとともに表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentdetails" type="jpg" alt="[Content Details]" caption="[<b>Content Details</b>]。この画面には、タイトルと説明を含むビデオの詳細情報が表示されます。" %}

ビデオの説明が表示幅を超える場合は、モーダルが表示されます。ユーザーはこのモーダルで説明の全内容を確認できます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentmoredetails" type="jpg" alt="詳細" caption="<b>[Content Details] 画面</b>。[Content Details] 画面に割り当てられたスペースに説明が収まっていない場合、ユーザーはスペースを拡張して、説明の続きを読むことができます。" %}

メディアを再生すると、[Renderer] 画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrenderer" type="jpg" alt="[Renderer]" caption="[<b>Renderer</b>]。この画面は、メディアを再生すると表示されます。" %}

ビデオにコントロールが表示されると、その下にお勧めのコンテンツが薄くオーバーレイとして表示されます。リモコンで下矢印をクリックすると、薄いオーバーレイが消え、コンテンツがはっきりと表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrecexpanded" type="jpg" alt="お勧めのコンテンツ" caption="お勧めのコンテンツを選択して表示する" %}

メディアを再生した後でコンテンツの詳細を表示すると、ビデオの下に複数のコントロールが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_resumeplayback" type="jpg" alt="再生を再開する。" caption="<b>[Details] で再生を再開する</b>。ビデオの視聴を途中で停止した場合は、[WATCH NOW] ボタンの代わりに、[RESUME PLAYBACK] ボタンと [WATCH FROM BEGINNING] ボタンが表示されます。" %}

ビデオを検索するには、ホーム画面で検索ボタンを選択します。検索ボタンを選択すると、[Search] 画面が表示され、キーワードを入力できるようになります。検索では、キーワードがタイトル要素および説明要素と照合されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_search" type="jpg" alt="[Search]" caption="[<b>Search</b>]。この画面でメディアを検索できます。" %}

検索結果の画面では、結果がメディアカードとしてグリッドに表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_searchresultsmultiple" type="jpg" alt="検索" caption="<b>検索</b>。検索結果は、検索バーの下にサムネイルで表示されます。この例では、「<i>beach</i>」という語に多数のビデオが一致しています。" %}

## 各画面で実行されるアクティビティ

アクティビティとは、アプリで実行可能なさまざまな機能のことです。アクティビティごとに異なる画面が呼び出されます。Fire App Builderでは、次の 6 つのアクティビティを使用できます。

* `ContentBrowseActivity`
* `ContentDetailsActivity`
* `ContentSearchActivity`
* `FullContentBrowseActivity`
* `SplashActivity`
* `VerticalContentGridActivity`

(ホーム画面では、`ContentBrowseActivity`と`FullContentBrowseActivity`を使用できます。)

Fire App Builderでは、アクティビティごとに異なる画面が使用されます。各アクティビティで使用される画面は、(**app** > **assets**にある) Navigator.jsonファイルによって構成されます。Navigator.jsonの`graph`オブジェクト (下記参照) にはキーと値のペアが含まれており、このペアを使用してアクティビティが画面や他のプロパティに関連付けられています。

```json
"graph": {
    "com.amazon.android.tv.tenfoot.ui.activities.SplashActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_SPLASH_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.FullContentBrowseActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_HOME_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.ContentDetailsActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_DETAILS_SCREEN"
    },
    "com.amazon.android.tv.tenfoot.ui.activities.ContentSearchActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_SEARCH_SCREEN"
    },
    "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": false,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
  }
```

たとえば、`SplashActivity`ではスプラッシュ画面が表示され、`ContentBrowseActivity`ではホーム画面が表示されます。

どの画面にも任意のアクティビティを関連付けることができますが、変更する意味のあるアクティビティは、`ContentBrowseActivity`のみです。このアクティビティを`FullContentBrowseActivity`に置き換えると、前述したようにホーム画面のレイアウトをコンパクトにすることができます。

上記のコードに含まれる各アクティビティの他のプロパティを次に示します。

| アクティビティのプロパティ | 説明 |
|--------|------------|
| `verifyScreenAccess` | 画面を表示する際にユーザーに認証を要求します。コンテンツを表示する前にユーザーにログインを要求したい場合は、`true`に設定します。多くの場合、アクセスの検証を行うのは [Content Renderer] 画面のみです。そうすることで、ユーザーに、まずメディアの雰囲気を伝え、ログインしたい気持ちをかき立てることができます。|
| `verifyNetworkConnection` | 画面を表示する際にネットワーク接続を要求します。(ページに含まれるのが設定のみで、オンラインメディアは含まれない場合は、`false`に設定しますほとんどの画面では`true`のままにします)。 |
| `onAction` | アクティビティの実行時に実行するアクション (特定の画面の表示など)。|

AndroidManifest.xmlファイルは、アプリの起動時に`SplashActivity`アクティビティを開始します。

```xml
<activity
    android:name="com.amazon.android.tv.tenfoot.ui.activities.SplashActivity"
    android:screenOrientation="landscape">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
        <category android:name="android.intent.category.LEANBACK_LAUNCHER" />
    </intent-filter>
</activity>
```

この`SplashActivity`アクティビティでは、スプラッシュ画面がロードされます。

これらのアクティビティや画面を使用する以外に、自分でアクティビティを作成して、独自の画面に関連付けることもできます(こうした高度なカスタマイズ方法の詳細については、本ドキュメントでは扱いません)。

## Fire App Builderのコンテンツを見る

アプリの作成を開始する前に、少し時間を割いて、Fire App Builderに用意されている各種のライブラリ、モジュール、およびコンポーネントについて理解しましょう。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_components" type="png" caption="Fire App Builderで使用可能なコンポーネント" %}

次の表で、各コンポーネントについて簡単に説明します。



<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>フォルダー</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>AMZNMediaPlayerComponent</td>
      <td>ストリーミングメディアの再生に使用します。</td>
    </tr>
    <tr>
      <td>AdsInterface</td>
      <td>広告用のインターフェース。</td>
    </tr>
    <tr>
      <td>AmazonInAppPurchaseComponent</td>
      <td>Amazonアプリ内課金の購入インターフェースを実装するコンポーネント。</td>
    </tr>
    <tr>
      <td>AnalyticsInterface</td>
      <td>分析用のインターフェース。</td>
    </tr>
    <tr>
      <td>AuthInterface</td>
      <td>認証用のインターフェース。</td>
    </tr>
    <tr>
      <td>ContentBrowser</td>
      <td>アプリ内のコンテンツをユーザーが参照できるようにする (<a href="https://ja.wikipedia.org/wiki/Model_View_Controller">Model View Controller</a>アーキテクチャパターンの) コントローラ。フィードを取得し、フロー、レシピ、および構成を制御します。</td>
    </tr>
    <tr>
      <td>ContentModel</td>
      <td>レンダリングの際にブラウザで使用されるデータの格納方法を定義する (<a href="https://ja.wikipedia.org/wiki/Model_View_Controller">Model View Controller</a>アーキテクチャパターンの) モデル。</td>
    </tr>
    <tr>
      <td>DataLoader</td>
      <td>ネットワークからデータをロードする再利用可能なモジュール。一般に、フィードをロードする際に使用されます。</td>
    </tr>
    <tr>
      <td>DynamicParser</td>
      <td>フィードを解析し、モデルにデータを入力する構成可能なモジュール。</td>
    </tr>
    <tr>
      <td>FacebookAuthComponent</td>
      <td>Facebook認証用の認証インターフェースを実装するコンポーネント。</td>
    </tr>
    <tr>
      <td>FlurryAnalyticsComponent</td>
      <td>Flurry Analytics用の分析インターフェースを実装するコンポーネント。</td>
    </tr>
    <tr>
      <td>ModuleInterface</td>
      <td>Fire App Builderフレームワークで各種コンポーネントをモジュール化するコアコード。</td>
    </tr>
    <tr>
      <td>PassThroughAdsComponent</td>
      <td>Fire App Builderで使用される広告インターフェースのダミー実装。アプリに広告を実装していない場合は、これをベースコードとして使用して、独自の広告モジュールを開始できます。</td>
    </tr>
    <tr>
      <td>PurchaseInterface</td>
      <td>支払いを設定するためのインターフェース。</td>
    </tr>
    <tr>
      <td>TVUIComponent</td>
      <td>Leanback Support LibraryをベースにしたTV UIコードが含まれています。また、アクティビティ用のクラスも含まれています。</td>
    </tr>
    <tr>
      <td>UAMP</td>
      <td>汎用Androidメディアプレーヤー。Amazon Media Playerは、UAMPを基盤として構築され、追加機能で拡張されています。</td>
    </tr>
    <tr>
      <td>Utils</td>
      <td>Facebook認証、Adobe Pass、Flurry Analyticsなどのコンポーネントで使用される鍵の暗号化と暗号化解除を行うためのセキュリティクラスなど、再利用可能なJavaクラスが含まれています。</td>
    </tr>
    <tr>
      <td>Application</td>
      <td markdown="span">Fire App Builderフレームワークの各種のライブラリやコンポーネントを使用するサンプルアプリ。これは、カスタマイズ用のアプリです (「[Fire App Builderをダウンロードして、アプリをビルドする][fire-app-builder-download-and-build#customize]」を参照)。</td>
    </tr>
  </tbody>
</table>
 
{% include note.html content="Fire App BuilderをAndroid Studioで開いた場合、このアプリが作業アプリとなるため、\"Android\" ビューではフォルダーが \"app\" と表示されます。\"Project\" ビューに切り替えると、appフォルダーの実際の名前が [Application] であることがわかります。" %}

## サブフォルダーの内容

通常、Fire App Builderの各フォルダーには、同じパターンのサブフォルダーがあります。次の表で、各サブフォルダーについて説明します。

<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>サブフォルダー</th>
      <th>内容</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>build</td>
      <td>このサブフォルダーは、Androidに必要なもので、プロジェクトの作成時に自動的に生成されます。buildフォルダー内のファイルは編集しないでください。</td>
    </tr>
    <tr>
      <td>libs</td>
      <td>このサブフォルダーがある場合は、コンポーネントや他の外部サービスで必要な外部ライブラリが格納されています。このフォルダー内のファイルは編集しないでください。このサブフォルダーがない場合、コンポーネントで必要なライブラリは、build.gradleファイルで依存関係として参照され、プロジェクトのビルド時に取得されます。</td>
    </tr>
    <tr>
      <td>src</td>
      <td>コンポーネントや機能に対応する実際のコードが含まれています。resサブフォルダーには、コンポーネントのリソースが含まれています。これらのリソースは、コンポーネントを使用するときに頻繁に操作します。ファイルを編集する際は、主にresサブフォルダー内に表示されるファイルを編集します。</td>
    </tr>
    <tr>
      <td>test</td>
      <td>単体テストファイル。testフォルダーの内容は、メインフォルダーの内容をミラーリングしたもので、単体テストの作成を目的としています。単体テストでは、Androidの依存関係は不要です。Fire TV端末を使用せずに、コンピューターで単体テストを実行できます。通常、このフォルダーでは何もする必要はありません。</td>
    </tr>
    <tr>
      <td>androidTest</td>
      <td>Androidの依存関係が必要なテスト。このテストでは、Fire TV端末でコードを実行する必要があります。通常、このフォルダーでは何もする必要はありません。</td>
    </tr>
  </tbody>
</table>

## 構成可能なJSONファイルの概要

Fire App Builderの基本的なアプリでは、Javaのコーディングはほとんど不要です。代わりに、単純なキーと値のペアが含まれた各種JSONファイルを使用してアプリを構成します。アプリで必要なオプションは、このペアを使用して指定します。構成可能なJSONファイルを次に示します。

<table class="grid">
<colgroup>
<col width="40%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>構成するJSONファイルまたはXMLファイル</th>
      <th>場所</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Navigator.json</td>
      <td>app &gt; assets</td>
    </tr>
    <tr>
      <td>BasicFileBasedUrlGeneratorConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>BasicHttpBasedDownloaderConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>DataLoadManagerConfig.json</td>
      <td>app &gt; assets &gt; configurations</td>
    </tr>
    <tr>
      <td>LightCastCategoriesRecipe.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastContentsRecipe.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastDataLoaderRecipe1.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>LightCastDataLoaderRecipe2.json</td>
      <td>app &gt; assets &gt; recipes</td>
    </tr>
    <tr>
      <td>custom.xml</td>
      <td>app &gt; res &gt; values</td>
    </tr>
  </tbody>
</table>
(JSONファイルやXMLファイルの構成方法については、ここでは気にしないでください。これらのファイルは、今後の構成タスクを紹介するためだけに示しています。重要なのは、Javaプログラミングを行わなくても、JSONファイルまたはXMLファイルを調整するだけでアプリを構成できるという点です。)

## 次のステップ

Fire App Builderの基本的な機能、ライブラリ、およびコンポーネントを理解できたところで、続いては、ご自身で用意したコンテンツでカスタマイズを始めましょう。「[メディアフィードをロードする][fire-app-builder-load-media-feed]」を参照してください。

{% include links.html %}
