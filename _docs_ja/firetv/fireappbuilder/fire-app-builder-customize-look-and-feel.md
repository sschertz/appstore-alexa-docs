---
title: アプリのルックアンドフィールをカスタマイズする
permalink: fire-app-builder-customize-look-and-feel.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

アプリのcustom.xmlファイル (**res > values**にあります) とnavigator.json (**app > assets**にあります) を使用して、アプリのルックアンドフィールのほとんどをカスタマイズできます。簡単に変更できるアプリの要素を次に示します。

* フォント
* スプラッシュ画面
* お勧めのコンテンツ
* 背景色
* コンテンツカード
* ボタン
* 検索バー
* コンテンツのリロード時間
* ホームページのレイアウト
* アプリのアイコン
* 利用規約

* TOC
{:toc}

## フォントを変更する

Navigator.json (**app > assets**にあります) の`branding`オブジェクトを使用してアプリのフォントを変更できます。

```json
"branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "Roboto Light",
    "boldFont" : "Roboto Bold",
    "regularFont" : "Roboto Regular"
  }
```

フォントの使用方法を次に示します。

| プロパティ | 使用する場所 |
|----|-----|
| `lightFont` | 説明と本文で使用されます。|
| `boldFont` | タイトルで使用されます。|
| `regularFont` | ボタンと字幕で使用されます。|

(`globalTheme`プロパティを使用すると、他の値は指定できません。)

これら 3 つのフォントオプションには、有効なデバイスフォントのほか、カスタムフォントを割り当てることもできます。たとえば、必要に応じて`Roboto Bold`を 3 つのフォントすべてに適用できます。

使用できるデバイスフォントは次のとおりです。

* **Amazon Emberフォント**: Amazon Ember、Amazon Ember Bold、Amazon Ember Bold Italic、Amazon Ember Italic、Amazon Ember Light、Amazon Ember Light Italic、Amazon Ember Medium、Amazon Ember Medium Italic、Amazon Ember Thin、Amazon Ember Thin Italic、AndroidClock Regular、AndroidClock-Large Regular
* **Robotoフォント**: Roboto Black、Roboto Black Italic、Roboto Bold、Roboto Bold Italic、Roboto Condensed Bold、Roboto Condensed Bold Italic、Roboto Condensed Italic、Roboto Condensed Light、Roboto Condensed Light Italic、Roboto Condensed Regular、Roboto Italic、Roboto Light、Roboto Light Italic、Roboto Medium、Roboto Medium Italic、Roboto Regular、Roboto Thin、Roboto Thin Italic
* **Verdanaフォント**: Verdana、Verdana Bold、Verdana Bold Italic、Verdana Italic
* **その他のフォント**: Carrois Gothic SC、Clockopia、Code2000、Coming Soon、Cutive Mono、Dancing Script、Dancing Script Bold
Droid Sans Mono、Kindle Symbol、MotoyaLMaru W3 mono、MT Chinese Surrogates、NanumGothic、Source Code Pro Medium

たとえば、Amazon Emberフォントを使用する場合は、次のように`branding`オブジェクトをカスタマイズします。

```json
  "branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "Amazon Ember",
    "boldFont" : "Amazon Ember Bold",
    "regularFont" : "Amazon Ember"
  }
```

画面は以下のように表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fontsnewhome" type="png" %}

別の画面:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fontsnewdetails" type="png" %}

これらのAmazon Emberフォントはタイトル、字幕、説明、本文、ボタンに使用されます。

さまざまなデバイスフォントに加えて、カスタムフォントも使用できます。カスタムフォントを使用する場合は、アプリの**assets/fonts**ディレクトリにそのフォントを格納します。次に`branding`オブジェクトで (`Proxima Nova Light`と指定するだけではなく) `fonts/Proxima-Nova-Light.tff`のようにフォントへのパスを指定します。

```json
  "branding": {
    "globalTheme": "AppTheme",
    "lightFont" : "fonts/Proxima-Nova-Light.tff",
    "boldFont" : "fonts/Proxima-Nova-Light.tff",
    "regularFont" : "fonts/Proxima-Nova-Light.tff"
  }
```

## スプラッシュ画面をカスタマイズする

デフォルトでは、Fire App Builderのサンプルアプリでは次の起動画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_splashdiagram" type="svg" alt="スプラッシュ画面" %}

以下の方法で、custom.xmlファイルでスプラッシュ画面の設定を調整できます。

