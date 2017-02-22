---
title: Fire TV開発における最新情報
permalink: whats-new-in-the-fire-tv-sdk.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

* TOC
{:toc}

### Fire TVのユーザーインターフェースの更新

Fire TV第 2 世代端末のインターフェースが更新されました。ビデオのトレーラーやコンテンツのスクリーンショットが表示され、視聴者は観たい内容にすばやくアクセスできるようになり、これまで以上に魅力的な映画体験が得られます。ホームページのお勧めコンテンツ行にサードパーティ製のアプリを表示することもできます。ユーザー補助機能は、VoiceViewスクリーンリーダーを使ってFire TVや対応アプリを利用できるように強化されています([プレスリリース](http://phx.corporate-ir.net/phoenix.zhtml?c=176060&p=irol-newsArticle&ID=2206525)を参照)。

最新のUIに必要な新しいFire TVアセットをアップロードする方法については、「[Image Guidelines for App Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets)」を参照してください。第 1 世代の端末には、このユーザーインターフェースの更新が少し遅れて適用されます。

### Fire App Builder

2016 年 9 月 29 日に[Fire App Builder][fire-app-builder-overview]がリリースされました。Fire App Builderには、Javaベースのフレームワークが備わっており、このフレームワークを使用することで、Amazon Fire TVを対象としたストリーミングメディアAndroidアプリを短時間で簡単に開発することができます。Fire App Builderを使用すると、コードをゼロから開発しなくても、ベストプラクティスと各種手法に従いながら、魅力的で高品質のメディア体験をFire TVに構築することができます。Fire App BuilderのコードはJavaベースであり、Android StudioやGradleなど、Androidアプリの開発用として広く普及しているツールで使用できます。Fire App Builderは、Apache 2.0 ライセンスのオープンソースプロジェクトとして、Github ([github.com/amzn/fire-app-builder](github.com/amzn/fire-app-builder)) で公開されています。

### Fire TV Stick (第 2 世代)

2016 年 9 月 28 日、[Fire TV Stick with Alexa Voice Remote](https://www.amazon.com/dp/B00ZV9RDKK) (本ドキュメントでは「Fire TV Stick (第 2 世代)」と呼んでいます) がリリースされました。この次世代Fire TV Stickは、ベストセラーを博した初代バージョンと比較して速度が 30% 向上しています。300,000 本を超える映画やTVエピソードが提供され、Alexa Voice Remoteが付属して、価格はわずか 39.99 ドルとなっています。クアッドコアプロセッサが搭載されているため、ハイパフォーマンスのアプリやゲームの真価が存分に発揮されます。詳細については、「[Introducing the All-New Fire TV Stick with Alexa Voice Remote](https://developer.amazon.com/public/community/post/Tx1W8FCB4UA5KIB/Introducing-the-All-New-Fire-TV-Stick-with-Alexa-Voice-Remote)」を参照してください。Fire TV Stick (第 2 世代) の仕様は、「[Fire TV端末の仕様][device-and-platform-specifications]」でご確認いただけます。

## Fire TVドキュメントの更新

Fire TV関連のドキュメントが以下のように更新されています。

{: .grid}
| 日付 | ページ | 詳細 |
|-----|-----|------|
| 2016 年 12 月 14 日 | [Amazon Fire TV開発フレームワークの比較][fire-tv-development-framework-comparison] | Fire TV向けウェブアプリスターターキット (WASK) とFire App Builderとの違いについて詳しい説明が追加されました。|
| 2016 年 11 月 29 日 | [Notifications for Amazon Fire TV][notifications-for-amazon-fire-tv] | Fire TVのUIアップデート時に追加される予定の通知センター機能に関する説明が追加されました。|
| 2016 年 11 月 23 日 | [アプリのインストール場所の指定][specifying-installation-location] | アプリのインストールで外部ストレージを優先する点が強調されました。各種プラットフォームにおけるアプリのインストールについての記述も追加されています。|
| 2016 年 10 月 25 日 | [Image Guidelines for App Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets) | [Fire TV Assets] セクションのグラフィックと手順を更新しました。汎用的な図を廃止し、コンテンツセーフ領域に表示される実際のサンプルグラフィックを追加し、色分けによる説明を追加しました。|
| 2016 年 10 月 20 日 | [Amazon Fire TV用アプリとゲームを開発するための準備][getting-started-developing-apps-and-games-for-amazon-fire-tv]  | セクションを展開したり折りたたんだりするための新しいサイドバーナビゲーションが右側に追加されました。 |
| 2016 年 10 月 2 日 | [Fire App Builder: A Starter Kit for Java-based Amazon Fire TV and Android Apps][fire-app-builder-overview] | Fire TV向けにJavaベースのAndroid TVアプリをFire App Builderで作成する方法について詳しく (30 ページ超) 解説したドキュメントです。|
| 2016 年 9 月 29 日 | [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices] | Fire TV Stick (第 2 世代) の機能IDが追加されました。 |
| 2016 年 9 月 28 日 | [Image Guidelines for App Submission](/solutions/devices/fire-tv/docs/asset-guidelines-for-app-submission#firetvassets) | アプリの申請時に使用する [Images & Multimedia] タブの [Fire TV Assets] セクションに関して、画像アセットのガイドラインが追加されました。|
| 2016 年 9 月 28 日 | [Fire TV端末の仕様][device-and-platform-specifications] | Fire TV Stick (第 2 世代) の仕様が追加されました。また、同じページに、対応する「メディアの仕様」の内容も追加されています (情報を一元化、統合するため)。ナビゲーションタブで切り替えて情報を表示できます。|
| 2016 年 9 月 28 日 | [Image Guidelines for App Submission][asset-guidelines-for-app-submission] | 近く予定されているFire TVのUIアップデートに合わせて、Fire TV画像アセットについて説明する新しいセクションが追加されました。|
| 2016 年 9 月 16 日 | [Fire TV開発とAndroid TV開発の違い][amazon-fire-tv-differences-from-android-tv-development] | Fire TV向けアプリの開発時に考慮すべき点についてのページが追加されました。|
| 2016 年 9 月 14 日 | [Amazon Fire TV用アプリとゲームを開発するための準備][getting-started-developing-apps-and-games-for-amazon-fire-tv] | 新しいサイトワークフローに基づいて改訂されました。 |
| 2016 年 9 月 10 日 | [Using System X-Ray on Fire TV][amazon-fire-tv-system-xray] | System Xrayオーバーレイを使用したアプリの問題のトラブルシューティングについて詳しく説明するページが追加されました。|
| 2016 年 9 月 5 日 | [Getting the Advertising ID on Fire TV][fire-os-advertising-id] | 広告IDの取得について詳しく説明するページが追加されました。|
| 2016 年 9 月 1 日 | [Playing 4K Ultra HD Videos on Fire TV][fire-tv-4k-ultra-hd] | 4Kアプリ開発のガイドラインとFire TV用の拡張サポートライブラリに関するページが追加されました。|
| 2016 年 8 月 30 日 | [Amazon Fire TVホームページ](/solutions/devices/fire-tv) | 新しいバージョンの開発者ポータルサイトにホームページを移行しました (順次展開予定)。|
| 2016 年 8 月 5 日 | [Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices]|  Fire TV端末を識別するための新しい方法 (モデル + 製造業者ではなく機能の有無を調べる) について説明しています。|
| 2016 年 7 月 23 日 | [Fire TV端末の仕様][device-and-platform-specifications] | H.265 の詳細やWidevineサポートなどの情報を更新しました。|
| 2016 年 7 月 22 日 | [Dolby Integration Guidelines][amazon-fire-tv-dolby-integration-guidelines] | Dolby Audioをサポートするための技術的な情報について詳しく説明するページが追加されました。|
| 2016 年 7 月 1 日 | [Media Players for Amazon Fire TV][fire-tv-media-players] | サポートされるメディアプレーヤーについての情報を更新するとともに、ExoplayerのAmazon向けポートについて詳しい説明が追加されました。|
| 2016 年 6 月 10 日 | ["Handling Audio Focus with Voice Interactions"][managing-audio-focus#audiofocusvoice] | 音声操作が開始されたときのオーディオフォーカスの処理についてのセクションが追加されました。 |
| 2016 年 6 月 16 日 | ["Voice Search"][implementing-search-fire-tv#voicesearch] | 音声検索とテキスト検索の動作、およびこの 2 つの検索から返される内容についてのセクションが追加されました。|
| 2016 年 6 月 1 日 | Persistent sidebar navigation | [Fire TV](/solutions/devices/fire-tv)のドキュメントに、関連するドキュメントが閲覧しやすいよう右サイドバーが表示されます。|


## Fire TVブログの更新

ブログのFire TVカテゴリで、Fire TVに関する最新情報をチェックできます。

<a href="https://developer.amazon.com/blogs/tag/Fire+TV"><button class="feedbackButton">Fire TVのブログを読む</button></a>


{% include links.html %}
