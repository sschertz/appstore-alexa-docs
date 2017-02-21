---
title: Amazon Fire TV開発フレームワークの比較
sidebar: firetv_ja
product: Fire TV
permalink: fire-tv-development-framework-comparison.html
hippourl: https://developer.amazon.com/public/solutions/devices/fire-tv/docs/fire-tv-development-framework-comparison
reviewers: Russell Beattie, Mihir Choudhary, Jonathan Richardson, Pete Schwab, Mary Galvin, Chris DeNamur, Mario Mancia, Luca Sale, Stephen Whitney, Alexander Budyszewick
last_updated: 12-13-2016
toc: false
---

Amazon Fire TVを対象としたメディアベースのアプリ開発を検討している方のために、Amazonでは、アプリ開発を強力に支援する 2 つのフレームワークを用意しています。それぞれのフレームワークは、特定のスキルを持った開発者が対象となっています。

* [Fire TV用ウェブアプリスターターキット](#wask): HTML5、CSS3、JavaScriptを使ってウェブアプリを構築するウェブ開発者向けです。
* [Fire App Builder](#fab): Javaを使ってネイティブアプリを構築するAndroid開発者向けです。

この 2 つのフレームワークは、使用するコードの種類に違いがあるほか、備わっている機能も若干異なります。詳細については、「[機能の比較](#feature_comparison)」を参照してください。

* TOC
{:toc}

## Fire TV用ウェブアプリスターターキット {#wask}

Fire TV用ウェブアプリスターターキット ([Github](https://github.com/amzn/web-app-starter-kit-for-fire-tv)からダウンロード可能) は、Fire TVを対象とするメディア指向アプリをHTML5 やCSS3、JavaScriptで開発する際の基礎となるキットです。このキットで開発したウェブアプリをAmazon開発者ポータルでパッケージすることで、Fire TV対応アプリを作成できます。ネイティブアプリとまったく同様に、Amazonアプリストアで公開することができます。

Fire TV用ウェブアプリスターターキット (WASK) を使用した開発は、アプリテンプレートを基にして行うことができます。Media RSSフィードやJSONフィードのサポート、オンラインビデオプロバイダー (YouTube、Brightcoveなど) のサポートなど、各種メディア機能がテンプレートに含まれています。このテンプレートは、標準のウェブテクノロジー (JavaScript、HTML5、CSS3) を使って拡張機能を追加したり、設定ファイルを変更したりすることによってカスタマイズできます。

WASKテンプレートには、アプリ申請プロセスでAmazonアプリストアのテストに合格するために必要な要素に加え、ユーザーが大画面に期待する利便性を実現するうえで必要なコードがあらかじめ記述されています。 最低でも、メディアファイルのフィードを指定するだけでアプリを開発できます。アプリは、このメディアファイルを使用してカテゴリの選択リストを表示したり、回転式のスライドショーでメディアコンテンツを表示したりすることが可能です。

単純なレイアウトのスクリーンショットを以下に示します。

{% include image.html file="firetv/getting_started/images/wask_simple_layout" type="png" %}

基本的なWASKテンプレートをカスタマイズまたは拡張する際は、[Amazon Web App Tester](https://developer.amazon.com/public/solutions/platforms/webapps/docs/tester.html)を使ってアプリをテストできます。Amazon Web App Testerは、ウェブアプリを実際の端末でテストするためのFire TV対応アプリです。Web App Testerには、アプリが公開されたときに使用されるネイティブアプリラッパーとウェブエンジンがそのまま使用されていて、開発段階で正確にアプリの動作を確認することができます。

完成したアプリは、Amazon開発者ポータルからAmazonアプリストアに申請すれば、数分後には公開されます。この点に関して直接コードを記述する必要はありません。

オンラインでサインアップを行い、アプリに関する基本的な情報を入力してサムネイルやプレビュー画像をアップロードしたら、アプリのホスト先を選ぶことができます。アプリのアセットファイルを自社のウェブサーバーでホストし、そのURLだけを申請する方法と、Amazonのサーバーにアセットをアップロードし、そこでスタンドアロンのパッケージアプリとしてバンドルする方法があります。

申請したアプリはその後、Amazonのインジェストサービスを経ることになります。アプリが公開されると申請者に通知が送られます。

詳細については、「[Fire TV用ウェブアプリスターターキット][the-web-app-starter-kit-for-fire-tv]」を参照してください。WASKを使って作成されたFire TV対応アプリの例としては、[Acorn TV][acorn-tv]、[Urban Movie Channel][urban-movie-channel]、[Euronews][euronews]があります。

## Fire App Builder {#fab}

Fire App Builder ([Github](https://github.com/amzn/fire-app-builder)からダウンロード可能) には、Amazon Fire TVを対象としたストリーミングメディアAndroidアプリを短時間で簡単に開発できるJavaベースのフレームワークが備わっています。WASKではHTML5/CSS3/JSが使用されるのに対し、Fire App Builderで使用されるのはJava Androidコードです。

Fire App BuilderはAndroid Studioで使用します。Android Debug Bridge (ADB) を介してFire TV端末に接続し、APK (Android Package Kit) ファイルを生成してAmazonアプリストアにアップロードします。

Fire App BuilderではAndroidのAPI (特にLeanback Library) が使用されていますが、大半の設定とカスタマイズはJSONファイルとXMLファイルで行うことができます。たとえば、JSONファイルとXMLファイルで 10 種類以上のコンポーネントを設定し、アプリに追加することができます。各種コンポーネントには、分析、広告、承認、購入、メディアプレーヤーの機能があらかじめ搭載されています。

Fire App Builderは、Javaに関する専門的な知識に極力依存しないように設計されていますが、深いレベルで統合したい場合には、Fire App Builderを基盤として開発を進めることもできます。共通のインターフェースや他のコードから、カスタムのJavaクラスを追加して独自に機能を拡張することが可能です(Javaのカスタムプログラミングを望まない場合は、しなくても問題ありません)。

Fire App Builderでは、メディアフィードとして、JSONまたはXMLを使用できます。使用するデータの構造やタグ名に決まりはありません。Fire App Builderの設定作業では、対象となるフィードの各種要素をクエリ構文 (JSON Jayway構文またはXPath式) で記述します。

フィードには、DRMで保護されているメディアのトークンが必要になることもあります。YouTubeベースのフィードなど、各種ビデオホスティングサービスへの対応が予定されていますが、現時点ではまだ実装されていません。

色、レイアウト、タイポグラフィなどは自由に調整できます。いずれも、対応する設定が抽出されたXMLファイルまたはJSONファイルを編集することによって行います。

以下に示したのは、Fire App Builderを使って開発されたサンプルアプリのスクリーンショットです。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" %}

さらに簡潔なホームページレイアウトも用意されています。

詳細については、[Fire App Builderのドキュメント][fire-app-builder-overview]を参照してください。Fire App Builderで作成されたサンプルアプリについては、[Hallmarkアプリ][hallmark]を参照してください。


## 機能の比較 {#feature_comparison}

以下の表は、Fire App Builderの機能とWASKの機能を比較したものです。

{% include note.html content="特定の機能がフレームワークにない場合、その機能はフレームワークではサポートされません。言い換えれば、その機能はまだコードに組み込まれていない状態です。そうした機能に対応するために用意されたサードパーティのコードは、多くの場合、簡単に挿入することができます。" %}

<table class="grid">
<colgroup>
<col width="30%" />
<col width="30%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr>
<th style="text-align:center">カテゴリ</th>
<th style="text-align:center">機能</th>
<th style="text-align:center">Fire App Builder</th>
<th style="text-align:center">WASK</th>
</tr>
</thead>
<tbody>
<tr>
  <td class="white" rowspan="2"><b>コードベース</b></td>
  <td class="white">Java/Android</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td>HTML5/CSS3/JS</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td rowspan="3" class="gray"><b>フィードの形式</b></td>
  <td class="gray">JSONフィード</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">Media RSS XMLフィード</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">カスタムXMLフィード</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td rowspan="2"><b>アプリ配信オプション</b></td>
  <td class="white">端末にAPKとしてインストール</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">URLから直接アプリをホスト</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="3"><b>メディアの種類</b></td>
  <td class="gray">HLS, DASH, Smooth Streaming, MP4</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray">DRMで保護されたメディア</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">ライブストリーム</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white" rowspan="4"><b>メディアプロバイダー</b></td>
  <td class="white">YouTube</td>
  <td class="white"><div style="text-align: center"></div></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Brightcove</td>
  <td class="white"><div style="text-align: center"></div></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Kaltura</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white">Ooyala</td>
  <td class="white"></td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="2"><b>メディアプレーヤー</b></td>
  <td class="gray">Amazon Media Player</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Brightcove</td>
  <td class="gray"><div style="text-align: center"></div></td>
  <td class="gray">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="white"><b>課金</b></td>
  <td class="white">アプリ内課金</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white">{{site.data.code.check}}</td>
</tr>
<tr>
  <td class="gray" rowspan="3"><b>認証</b></td>
  <td class="gray">Login with Amazon</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Facebook Authorization</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Adobe Primetime</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>

<tr>
  <td class="white" rowspan="2"><b>広告サービス</b></td>
  <td class="white">Freewheel広告</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td class="white">VAST広告</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
<tr>
  <td class="gray" rowspan="4"><b>分析</b></td>
  <td class="gray">Omniture Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Google Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Crashlytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="gray">Flurry Analytics</td>
  <td class="gray">{{site.data.code.check}}</td>
  <td class="gray"></td>
</tr>
<tr>
  <td class="white"><b>グローバルカタログ検索</b></td>
  <td class="white" markdown="span">[Fire TVカタログ][integrating-your-catalog-with-fire-tv]統合によるグローバル音声検索</td>
  <td class="white">{{site.data.code.check}}</td>
  <td class="white"></td>
</tr>
</tbody>
</table>

すでに述べたように、どちらのフレームワークにも必要なサービスや機能を追加することができます。コードは開放されており (オープンソース)、フレームワークのコードをベースに機能強化や拡張など、各種開発を自由に行うことができます。

## ウェブアプリからAndroidアプリへの移行

企業によっては、まず (WASKで) ウェブアプリを作成し、後で (Fire App Builderを使用するなどして) Java Androidアプリに移行するよう計画する場合もあります。アプリの種類 (ウェブアプリかAndroidアプリか) は、アプリストアへの申請時に選択します。

アプリの申請後は、別の種類に移行することができません。最初にウェブアプリとして申請したら、新しいバージョンをAndroidアプリとしてアップロードすることはできません。まったく別のアプリをアップロードする必要があり、結果として既存のユーザーを失うことにつながります。

そこで、Androidアプリへの移行が予定されているウェブアプリには、[Cordova](https://cordova.apache.org/)の使用を検討してください。Cordovaを使用すると、ウェブアプリをAPKとしてラップし、Androidアプリとして申請することができます。後で完全にネイティブなAndroidアプリに移行することになった場合でも、新しいバージョンとしてアプリストアで公開することができます。

[acorn-tv]: https://www.amazon.com/RLJ-Entertainment-Acorn-TV/dp/B01EAY1XPW/ref=sr_1_1
[urban-movie-channel]: https://www.amazon.com/RLJ-Entertainment-Urban-Movie-Channel/dp/B016APTPSQ/ref=sr_1_1
[euronews]: https://www.amazon.com/Euronews-in-English/dp/B01LFEYRXA/ref=sr_1_1
[hallmark]: https://www.amazon.com/Hallmark-Channel-Everywhere-Fire-TV/dp/B018F20I42/ref=sr_1_1

{% include links.html %}