```xml
<!-- Splash Screen Customization -->

<!-- Background to display on Splash Screen -->
<drawable name="splash_background">@drawable/bg_generic_nopreview</drawable>
<!-- Company logo to show on Splash Screen -->
<drawable name="splash_logo">@drawable/fire_app_builder_white</drawable>
<!-- Copyright string text color -->
<color name="copyright">#E6FFFFFF</color>

<!-- End of Splash Screen Customization -->
```

`@drawable`への参照は、**TVUIComponent > res > drawable**の画像を参照します。これらの画像をカスタマイズした画像に置き換えるか、独自の画像ファイルを追加してそれをポイントするようXMLの参照を更新します。

`bg_generic_nopreview`は 1900 x 1080 pxの真っ黒な画像です。これと同じ背景がフルブラウズのホームページレイアウトに使用されますが、必要に応じてここに別の画像参照を指定できます。

`splash_logo`には、背景が透明であるロゴが含まれている必要があります。サンプルアプリケーションのロゴ画像のサイズは 356 x 108 pxですが、必要に応じて大きい画像を使用できます。画像は自動的に縮小されます。

ロゴはスプラッシュ画面の背景画像の上にオーバーレイとして表示されるため、画像をカスタマイズする場合は、重なったときの見た目が適切になるようにします。暗い背景の上に明るいロゴを表示すると、必要なコントラストが得られます。

スプラッシュ画面の著作権に関するテキストをカスタマイズするには、アプリの**res > values > strings.xml**フォルダーにある**strings.xml**フォルダーを確認してください。

```xml
<!-- Copyright string -->
<string name="copyright">Copyright 2016. All Rights Reserved.</string>
```

## [Recommended Content] セクションをカスタマイズする {#recommendations}

[Content Details] 画面では、コンテンツのプレビューの下に "お勧めのコンテンツ" の一覧が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_recommendedcontentdiagram" type="svg" alt="[Recommended Content]" %}

[Recommended Content] の設定方法の詳細については、「コンテンツレシピをセットアップする」の「["お勧めのコンテンツ (タグを使用)"][fire-app-builder-set-up-recipes-content#tags]」を参照してください。

## ホームページのレイアウトをカスタマイズする

デフォルトのホーム画面のレイアウトには、`ContentBrowseActivity`を使用します。このレイアウトを "ホームページブラウズレイアウト" と呼びます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="ホーム" caption="<b> <code>ContentBrowseActivity</code>を使用したホーム</b>。この画面では、ビデオがチャネル別またはグループ別に整理されて表示されます。チャネルを表示すると、チャネルグループの先頭のビデオが背景画像として使用され、左上にそのタイトルと説明が表示されます。" %}

代わりに`FullContentBrowseActivity`を使用すると、ホームページのレイアウトを変更して、コンパクトに表示することができます。このホームページレイアウトを "ホームページフルブラウズレイアウト" と呼びます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fullbrowse" type="png" alt="FullContentBrowseActivityを使用したホーム" caption="<b><code>FullContentBrowseActivity</code>を使用したホーム</b>。このアクティビティを使用すると、圧縮されたグリッドにすべてのビデオが表示され、左側にチャネルの一覧が表示されます。大きな画像で背景に重ね合わせて表示されるビデオはありません。" %}

左のサイドバーは、ユーザーがビデオタイトルを閲覧している間は縮小することができます。これにより、ビデオコンテンツに多くの領域を割り当て、見やすくすることができます。

コンパクトなフルブラウズレイアウトにホームページを変更するには:

1.  **Navigator.json** (**app > assets**にあります) をファイルを開きます。
2.  `graph`オブジェクトで、**CONTENT_HOME_SCREEN**を見つけます。

    ```json
    "com.amazon.android.tv.tenfoot.ui.activities.ContentBrowseActivity": {
          "verifyScreenAccess": false,
          "verifyNetworkConnection": true,
          "onAction": "CONTENT_HOME_SCREEN"
        }
    ```

