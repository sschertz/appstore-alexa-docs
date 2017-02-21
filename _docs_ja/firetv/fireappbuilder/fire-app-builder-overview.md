---
title: "Fire App Builder: JavaベースのAmazon Fire TVおよびAndroidアプリ向けスターターキット"
permalink: fire-app-builder-overview.html
sidebar: fireappbuilder_ja
product: Fire App Builder
---

Fire App Builderに用意されているJavaベースのフレームワークを使用すると、Amazon Fire TV向けのストリーミングメディアAndroidアプリを短時間で簡単に開発できます。 

Fire App Builderでは、ベストプラクティスや各種テクニックをもとにして、Fire TVでの魅力的かつ高品質なメディア体験を構築できるため、すべてのコードを自分で開発する必要はありません。Fire App BuilderのコードはJavaをベースにしており、Android StudioやGradleなど、Androidアプリの開発に広く使用されているツールが使用できます。 

Fire App Builderは、Apache 2.0 ライセンスに基づくオープンソースプロジェクトとして、Github ([github.com/amzn/fire-app-builder](https://github.com/amzn/fire-app-builder)) でリリースされています。

* TOC
{:toc}

## Fire App Builderの操作方法

Fire App Builderでアプリを作成する場合は、複数のJSONファイルを通じてデータフィード、画面レイアウト、および各機能の設定を構成します。また、メディアフィードからカテゴリとコンテンツを取得するクエリ構文の作成も行います。

認証、広告、分析、またはアプリ内課金用に、インターフェースを実装するさまざまなコンポーネントをプラグインとして使用できます。フォント、色、ロゴ、レイアウト、その他の詳細など、アプリのルックアンドフィールをカスタマイズする際は、(Javaで直接コーディングするのではなく) XMLファイルかJSONファイルの一部の値を更新するだけで済みます。 

このように、Fire App Builderでは、Javaプログラミングを行わずに高品質なアプリを短時間で開発できます。Fire App Builderのコンポーネントの大部分はモジュール形式であるため、より高度な機能でFire App Builderを拡張したい場合は、Fire App Builderを基礎フレームワークとして使用し、その上に高度な機能を構築することができます。 

## Fire App Builderのサンプルアプリ

Fire App Builderには、次のようなホーム画面を持つ "Application" という名前のサンプルアプリが用意されています。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="Fire App Builderによるサンプルアプリ" caption="Fire App Builderで構築されたサンプルアプリのホーム画面。" %}

Fire App Builderのサンプルアプリには、Lightcastによる汎用のビデオフィードが含まれており、テスト目的に限定して使用できます。

## Fire App Builderの対象者

Fire App Builderは、NetflixやHuluのようにストリーミングメディアアセットを保有し、自社のコンテンツをFire TVやその他のAndroid TVプラットフォームを通じてオンラインで提供したいと考えている企業向けに設計されています。メディアアセット (映画やテレビ番組などのビデオコンテンツ) を公開するためのビデオフィードがある場合には最適な開発ツールです。 

メディアフィードにはJSONまたはXMLを使用できますが、YoutubeチャネルやVimeoチャネルではなく独自のフィードを用意する必要があります(XMLの場合は、iTunesに送信するようなメディアRSSフィードを使用できます)。フィードはどのような構造でもかまいません。フィードからカテゴリとコンテンツを選択するにはクエリ構文を使用します。 

また、Fire App BuilderではAndroid Studioを使用してファイルを構成する必要があるため、(HTML5 ウェブテクノロジーではなく) JavaベースのAndroidを使用したアプリ制作を好むような開発者が対象となります。Fire App Builderのフレームワークを基盤として、より高性能のアプリを作成することもできます。Fire App Builderは、本質的にはAndroid Java開発者向けのFire TV SDKです。
 
コーディングではなくコンテンツ制作を担当している場合や、自分のYoutubeビデオ用のFire TVアプリを構築したい場合、またはAndroid Studioでのコーディングに慣れていない場合 (プログラミングスキルは不要ですが) は、代わりに[Fire TV用ウェブアプリスターターキット](the-web-app-starter-kit-for-fire-tv)の使用を検討してください。

## Fire App Builderの使用条件 {#requirements}

Fire App Builderで開発を行うには、以下のものが必要になります。

* **[Android Studio](http://developer.android.com/sdk/index.html)**。お使いのコンピューターにAndroid Studio開発環境をセットアップする方法については、Androidドキュメントの「[Android Studioを使い始める](https://developer.android.com/sdk/installing/studio.html)」と「[Android Studioのインストール](https://developer.android.com/sdk/installing/index.html)」を参照してください。
* **[Java Development Kit (JDK) 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)**。コンピューターに[Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 以降がインストールされている必要があります。
* **[Fire TV](https://www.amazon.com/dp/B00U3FPN4U)または[Fire TV Stick](https://www.amazon.com/Amazon-Fire-TV-Stick-Streaming-Media-Player/dp/B00GDQ0RMG)**。実際のFire TV端末、つまりFire TVまたはFire TV Stickでアプリをテストする必要があります(エミュレーターでもテストできますが、常に動作するとは限らず、またFire TV開発用としてはサポートされていません)。パフォーマンスはFire TVの方が優れているため、お持ちのメディアがリソースを多く消費する場合は、Fire TV Stickでも正常に再生されるか確認するとよいでしょう(Fire TVにはHDMIケーブルが付属していないため、Fire TVボックスをテレビに接続するにはHDMIケーブルを用意する必要があります)。
* **HDMIポート付きのテレビ**。Fire TVを接続できるHDMIポート付きのテレビが必要になります。
* **A-to-A USBケーブル**。(Fire TV Stickではなく) Fire TVを使用しており、(ワイヤレスネットワーク経由ではなく) USB経由でコンピューターをFire TVに接続する場合は、A-to-A USBケーブルが必要になります。ケーブルを使用する代わりに、ネットワーク経由でFire TV端末に接続することもできます(詳細については、「[ADBを使用してFire TVに接続する][fire-app-builder-connecting-adb-to-fire-tv]」を参照してください)。
* **必要な要素を含むメディアフィード。** ビデオアセットと次のフィード要素が含まれたメディアフィード (JSON形式またはXML形式) が必要になります: タイトル、ID、説明、URL、カード画像、背景画像(カードと背景の両方に同じ画像を使用することができます)。[Exoplayer](https://google.github.io/ExoPlayer/supported-formats.html)でサポートされているビデオ形式は、Fire App Builderとも互換性があります。

## Fire App Builderの機能

Fire App Builderは以下の機能を備えています。

* **5 種類の画面**: スプラッシュ画面、ホーム (2 種類のレイアウト)、Content Details、Content Renderer、Search。
* **検索機能と検索結果**: アプリ内でのテキスト検索。メディアがFire TVカタログに統合されている場合にグローバルFire TV検索に組み込むインテントフィルターも備えています。
* **Exoplayerベースのストリーミングメディア用Amazon Media Player**: このメディアプレーヤーには、クローズドキャプション、HTTPライブストリーミング (HLS)、帯域幅設定など、数多くの機能が搭載されています。
* **広告、分析、承認、およびアプリ内課金用のコンポーネント**: 10 種類以上のコンポーネントをプラグインとして簡単にアプリに適用でき、XMLファイルを通じて構成できます。これらのコンポーネントには、Amazonアプリ内課金、Login with Amazon、Facebookログイン、Omniture Analytics、Flurry Analytics、Adobe Pass認証、Freewheel広告、VAST 2.0 広告などがあります。

## Fire TVの用語と端末

"[Fire TV](https://www.amazon.com/dp/B00U3FPN4U)" はFire TVボックスを表し、"[Fire TV Stick](https://www.amazon.com/dp/B00ZV9RDKK)" はスティックバージョンのFire TVを表します。 

「[端末およびプラットフォームの仕様][device-and-platform-specifications]」で各端末の仕様を比較できます。処理能力と処理速度はFire TVの方が優れていますが、2 つの端末はどちらも同じFire OSを搭載しており、エンドユーザーの観点ではほとんど差がありません。各端末には 2 つの世代があり、第 2 世代が最新です。 

Fire TVは、[Gaming Edition](https://www.amazon.com/Amazon-Fire-TV-Gaming-Edition-Streaming-Media-Player/dp/B00XNQECFM)も提供されています。また、専用の[ゲームコントローラ](https://www.amazon.com/Amazon-Fire-Game-Controller-Alexa/dp/B00NO8LX7E)を購入してFire TV Stickとともに使用することもできます。Fire TV Stick (第 2 世代) にはデフォルトでVoice Remoteが付属していますが、Fire TV Stick (第 1 世代) ではVoice Remoteがオプションとして提供されます。

## 始めるにあたって開始

Fire App Builderを使用してアプリ開発を始めるには、Fire App Builderによるアプリ開発に必要な手順がまとめられている「[アプリを作成するための一連のプロセス][fire-app-builder-end-to-end-process]」を参照してください。

ドキュメントに記載された手順に従う場合は、通常、Android Studioの**Androidビュー**で操作していることが前提となっている点に注意してください。特定のフォルダーやパスが見つからない場合は、どのビューで操作しているかを確認してください。

また、**Shift**キーを 2 回押してファイル名を入力すると、任意のファイルを検索することもできます。ファイルをロードすると、上部のナビゲーションバーにあるボタンの列のすぐ下にファイルへのパスが表示されます。

## プロジェクトの更新データを取得する

アプリケーションの更新データを取得するには、[Fire App Builder Githubリポジトリ](https://github.com/amzn/fire-app-builder)をwatchし、`git pull`コマンドを実行して最新の更新データを取得します。詳細については、「[
Githubから更新データをプルする][fire-app-builder-pulling-updates-from-github]」を参照してください。

## サポートを受ける

Fire App Builderに関するフィードバックまたは不明点がある場合は、[Fire TVに関するAmazonフォーラム](https://forums.developer.amazon.com/spaces/166/index.html)を通じてサポートを受けることができます。

## バグの指摘または機能に関する要望

バグの指摘や機能に関する要望は、[Fire App Builder Githubリポジトリ](https://github.com/amzn/fire-app-builder)の [Issues] タブから提出できます。または、[Fire TVに関するAmazonフォーラム](https://forums.developer.amazon.com/spaces/166/index.html)に投稿することもできます。


{% include links.html %}
