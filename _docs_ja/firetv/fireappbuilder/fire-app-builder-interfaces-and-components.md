---
title: コンポーネントの概要
permalink: fire-app-builder-interfaces-and-components.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire App Builderには、インターフェースを実装する多くのコンポーネントが用意されています。これらのインターフェースは、Fire App Builderのメソッドとフィールドを定義しているため、コンポーネントは、コードが完全に開発されたインターフェースを実装します。 

コンポーネントを使用するために独自のJavaコードを記述する必要はありません。必要なのは、コードからXMLファイルに抽出されたさまざまな文字列値のカスタマイズのみです。

主なインターフェースには、以下が含まれます。

* **広告インターフェース** (`IAds`): ユーザーに広告を表示するために使用されます。
* **分析インターフェース** (`IAnalytics`): 分析を実装するために使用されます。
* **認証インターフェース** (`IAuthentication`): ユーザーのメディアに対するアクセスを認証または許可するために使用されます。
* **購入インターフェース** (`IPurchase`): アプリ内課金に使用されます。
* **UAMP** (`UAMP`): メディアの再生に使用されます。

使用できないコンポーネントが必要な場合は、いずれかのインターフェースを実装する独自のコンポーネントを作成できます。

* TOC
{:toc}

## 利用可能なコンポーネントとインターフェース {#componentgroups}

Android Studioでは、すべてのコンポーネントが表示されるわけではありません。Fire App Builderサンプルアプリでは、アプリのGradleファイルでの定義に従って、Fire App Builderライブラリから利用可能なコンポーネントの一部のみがロードされるためです。各インターフェースからコンポーネントが 1 つだけロードされます。

ディスクで利用可能なコンポーネントを表示するには、コンピューターでFire App Builderプロジェクトをダウンロードしたフォルダーを参照する (Finderまたはエクスプローラーを使用) か、ここに記載されている情報を参照してください。

## 認証コンポーネント {#authenticationcomponents}

認証コンポーネントを使用すると、ユーザーがアプリで操作を行う前に、そのユーザーの身元を確認して権限を付与することができます。認証コンポーネントは、アプリに少なくとも 1 つロードしておく必要があります。認証コンポーネントは`IAuthentication`インターフェースを実装します。これはAuthInterfaceフォルダーで確認できます。次の認証コンポーネントが利用可能です。

| コンポーネント | 説明 |
|---------|----------|
| [AdobepassAuthComponent][fire-app-builder-adobe-pass-auth-component] | Adobe Passを使用してユーザーを認証します。|
| [FacebookAuthComponent][fire-app-builder-facebook-auth-component] | Facebookを使用してユーザーを認証します。デフォルトでは、このコンポーネントはFire App Builderサンプルアプリにロードされます (ただし、構成されていないため、ユーザーにはプロンプトが表示されません)。|
| [LoginWithAmazon Component][fire-app-builder-login-with-amazon-component] | Login with Amazonを使用してユーザーを認証します。|

<!--| PassThroughLoginComponent | その他の認証コンポーネントを使用しない場合にのみ使用されます。アプリには、少なくとも 1 つの認証コンポーネントをロードする必要があります。このPassThrough Loginコンポーネントは、アプリのユーザーエクスペリエンスに影響を及ぼすことなく、認証コンポーネントの要件を満たします。|-->

{% include note.html content="ログインコンポーネントを使用しない場合は、FacebookAuthComponentを、アプリのcustom.xmlファイルでこのコンポーネントに汎用の値を使用したコンポーネントとして構成された状態にしておいてください。Fire App Builderには認証コンポーネントが必要なため、(広告コンポーネントのPassThroughAdsのような) ダミーの認証コンポーネントはありません。この方法では、Facebookに関連した情報がユーザーに表示されません。" %}

## 分析コンポーネント {#analyticscomponents}

分析コンポーネントでは、アプリ内での行動を追跡し、分析用の指標を提供します。分析コンポーネントは、AnalyticsInterfaceフォルダーの`IAnalytics`インターフェースを実装します。アプリに分析コンポーネントをロードすることは必須ではありません。