3.  ContentBrowseActivity`を`FullContentBrowseActivity`に変更します。

次の図は、ホームページのブラウズレイアウト (デフォルト) でカスタマイズできるプロパティを示しています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_homepagediagram" type="svg" %}

<code>browse_background</code>プロパティは、TVUIComponentのcustom.xmlファイルを使用して設定され、16 進値ではなく画像を参照します。

次の図は、ホームページのフルブラウズレイアウトのプロパティを示しています

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_althomepagediagram" type="svg" %}

アプリのcustom.xmlにある次のコードは、これらのホームページオプションが設定される場所を示しています。

```xml
    <!-- Browse Customization -->

    <!-- The background gradient color, if no background drawable is provided.
         Used in default_background.xml -->
    <color name="background_gradient_start">#000000</color>
    <color name="background_gradient_end">#DDDDDD</color>
    <!-- The header's bar color (left-hand navigation bar) -->
    <color name="browse_headers_bar">#0DFFFFFF</color>
    <!-- Selected header text color -->
    <color name="browse_header_selected">#FFFFFFFF</color>
    <!-- Non-selected header text color -->
    <color name="browse_header">#CCD8D8D8</color>
    <!-- Header color when left-hand navigation bar is closed
         and rows are showing full screen -->
    <color name="browse_row_header">#99FFFFFF</color>
    <!-- Search orb color (the circle shape around the search icon) -->
    <color name="search_orb">#EE962D</color>
    <!-- Header text size -->
    <dimen name="browse_header_text">16sp</dimen>
    <!-- Company logo -->
    <drawable name="company_logo">@drawable/fire_app_builder_white_2</drawable>

    <drawable name="browse_bg_color">@drawable/bg_generic_nopreview</drawable>
    <!-- End of Browse Customization -->
```
{% include note.html content="上記のアプリケーションのcustom.xmlファイルでは、Browse Customizationセクション内のコメントは無視してください。一部のコメントは古く、一部の要素は適切に設定されていません。各要素の働きを理解するには、次の表を参照してください。" %}

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="60%" />
   </colgroup>
   <thead>
      <tr>
         <th>要素</th>
         <th>説明</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td><code>background_gradient_start</code></td>
         <td>何もしません。これはバグです。</td>
      </tr>
      <tr>
         <td><code>background_gradient_end</code></td>
         <td>何もしません。これはバグです。</td>
      </tr>
      <tr>
         <td><code>browse_headers_bar</code></td>
         <td>コンパクトなホームページレイアウトの使用中に、左ナビゲーションバーの色を変更します。</td>
      </tr>
      <tr>
         <td><code>browse_header_selected</code></td>
         <td>何もしません。これはバグです。</td>
      </tr>
      <tr>
         <td><code>browse_header</code></td>
         <td>何もしません。これはバグです。</td>
      </tr>
      <tr>
         <td><code>browse_row_header</code></td>
         <td>ホームページブラウズレイアウトの場合は、ビデオ行の上にあるカテゴリタイトルを変更します。フルブラウズレイアウトの場合は、左ナビゲーションバーにあるカテゴリタイトルの色を変更します。選択されたカテゴリタイトルと選択されていないカテゴリタイトルの両方にこの色が適用されます。選択されたカテゴリタイトルは太字で、非選択のカテゴリタイトルは柔らかい落ち着いた色で表示されます。</td>
      </tr>
      <tr>
         <td><code>search_orb</code></td>
         <td>何もしません。これはバグです。</td>
      </tr>
      <tr>
         <td><code>browse_header_text</code></td>
         <td>カテゴリタイトルのサイズを変更します。</td>
      </tr>
      <tr>
         <td><code>company_logo</code></td>
         <td>この画像は<b>TVUIComponent > res > drawable</b>に保存されています。使用するロゴに置き換えてください。用意する画像は透明で、高さが 108 px、幅が 356 px以上になるようにしてください。これよりも大きい画像も使用できますが、アプリによって縮小されます。</td>
      </tr>
      <tr>
         <td><code>browse_bg_color</code></td>
         <td>フルブラウズレイアウトとスプラッシュ画面の両方に使用される背景画像。この画像は<b>TVUIComponent > res > drawable</b>に保存されています。画像は黒色を使用した 1900 x 1080 pxのPNG画像です。</td>
      </tr>
      <tr>
         <td><code>browse_background</code></td>
         <td>この要素はアプリケーションのcustom.xmlファイルには含まれていませんが、これはバグです。この要素には、行の追加だけを行うことができます。この要素を使用して、ホームページ<i>ブラウズ</i>レイアウトに使用される背景画像を制御します。この画像の詳細については、下のセクションを参照してください。</td>
      </tr>
      <tr>
         <td><code>browse_bg_color</code></td>
         <td>この要素はアプリケーションのcustom.xmlファイルには含まれていませんが、これはバグです。この要素には、行の追加だけを行うことができます。この要素を使用して、ホームページ<i>フルブラウズ</i>レイアウトに使用される背景画像を制御します。この画像の詳細については、下のセクションを参照してください。</td>
      </tr>
   </tbody>
</table>

### ホームページブラウズレイアウトの背景画像

custom.xmlに行がないため、上記のcustom.xmlの値ではホームページブラウズレイアウト (デフォルト) の背景色は設定できません。

```
    <drawable name="browse_background">@drawable/bg_generic</drawable>
