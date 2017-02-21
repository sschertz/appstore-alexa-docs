---
title: Amazon Fire TV用アプリとゲームを開発するための準備
permalink: getting-started-developing-apps-and-games-for-amazon-fire-tv.html
sidebar: firetv_ja
product: Fire TV
toc: false
---

Fire TV向けのアプリを開発する際は、まず、Androidアプリとウェブアプリのどちらを作成するのかを決めます。

*  **Androidアプリ**: Android Java開発者は、既存のツール (Android Studioなど) とフレームワーク (Unityなど) を使用して、10 フィートエクスペリエンス向けのアプリやゲームを開発できます。アプリの開発に役立つサンプルコード、ドキュメント、ガイドラインが用意されています。ストリーミングメディアアプリを開発する場合、[Fire App Builder][fire-app-builder-overview] (JavaベースのAndroidスターターキット) を使用して効率的にアプリを作成することができます。*  **ウェブアプリ**: HTML5 ウェブ開発者は、Amazon WebViewを利用してアプリやゲームを開発できます。開発の成果物として、[HTML5 ウェブアプリ](https://developer.amazon.com/public/solutions/platforms/webapps)、Fire OSポートを使用する[Cordovaアプリ](https://developer.amazon.com/public/solutions/platforms/cross-platform)、[ハイブリッドアプリ](https://developer.amazon.com/public/solutions/platforms/android-fireos/docs/building-and-testing-your-hybrid-app)のいずれかを選択できます。ストリーミングメディアアプリを開発する場合、[Fire TV向けウェブアプリスターターキット][the-web-app-starter-kit-for-fire-tv]を使用して効率的にアプリを作成することができます。

{% include tip.html content="Fire TV向けウェブアプリスターターキット (WASK) とFire App Builderの詳しい比較については、「[Fire TV開発フレームワークの比較][fire-tv-development-framework-comparison]」を参照してください。" %}

加えて、自分のスキルを考慮することも重要です。得意とするのはJavaを使用したAndroid開発でしょうか。それとも、HTML5/ウェブ開発でしょうか。自分の経験とアプリの要件に合った方法を選んでください。

* TOC
{:toc}

## Fire TV向けAndroidアプリ開発

JavaでAndroid開発を行っている方のために、Fire TVでも、Android開発で使われているのと同じツール、IDE、APIが使用されています。まずは、「[Fire App Builder][fire-app-builder-overview]」を参照してください。Amazon Fire TVアプリやAndroidアプリをJavaで開発する方法が、初めての方にもわかりやすく説明されています。Fire App Builderは、(ゲームではなく) ストリーミングメディアTVアプリを意図した設計になっています。

独自のアプリをゼロから開発する場合は、次のトピックを参照してください。

* [開発環境のセットアップ][setting-up-your-development-environment]: 初めてAndroid開発に取り組む場合は、このページが参考になります。
* [ADBを使用してFire TVに接続する][connecting-adb-to-fire-tv-device]: ネットワーク接続またはUSBケーブルを使用し、ADBを介して開発コンピューターをFire TV端末に接続する方法を説明しています。
* [アプリのインストールと実行][installing-and-running-your-app]: アプリストアに申請する前のテスト目的で、Fire TV端末にアプリをインストールして実行し、その後アンインストールします。

Android開発の経験が豊富にある方は、[Fire OSを対象とする開発とAndroidを対象とする開発の違い][amazon-fire-tv-differences-from-android-tv-development]も確認しておいてください。

## Fire TV向けのHTML5 ウェブアプリ開発

日頃、HTML5 を使ったウェブアプリ開発でストリーミングメディアアプリを作成している方は、[Fire TV用ウェブアプリスターターキット][the-web-app-starter-kit-for-fire-tv] (WASK) をご使用ください。WASKは、Fire TV向けに簡単なメディア指向アプリを短時間で作成できるよう支援することを目的としたオープンソースプロジェクトです。このスターターキットには、10 フィートエクスペリエンスに適したユーザーインターフェース設計の例や、Fire TVリモコンのサポートが含まれています。サンプルコンポーネントも用意されており、それを基に独自のメディアアプリを作成してカスタマイズすることができます。

HTML5 のウェブアプリをゼロから開発する場合は、「[Fire TV用ウェブアプリの準備][getting-started-with-web-apps-for-fire-tv]」を参照してください。

## Fire TVアプリに使用されるAPI

Fire TVアプリを作成する際、堅牢性を高めるために他のAmazon APIを実装することもできます。

*  [アプリ内課金API](https://developer.amazon.com/public/apis/earn/in-app-purchasing): Fire TV端末を購入し、Amazonアカウントに登録すると、自動的にAmazon支払いプロファイルが設定され、その他の設定をすることなく、アプリまたはアプリ内アイテムを購入できるようになります。Amazon Fire TVとFire TV Stickは、Amazonアプリ内課金のAPIをサポートしているため、消費可能アイテムや消費不可アイテムのほか、定期購入もアプリ内で販売することができます。
*  [Amazon Fling SDK](/apis/experience/fling/docs/understanding-the-amazon-fling-service): Amazon Fling SDKを使用すると、スマートフォンやタブレットに表示される画面を直接テレビに転送することができます。アプリを 2 画面に拡張することにより、複数のユーザーでアプリを利用することができます。

その他のFire TV APIとSDKについては、「[アプリおよびゲームサービスSDK](/resources/development-tools/sdk)」を参照してください。

## 端末とメディアの仕様

Fire TVでサポートされるメディア、端末、仕様 (ビデオの形式、DRM、コーデック、解像度など) については、「[Fire TV端末の仕様][device-and-platform-specifications]」を参照してください。

## Fire TVのドキュメントについて

Fire TVのドキュメントは、次のグループに分かれています。

*  [準備][getting-started-developing-apps-and-games-for-amazon-fire-tv]
*  [Fire App Builder][fire-app-builder-overview]
*  [ウェブアプリ][getting-started-with-web-apps-for-fire-tv]
*  [カタログの統合][integrating-your-catalog-with-fire-tv]
*  [Fling SDK][understanding-the-amazon-fling-service]
*  [アプリストアへの公開][appstore-understanding-submission]


## Fire TVフォーラム

ご不明な点やご質問、ご意見など、他のユーザーと共有したい事柄がある場合は、Amazon開発者フォーラムの[Fire TVとFire TV Stickのカテゴリ](https://forums.developer.amazon.com/spaces/43/Fire+TV+and+Fire+TV+Stick.html)をご利用ください。

{% include links.html %}