| コンポーネント | 説明 |
|---------|----------|
| [FlurryAnalyticsComponent][fire-app-builder-flurry-analytics-component] | Flurry Analyticsを使用して分析を行います。デフォルトで、このコンポーネントはFire App Builderサンプルアプリにロードされますが、構成されていません。|
| [OmnitureAnalyticsComponent][fire-app-builder-omniture-analytics-component]|  Omnitureを使用して分析を行います。|
| [CrashlyticsComponent][fire-app-builder-crashlytics-component] | Crashlyticsを使用して分析を行います。|
| LoggerAnalyticsComponent |  Logger Analyticsコンポーネントは、実際の分析に使用することを目的としていません。これは、イベント中にログメッセージを挿入するダミーの分析コンポーネントです。アプリでは少なくとも 1 つの分析コンポーネントを構成する必要があるため、他の分析コンポーネントを使用していない場合にLogger Analyticsコンポーネントを使用します。LoggerAnalyticsコンポーネントによって発生したログメッセージは、logcatで "loggeranalytics" を検索すると確認できます。|

## 広告コンポーネント {#adscomponents}

広告コンポーネントとは、アプリで広告を表示するすべてのコンポーネントを指します。広告コンポーネントは、AnalyticsInterfaceフォルダーの`IAds`インターフェースを実装します。アプリに広告コンポーネントをロードすることは必須です。次の広告コンポーネントが利用可能です。

| コンポーネント | 説明 |
|---------|----------|
| [FreeWheelAdsComponent][fire-app-builder-freewheel-ads-component] |  アプリでFreewheel広告を表示するために使用されます。|
| [VastAdsComponent][fire-app-builder-vast-ads-component] | アプリでVAST広告を表示するために使用されます。| | PassThroughAdsComponent | その他の広告コンポーネントを使用しない場合にのみ使用されます。アプリには、少なくとも 1 つの広告コンポーネントをロードする必要があります。このPassThroughAdsコンポーネントは、アプリのユーザーエクスペリエンスに影響を及ぼすことなく、広告コンポーネントの要件を満たします。|

上記の表に記載されている認証コンポーネントを使用しない場合は、PassThroughAdsComponentを使用します。アプリには、少なくとも 1 つの広告コンポーネントをロードしておく必要があります。このダミーコンポーネントは、アプリのユーザーエクスペリエンスに影響を及ぼすことなく、広告コンポーネントの要件を満たします。

## 課金コンポーネント {#purchasingcomponents}

課金コンポーネントを使用すると、ユーザーは、アプリでコンテンツをレンタルまたは購読するために支払いを行うことができます。課金コンポーネントは、PurchaseInterfaceフォルダーの`IPurchase`インターフェースを実装します。課金コンポーネントのロードは必須です。ただし、このコンポーネントをアクティブにしたくない場合は、アクティブにしなくてもかまいません。

次の課金コンポーネントが利用可能です。

| コンポーネント | 説明 |
|---------|----------|
| [AmazonInAppPurchasingComponent][fire-app-builder-amazon-in-app-purchase-component] |  Amazonアプリ内課金を使用して、ユーザーがアプリでメディアを視聴するためにDaily Pass (24 時間アクセス) またはGo Premium (購読) を選択できるようにします。このコンポーネントはデフォルトでロードされますが、無効に設定されていて、構成もされていません。|

## メディアプレーヤーコンポーネント {#mediaplayercomponents}

メディアプレーヤーコンポーネントは、アプリでのメディア再生に使用されるメディアプレーヤーを制御します。メディアコンポーネントは、UAMPフォルダーの`UAMP`インターフェースを実装します (UAMPは、Universal Android Media Playerの略です)。メディアコンポーネントのロードは必須です。次のメディアプレーヤーコンポーネントが利用可能です。

| コンポーネント | 説明 |
|---------|----------|
| [AMZNMediaPlayerComponent][fire-app-builder-amazon-media-player-component] | ExoPlayerをベースにしたAmazon Media Playerを使用します。このコンポーネントはデフォルトでFire App Builderにロードされます。|

<!--| [BrightCoveMediaPlayerComponent][fire-app-builder-brightcove-media-player-component] | BrightCoveメディアプレーヤーを使用します。|-->

## UIコンポーネント

TVUIComponentは省略可能なコンポーネントではありません。このコンポーネントには、アプリのユーザーインターフェースと機能 (メディアプレーヤー機能以外) のほとんどが含まれています。これは、タブレットなどの他の端末用にさらにUIコンポーネントを追加できるようにするためのコンポーネントとして作成されています。 


{% include links.html %}