```

アプリケーションのcustom.xmlファイルにこの行を追加して、使用する背景画像ファイルを制御できます。または、**TVUIComponent > res > values > custom.xml**にあるcustom.xmlを編集することもできます(アプリのcustom.xmlファイルによって各種XMLファイルの他の値が上書きされるため、どちらのファイルにプロパティ設定してもかまいません。ただし、ベストプラクティスとしては、コンポーネントファイルは編集せずに、アプリケーションのconfig.xmlファイルのみを編集してください)。

`browse_bg_color`要素と同様、ここで参照されるドローワブル画像は、**TVUIComponent > res > drawable**に保存されます (Androidビューで閲覧している場合)。

bg_generic.png画像は次のようになります。

{% include image.html url="https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/fireappbuilder/bg_generic.png" file="firetv/fireappbuilder/images/bg_generic" type="png" caption="<b>bg_generic.png</b>。画像は、右上を透明にした 1900 x 1080 pxのPNG画像です。" max-width="80%" %}

このデフォルトの画像を変更するには、使用する画像ファイルを同じサイズで作成します。既存の画像のように、右上隅を透明にします。このファイルに**bg_generic.png**という名前を付け、**TVUIComponent > res > drawable**の既存のファイルと置き換えます。

画像を簡単にカスタマイズする方法として、[Photoshopファイルをダウンロード](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/fireappbuilder/bg_generic.psd_V523835574_.zip)し、これを使用して画像を作成できます。Photoshopで画像のレイヤーを表示し、塗りつぶしツールを使用して、2 つのレイヤーに新しい色を適用します。ダウンロードファイルでは、色が黒から青に変更されており、識別しやすくなっています。

フルブラウズレイアウトの場合も、同様の方法でこの画像を変更できます。具体的には、**TVUIComponent > res > drawable**にあるbg_generic_nopreview.pngを置き換えるか、独自の画像ファイルを作成して、アプリケーションのcustom.xmlファイルでその画像ファイルへの参照を更新します。

## コンテンツカードをカスタマイズする

コンテンツカードの背景色とサイズをカスタマイズできます。"カード" とは、メディアの詳細が表示される長方形のサムネイルです。たとえば、次のホーム画面には "Jamaican Attractions" というタイトルの下にコンテンツカードの行が表示されています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_moviecardsdiagram" type="svg" alt="Fire App Builderカードのホーム" %}

次のコードは、これらのカードの外観をカスタマイズするために使用できる設定を示しています。

```xml
<!-- Movie Info Card Customization -->

<!-- Background color of the card info -->
<color name="card_info_bg">#2B2B2B</color>
<!-- Card info title text color -->
<color name="card_info_title_text">#E6FFFFFF</color>
<!-- Card info content text color -->
<color name="card_info_content_text">#FFFFFFFF</color>
<dimen name="card_info_title_text">8sp</dimen>
<!-- Card info content text size -->
<dimen name="card_info_content_text">12sp</dimen>
<!-- Card info selected title text size -->
<dimen name="card_info_selected_title_text">10sp</dimen>
<!-- Card info selected content text size -->
<dimen name="card_info_selected_content_text">15sp</dimen>

<!-- End of Movie Info Card Customization -->
```

## コンテンツの詳細をカスタマイズする

[Content Details] 画面のさまざまな要素をカスタマイズできます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentdetailsdiagram" type="svg" %}

custom.xmlファイル内の関連するコードは次のとおりです。

```xml
<!-- Details Customization -->

<!-- Text color for unfocused action button -->
<color name="action_button_text_color">#E6FFFFFF</color>
<!-- Text color for focused action button -->
<color name="action_button_text_color_focused">#E6FFFFFF</color>
<!-- Background color for focused action button -->
<drawable name="action_button_focused">@drawable/btn_generic_focused</drawable>
<!-- Background color for normal state of action button -->
<drawable name="action_button_normal">@drawable/btn_normal</drawable>
<!-- Details description title text color -->
<color name="details_description_title">#E6FFFFFF</color>
<!-- Details description body text color -->
<color name="details_description_body">#FFFFFFFF</color>
<!-- Action button text size -->
<dimen name="action_text">14sp</dimen>
<!-- Details description title text size -->
<dimen name="details_description_title_text">24sp</dimen>
<!-- Details description body text size -->
<dimen name="details_description_body_text">16sp</dimen>

