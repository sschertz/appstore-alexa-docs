---
title: Fire TV開発とAndroid TV開発の違い
permalink: amazon-fire-tv-differences-from-android-tv-development.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Fire TVとAndroid TVは、どちらもAndroidが使用されているため、開発したAndroidアプリをAmazonアプリストアとGoogle Playストアの両方で配信することができます。AmazonストアとGoogleストアの両方で配信することによって、アプリの知名度とダウンロード件数を大幅に向上させることができます。

ただし、Fire TVにはコードの中で考慮すべき違いがいくつかあります。これらの違いは、たとえばハードウェアやサービスを構成する独自の要素に起因しています。一方のサービスには存在する機能がもう一方のサービスにはない場合もあります。異なるサービスではあるものの、同等の働きをするものもあります。

コードの中でこうした違いに対応するためには、[Amazon Fire TV端末を特定](identifying-amazon-fire-tv-devices)し、条件分岐させることで、それに応じた動作をコーディングする必要があります。

* TOC
{:toc}

## Fire TVとは何か、Android TVとは何か

まず、Android TVとFire TVとは何かを明確にしておきましょう。

* **Android TV**とは、テレビ用に最適化されたAndroidオペレーティングシステムをいいます (Lollipop以降)。Android LollipopとLeanback Support Libraryには、テレビのプラットフォームにAndroidを最適化する機能が備わっています。Android TVは、テレビ端末単体でネイティブOSとして実行できるほか、テレビからセットトップボックスを介してAndroidを実行することができます。Android TVの詳細については、[Wikipedia](https://en.wikipedia.org/wiki/Android_TV)を参照してください。
* **Fire TV**とは、ご使用のテレビでFireオペレーティングシステム (OS) を実行するFire TVセットトップボックスまたはFire TV Stickを指します。Fire OSは、Amazonのハードウェアとサービスに対応したAndroidフォークです。Fire OSは、最新のAndroidリリースにほぼ足並みを合わせる形で進化していますが、拡張範囲はLollipopおよびバックポートされたMarshmallowコードまでです(Fire OSはFireタブレットでも使用されていますが、テレビプラットフォームでの "10 フィートエクスペリエンス" に主眼を置いた機能はFireタブレットでは利用されません)。

重要な点は、Android TVとFire TVはどちらもAndroidをベースにしており、アプリ開発において開発者が実装する技術は、相違点よりも共通点の方がはるかに多いということです。

## Android TVとFire TVの違い

以降のセクションでは、Fire TVでの使用を予定しているコードの中で考慮に入れるべき相違点を見ていきます。

### Leanback Support Library

Fire TVでは、AndroidのLeanback Support Libraryが一部サポートされています。Fire TVではLeanbackに含まれているテレビ用のUIコンポーネントが使用されており、Leanbackのウィジェットも動作しますが、Leanbackランチャーのアクティビティをタグで定義しても、アクティビティは動作しません。具体的には、アクティビティカテゴリ[`CATEGORY_LEANBACK_LAUNCHER`](https://developer.android.com/reference/android/content/Intent.html#CATEGORY_LEANBACK_LAUNCHER)がFire TVでは認識されません。

### 音声検索

音声検索に関しては、Android TVではLeanback APIを利用した*アプリコントロール*が使用されています ([SearchFragment](https://developer.android.com/reference/android/support/v17/leanback/app/SearchFragment.html)による音声認識など)。一方、Fire TVでの音声検索には、Amazon独自の*システムコントロール*が使用されています。 

ユーザーはFire TV上の任意の操作画面 (ランチャー、アプリ内など) で、音声対応リモコンのマイクボタンを押し、目的のテレビ番組やAlexaアクションを声に出して言うと、そのアクションによって*グローバル検索*が開始されます。このときに使用されるのはAlexaクラウドサービスであり、Leanbackライブラリの音声認識APIは使用されません。音声によるメディアリクエストでは、常にFire TVカタログからコンテンツが返されます。詳細については、「[Implementing Search in Fire TV](implementing-search-fire-tv)」を参照してください。

### グローバル検索

Android TVでは、目的のコンテンツをグローバル検索に統合したい場合、検索結果のContentProviderをアプリから使用してローカルに実現できます。 

一方、Fire TVでグローバル検索の結果にコンテンツを表示させるには、[メディアのコンテンツをFire TVカタログに統合][integrating-your-catalog-with-fire-tv]する必要があります。カタログへの送信は、(アプリ内からローカルにではなく) クラウドベースモデルで実現されます。

### 早送りボタン、早戻しボタン、メニューボタン

Android TVとFire TVにはどちらも、4 方向のD-Padボタン、D-Pad のCenter/選択ボタン、戻るボタン、再生/一時停止ボタンが備わっています。ただし、Fire TVにはさらに、早戻しボタン、早送りボタン、メニューボタンが備わっています。 

Fire TVの [Menu] ボタンを押すと、Androidのコンテキストメニューが起動し、画面の中央にメニューアイテムのリストが表示されます。[Menu] ボタンをオーバーライドして、独自のカスタムメニューユーザーインターフェースを提供するなどの目的に使用できます。 

メニューアイテムが 1 つだけ必要な場合は、単純な切り替えスイッチ (たとえば、クローズドキャプションのオン/オフ) として [Menu] ボタンを使用し、この機能をユーザーに知らせるためにヒントを画面に表示することもできます。

### Googleのサービス

Google固有のサービスに依存するAPI (位置情報サービスなど) は、Fire TVでは利用できません。[Amazon Maps API](https://developer.amazon.com/public/apis/experience/maps)や[Amazonモバイル広告API](https://developer.amazon.com/public/apis/earn/mobile-ads)は存在しますが、まだFire TVではサポートされていません。

### アプリ内課金

Android TVでのアプリ内課金には、通常、Googleのアプリ内課金機能 (Google In-App Billing) を使用します。Fire TVでのアプリ内課金には、Amazonの[アプリ内課金 (IAP) API](https://developer.amazon.com/public/apis/earn/in-app-purchasing)を使用します。詳細については、[両者の詳しい機能比較を参照](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/migrating-from-googles-iab-to-amazons-iap)してください。

### Firebase Analytics

Android TVでは、分析ソリューションとしてFirebaseが使用されています。Fire TVでは、[Amazon Mobile Analytics](https://aws.amazon.com/mobileanalytics/)やその他の分析パッケージ (Google Analytics、Flurry Analytics、Crashlyticsなど) を使用できます。

### SDKレベル

Android TVでは最新のSDK (Nougat) が使用できるのに対し、Fire TVではLollipopのみが最小SDKレベルとして使用されます(特定のアプリをサポートするために、Fire OSにはMarshmallowの一部のAPIがバックポートされています)。 

### おすすめの表示

Android TVでは、アプリからランチャーでおすすめの表示を行うことができます。Fire TVにはRecommendations APIが存在するものの、現時点では機能しません。つまり、Fire TVランチャー上のコンテンツをアプリから制御することはできません (ランチャーのおすすめカテゴリに対する登録は、一部の限られたアプリで間もなく対応予定となっていますが、現時点では実装されていません)。

### エミュレータ

開発したFire TVアプリのコードをテストするときに使用するのは、通常、仮想エミュレータではなく実際のFire TV端末です。詳細については、「[ADBを使用してFire TVに接続する][fire-app-builder-connecting-adb-to-fire-tv]」を参照してください。

### 通知API

Fire TVアプリで通知を作成するには、標準の[Android通知API](http://developer.android.com/reference/android/app/Notification.html)を使用します。Fire TVには、Android TVと同じトースト通知と永続化モデルが備わっています。ただし、Fire TVはトーストの他にもHeads up (高優先度) 通知に対応しており、それらの通知にはインタラクティブなボタンが利用できます。さらに、古い通知は、通知ドロワーには格納されずに通知センターに保存されます。詳細については、「[Notifications for Amazon Fire TV][notifications-for-amazon-fire-tv]」を参照してください。

### ユーザー補助

Fire TVには[VoiceView](https://www.amazon.com/b?node=14100715011)が備わっており、目の不自由な方でも利用しやすいようにアプリを開発することができます。VoiceViewとユーザー補助の詳細については、次のページを参照してください。
 
*  [Understanding Assistive Technologies for Fire OS](https://developer.amazon.com/appsandservices/solutions/platforms/fire-os/docs/implementing-accessibility-in-fireos)
*  [Implementing Accessibility in Fire OS](https://developer.amazon.com/appsandservices/solutions/platforms/fire-os/docs/implementing-accessibility-in-fireos)

### アプリストア 

Android TV端末では、Google Playストアが使用されます。これに対してFire TVでは、Amazonアプリストアが使用されます。Google Playストアへのリンクはすべて、Amazonアプリストアへのリンクに変更する必要があります。

{% include links.html %}
