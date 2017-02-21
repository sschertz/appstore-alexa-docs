---
title: アプリを作成するための一連のプロセス
toc: false
permalink: fire-app-builder-end-to-end-process.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---
<style>
th {
background-color: #444;
color: white;
font-weight: bold;
}
</style>


次の手順では、Fire App Builderでアプリを作成するための一連のプロセスを示しています。すでに[要件][fire-app-builder-overview#requirements]を満たしている場合は、以下の各セクションのトピックを完了するとアプリを作成できます。

|1. 準備する |
|---|-----|
| a. [Fire App Builderをダウンロードして、アプリをビルドする][fire-app-builder-download-and-build] | Fire App Builderのコードをダウンロードし、Fire App Builderで "アプリケーション" をビルドします。その後、Fire App Builderのクローンを作成して、独自のアプリを作成します。|
| b. [ADBを使用してFire TVに接続する][fire-app-builder-connecting-adb-to-fire-tv] | アプリをテストできるように、ADBを使用してFire TV端末にコンピューターを接続します。|
| c. [アプリの概要][fire-app-builder-app-tour] | Fire App Builderのサンプルアプリの基本的な機能と画面について理解を深めます。|

| 2. フィードを構成する |
|---|
| a. [メディアフィードをロードする][fire-app-builder-load-media-feed] | アプリにメディアフィードをロードします。フィードには、タイトル、説明、サムネイル、メディアオブジェクトなど、すべてのメディアアセットが含まれます。|
| b. [レシピ構成の概要][fire-app-builder-set-up-recipes-overview] | Fire App Builderに含まれているレシピと、構成の要件について説明します。|
| c. [カテゴリレシピをセットアップする][fire-app-builder-set-up-recipes-categories] | Fire App Builderでフィードのカテゴリを読み取る方法を構成します。カテゴリにより、コンテンツがさまざまなグループに分類されます。|
| d. [コンテンツレシピをセットアップする][fire-app-builder-set-up-recipes-content] | Fire App Builderでフィードのコンテンツを読み取る方法を構成します。コンテンツとは、タイトル、説明、ビデオのURLなど、フィードのすべての要素のことです。|
| e. [Navigatorの構成の概要][fire-app-builder-configure-navigator] | Navigatorファイルの役割と、構成が必要な項目について説明します。|
| f. [Navigatorの構成 -- トークンベースフィード][fire-app-builder-configure-navigator-token-feeds] | カテゴリレシピとコンテンツレシピをアプリのUIの画面に関連付けます。アクセスするためにトークンが必要なフィードの場合は、この手順に従います。|
| g. [Navigatorの構成 -- オープンフィード][fire-app-builder-configure-navigator-open-feeds] | カテゴリレシピとコンテンツレシピをアプリのUIの画面に関連付けます。トークンなしでオープンにアクセスできるフィードの場合は、この手順に従います。|

| 3. アプリの外観をカスタマイズする |
|---|
| a. [アプリのルックアンドフィールをカスタマイズする][fire-app-builder-customize-look-and-feel] | custom.xmlファイルを使用して、アプリの外観をカスタマイズします。フォントから背景色、ホームページのレイアウト、スプラッシュ画面などまで、アプリのほぼすべての要素をカスタマイズできます。|

| 4.コンポーネントを追加する |
|---|
| a. [コンポーネントの概要][fire-app-builder-interfaces-and-components] | Fire App Builderのインターフェースを実装する、すでにコーディングされたコンポーネントをロードすることで、認証、アプリ内課金、分析、広告、またはMedia Playerを設定します。|

アプリの開発が完了し、Amazonアプリストアにそのアプリを申請する準備ができたら、「[Understanding Amazon Appstore Submission][appstore-understanding-submission]」を参照してください。アプリの公開を準備する場合は、[さまざまなスクリーンショットを撮影](/support/submitting-your-app/tech-docs/04-taking-screenshots)し、[Fire TVの画像アセット][asset-guidelines-for-app-submission#firetvassets]を収集する必要があります。

{% include links.html %}