<!-- End of Details Customization -->
```

## 再生オーバーレイをカスタマイズする

[Content Renderer] 画面で再生オーバーレイをカスタマイズできます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_contentrendererdiagram" type="svg" alt="[Content Renderer] 画面" %}

```xml
<!-- Playback Overlay Customization -->

<!-- Background color of the playback overlay -->
<color name="playback_background">#9922262A</color>
<!-- Background color of the progress bar -->
<color name="playback_background_progress_bar">#FF373737</color>
<!-- Color of buffered progress bar -->
<color name="playback_buffered_progress">#FF5A5A5A</color>
<!-- Color of the progress bar -->
<color name="progress_bar">#FFDADADA</color>
<!-- Text color of the playback time -->
<color name="playback_time_text">#FFFFFFFF</color>
<!-- Hide More options status activation status-->
<bool name="hide_more_options">true</bool>
<!-- End of Playback Overlay Customization -->
```

## 検索バーをカスタマイズする

検索バーで使用される背景色とテキストをカスタマイズできます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_searchscreendiagram" type="svg" alt="検索画面の図" %}

custom.xmlファイル内の関連するコードは次のとおりです。

```xml

<!-- Search Bar Customization -->

<!-- Background color of search orb -->
<color name="search_orb_background">#EE962D</color>
<!-- Icon to display inside of search orb -->
<drawable name="search_icon">@drawable/ic_search</drawable>
<!-- Background color of Search screen -->
<drawable name="search_background">@drawable/bg_gradient_search</drawable>
<!-- Search hint text -->
<string name="search_bar_hint">I\'m looking for…</string>

<!-- End of Search Bar Customization -->
```

## コンテンツのリロード時間をカスタマイズする

コンテンツのリロード時間をカスタマイズできます (コンテンツとは、アプリがメディアフィードからロードするビデオと詳細情報のことです)。デフォルトでは、リロード時間は 14400000 ミリ秒、つまり 4 時間です。この時間が経過すると、Navigator.jsファイル (**app > assets > resources**にあります) によって、レシピやデータローダーの設定がリロードされます。

```xml
<!-- Content Reload time in milliseconds -->

<integer name="time_to_reload_content">14400000</integer>

<!-- End Content Reload time in milliseconds -->
```

## アプリのアイコンをカスタマイズする

アプリのアイコンを変更できます。これは、Fire TVでアプリのリストに表示されるサムネイル画像です。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_appicondiagram" type="svg" alt="アプリのアイコン" %}

このファイルを更新するには、ic_launcher.svgファイルを変更します。**Project**ビューに切り替えて、**app > src > main > res**の中を探します。アプリアイコンのファイルが 4 つあり、それぞれが異なる画面サイズに対応しています。

* mipmap-hdpi (72 x 72 px)
* mipmap-mdpi (48 x 48 px)
* mipmap-xhdpi (96 x 96 px)
* mipmap-xxhdpi (96 x 96 px)

アプリアイコンの背景は透明です。

## 利用規約を更新する

[Terms of Use] セクションはアプリのフッターに表示され、terms_of_use.htmlファイル (**app > assets**にあります) にリンクしています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_termsofusediagram" type="svg" alt="利用規約の図" %}

Terms of Useファイルはサンプルファイルで、アプリの配布前に編集が必要です。たとえば、利用規約、エンドユーザーライセンス契約、プライバシーに関する通知などの法的な通知をこのファイルに含めることができます。

このTerms of Useファイルには、デフォルトでサンプルアプリに組み込まれているオープンソースのコンポーネントに関する通知も記載されています。これらの通知は、あくまで利便性の目的でのみ提供されます。Amazonでは、これらの正確性または完全性についていかなる表明も行いません。また、これらの不正確性または不完全性について一切の責任を負いません。

## 次のステップ

認証、広告、アプリ内課金、または分析を活用するコンポーネントの使用方法について、「[コンポーネントの概要][fire-app-builder-interfaces-and-components]」を参照してください。

{% include links.html %}
