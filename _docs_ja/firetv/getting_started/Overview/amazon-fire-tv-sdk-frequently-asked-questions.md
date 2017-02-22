---
title: Amazon Fire TVのよくある質問 (FAQ)
permalink: amazon-fire-tv-sdk-frequently-asked-questions.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

## 全般 {#general}

Q: Amazon Fire TVプラットフォーム向けに開発したアプリを申請するにはどうすればよいですか?
:   Amazon Fire TV端末向けのアプリを申請するには、[Amazonアプリおよびサービス開発者ポータル](https://developer.amazon.com/appsandservices)でアカウントを作成し、ポータルからアプリを申請します。

    すでにAmazonでアプリを公開済みである場合は、Amazon Fire TV用に別のバイナリAPKを追加してアプリを更新してください。 詳細については、「[Device Targetingの使用](https://developer.amazon.com/public/ja/solutions/devices/fire-tv/docs/using-device-targetng)」を参照してください。

Q: 私が開発したアプリは、Amazon Fire TVプラットフォームで動作するでしょうか?
:   開発したアプリが、Amazon Fire TV端末の仕様に適合している必要があります。端末と機能の仕様について詳しくは、[端末の仕様](/solutions/devices/fire-tv/docs/device-and-platform-specifications)を参照してください。

    [Amazonアプリストアコンテンツポリシー要件](https://developer.amazon.com/public/support/submitting-your-app/tech-docs/appstore-content-policy)にアプリが準拠していることが必要です。またアプリをご自身でテストし、問題が見つかった場合はアップデートを申請することをお勧めします。

Q:  自分のアプリがAmazon Fire TV端末で実行されるかどうかを確認するにはどうすればよいでしょうか?
:   A: `amazon.hardware.fire_tv`という機能が存在するかどうかを確認してください。厳密に端末の機種 (Fire TV第 1 世代、Fire TV第 2 世代など) を調べる必要がある場合は、MODELの戻り値 (それぞれ`AFTB`、`AFTS`) を調べることで確認できます。ただし、機能のフォールバックは必ず含めるようにしてください。詳細については、「[Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices]」を参照してください。

Q: テストのためにアプリをAmazon Fire TVにサイドロードすることはできますか?
:   はい。できます。Android Debug Bridge (ADB) をご利用ください。  詳細については、「[Getting Started Developing Apps and Games for Fire TV](/solutions/devices/fire-tv/docs/getting-started-developing-apps-and-games-for-amazon-fire-tv)」を参照してください。 

Q: 開発者向けにAmazon Fire TVのテスト端末は提供されていますか?
:   開発者向けのテスト端末は、Amazonでは提供していません。

    Amazon Fire TV端末は、[米国](http://www.amazon.com/Fire-TV-streaming-media-player/dp/B00CX5P8FC)、[英国](http://www.amazon.co.uk/dp/B00KQEJBSW)、[ドイツ](http://www.amazon.de/dp/B00KQEIMY6)のAmazonリテールサイトで販売されています。国ごとの販売状況については、商品詳細ページを参照してください。

Q: Amazon Fire TVプラットフォームでは、具体的にどのような機能がサポートされていますか?
:   端末と機能の仕様について詳しくは、[端末の仕様](/solutions/devices/fire-tv/docs/device-and-platform-specifications)および「[Fireタブレット](https://developer.amazon.com/public/solutions/devices/fire-tablets "https://developer.amazon.com/sdk/fire.html")」を参照してください。

Q: 自分が作ったアプリをAmazon Fire TVプラットフォームで宣伝してもらうにはどうすればよいでしょうか? 
:   「[アプリのマーケティング](https://developer.amazon.com/public/jp/resources/marketing-tools/marketing-your-app "アプリのマーケティング")」を参照してください。

Q: Amazon Fire TVプラットフォームに関する情報はどのようにして入手できますか?
:   [お問い合わせ](https://developer.amazon.com/public/support/contact/contact-us "お問い合わせ")フォームでご質問をお寄せください。

Q:  アプリはどのような状況で一時停止しますか? また、一時停止の動作をどのように実装すればよいですか?
:   A: アプリが一時停止するのは、Fire TVのリモコンまたはAmazon Fireゲームコントローラの [Microphone] (音声検索)、[Home]、[GameCircle] のいずれかのボタンが押されたときです。[Back] ボタンが押されたときも、現在のアクティビティが一時停止します。この場合、スタック上の前のアクティビティが再開されます。再開されるアクティビティは、一時停止されたアプリのものではないことがあります。

    一時停止の動作を処理するには、他のAndroidアプリと同様に、アクティビティに`onPause()` メソッドを実装します。`onPause()` メソッドでは、ユーザーの状態または場所を保存して、アプリが再開したときにユーザーを一時停止前と同じ位置に置く必要があります。

    また、メディアアプリでは、次の要件を満たす必要があります。

    * ビデオを再生するアプリは再生を一時停止し、<b>一時停止状態になったらすぐにデコーダーなどのすべてのメディアリソースを解放する必要があります</b>。これは、これらの限りあるリソースがハードウェアベースであり、メモリを大量に消費するためです。詳細については、「[Releasing the Media Player](https://developer.android.com/guide/topics/media/mediaplayer.html#releaseplayer)」および「[`MediaPlayer.release()`](https://developer.android.com/reference/android/media/MediaPlayer.html#release()">)」を参照してください。
    * オーディオを再生するアプリは、一時停止しても再生を継続することは許可されますが、他のアプリによって要求された場合は、オーディオフォーカスを放棄する必要があります。詳細については、「[Managing Audio Focus](http://developer.android.com/training/managing-audio/audio-focus.html)」を参照してください。

Q:  デフォルトのウェブブラウザで広告を開くことはできますか?
:   A: Amazon Fire TVにはブラウザアプリは含まれていないため、URL (広告など) は一切機能しません。アプリにウェブページへのリンクを含める必要がある場合は、Androidの[WebView](http://developer.android.com/reference/android/webkit/WebView.html)を使用できます。ただし、AndroidのWebViewを使用する場合は、コントローラからの入力イベントを処理するために、アプリで独自のCookieや認証を管理する必要があります(AndroidのWebViewはD-Padナビゲーションをサポートしますが、レイアウトとナビゲーションの見直しが必要になることがあります)。

    AndroidのWebViewを実装する際のヒントについては、ブログ投稿「[Using WebViews on Tablets with HD Screens](https://developer.amazon.com/post/Tx3BY931C53CRLA/Using-WebViews-on-Tablets-with-HD-Screens)」を参照してください。この投稿はタブレットに関するものですが、アドバイスはテレビにも適用できます。

Q:  Amazonアプリ内課金APIをFire TVアプリで使用できますか?:   A: はい。Amazonの[アプリ内課金 (IAP)](https://developer.amazon.com/public/apis/earn/in-app-purchasing) APIは、すべてのFire TV端末およびFire TVのリモコンとゲームコントローラで機能します。トランザクションをプレビューするには、[App Tester](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/testing-iap-2.0)を使用します。

Q:  Amazon Device Messaging (ADM) をFire TVアプリで使用できますか?
:   A: [Amazon Device Messaging](https://developer.amazon.com/device-messaging)を使用して、プッシュメッセージや他のデータを受け取ることができます。

Q:  Amazon Maps APIをFire TVアプリで使用できますか?
:   A: Amazon Fire TVには位置情報サービスはないので、現時点で[Amazon Maps API](https://developer.amazon.com/maps)はサポートされていません。

Q:  Amazon Mobile Ads APIをFire TVアプリで使用できますか? 
:   A: 現時点では、[Amazon Mobile Ads API](https://developer.amazon.com/public/apis/earn/mobile-ads)はFire TVでサポートされていません。

Q:  Amazonアプリストアに存在するアプリの詳細ページにリンクできますか?
:   A: はい。Amazonアプリストアのディープリンクは、他のアプリと同様にFire TVアプリでも機能します。ディープリンクの詳細については、「[Amazonアプリストアアプリへのリンク](https://developer.amazon.com/public/ja/apis/earn/in-app-purchasing/docs/deeplink)」を参照してください。ただし、アプリを評価する機能は、現在、Amazon Fire TVでサポートされません。アプリでは、ユーザーに評価を依頼しないでください。

Q:  オンスクリーンキーボードをテンキーパッドに変更するにはどうすればよいですか?
:   A: 標準のAndroidメカニズムを使用することにより、すべての[EditText](http://developer.android.com/reference/android/widget/EditText.html)ウィジェットに対してIME (入力方式エディター) を構成できます。In XML, use `android:inputType="number"`:

    ```
    <EditText
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/edittext"
    android:inputType="number"/>
    ```

Q:  Amazon Fire TVでスクリーンショットを取得するにはどうすればよいですか?
:   A: スクリーンショットを取得するには、adbを使用して端末に接続し、次のコマンドを使用します。`filename.png`をスクリーンショットの名前に変更し、`/tmp`をスクリーンショットを格納する開発コンピューターのディレクトリに置き換えます。

    ```
    adb shell screencap -p /sdcard/filename.png
    adb pull /sdcard/filename.png /tmp
    adb shell rm /sdcard/filename.png
    ```
    詳細については、「[Taking Screenshots of your Fire TV App](/support/submitting-your-app/tech-docs/04-taking-screenshots#firetv)」を参照してください。

Q: Googleのアプリ内課金テクノロジーを使用しているのですが、Amazon Fire TVプラットフォームで動作するでしょうか?
:   いいえ。Googleのアプリ内課金APIなどのサービスは、Google Mobile Servicesにアクセスする必要があり、Amazon Fire TVプラットフォームでは動作しません。アプリ内課金に関して、Amazonでは独自のアプリ内課金APIを提供しています。開発者の皆さまが開発するアプリでは、このAPIを通じてデジタルコンテンツやサブスクリプションを簡単に提供することができます。

Q: Amazon Fire TVまたはFire TV Stick用のアプリをドイツやオーストリアで配布する場合、他に基準はありますか?
:   はい。Amazonではでは、Amazon Fire TVやFire TV Stickのアプリのうち、Amazon Fire TVやFire TV Stick端末、SDメモリカード、接続されている外部ストレージ (該当する場合) に対して何らかのビデオコンテンツやオーディオコンテンツを格納 (複製、録音、ダウンロード、保存など) する機能が備わっているアプリをドイツやオーストリアで配布することを禁止しています。そのような機能が含まれているとAmazonが判断したアプリは、ドイツやオーストリアでは公開されません。    

## Fire OS 5 (Android Lollipop) {#fireos}

Q: Fire OS 5 では、Android TVとv17 Leanback Libraryがサポートされますか?
:   A: Fire OS 5 では、Android TV機能とLeanback Libraryの両方がサポートされています。ただし、Leanbackライブラリの[`SearchFragment`]((http://developer.android.com/reference/android/support/v17/leanback/app/SearchFragment.html))クラスによる音声認識は現在サポートされていません。詳細については、「[Implementing Search in Fire TV][implementing-search-fire-tv]」を参照してください。

## メディアとDRM {#mediaanddrm}

Q: Fire TVでは、どのようなサードパーティ製のメディアプレーヤーSDKがサポートされますか?
:   A: Androidメディア再生APIおよび暗号化APIを実装しているメディアプレーヤーSDKであれば、Amazon Fire TVプラットフォームで機能します。  そのようなSDKの例として、[Amazon Port of the ExoPlayer](https://github.com/amzn/exoplayer-amazon-port)、[Android MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html)、[VisualOn OnStream MediaPlayer+ SDK](http://visualon.com/onstream-mediaplayer/)、[NexStreaming NexPlayer SDK](http://www.nexstreaming.com/index.php)があります。詳細については、「[Media Players][fire-tv-media-players]」を参照してください。

{% comment %}
Q:  無料のVisualOnライセンスをAmazonから入手できますか? 
:   A: 現在Amazonからは、[VisualOn OnStream Mediaplayer](http://visualon.com/onstream-mediaplayer)用の無料ライセンスは提供されていません。過去にAmazonから提供されたライセンスを使用してVisualOnプレーヤーをFire TVアプリに統合した場合、2017 年 1 月 1 日以降は、既存のライセンスの有効期限が切れた時点でライセンスをご購入いただく必要があります。VisualOnライセンスの購入を希望されない場合は、ExoplayerのAmazon向けポートの使用をお勧めします。詳細については、「[Media Players][fire-tv-media-players]」を参照してください。
{% endcomment %}

Q:  Microsoft PlayReady DRMで暗号化されたメディアを再生する機能を実装する場合、どのようなオプションがありますか?
:   A: Amazon Fire TVプラットフォームには、保護されたメディアを再生するためのAndroid `MediaCrypto`クラスがサポートされています。または、PlayReadyをサポートするサードパーティ製のメディアプレーヤーSDKも使用できます。Amazon Fire TVでサポートされるのはPlayReady Version 2.5 だけです。  Fire TV端末では、PlayReady Security Level 3000 (SL3000) がサポートされません。PlayReady 3.0 でSL3000 が導入されたものの、Amazon Fire TV端末の対応がバージョン 2.5 に限られるためです。


Q:  Amazon Fire TVでは、暗号化されたオーディオを含むPlayReadyコンテンツはサポートされますか?
:   A: Fire TVでサポートされるのは、暗号化されたビデオとクリア (暗号化されていない) オーディオを含むPlayReadyコンテンツのみです。Widevine DRMもサポートされます。詳細については、[Fire TV端末の仕様に関するページのDRMのセクション][device-and-platform-specifications]を参照してください。暗号化されたオーディオとビデオの両方を含むコンテンツを再生する方法の詳細については、[こちら](https://developer.amazon.com/public/support/contact/contact-us)からお問い合わせください。


Q:  Dolby DigitalオーディオをFire TV端末で復号化する方法は?
:   A: Fire TVとFire TV Stickのどちらにも、Dolby Digital (AC3) およびDolby Digital Plus (eAC3) の復号化機能が含まれています。これらの形式は、標準のAndroidメディアプレーヤーライブラリを使用して再生できます。Amazon Fireタブレット用のDolbyAudioProcessing SDKは、Fire TV端末でサポートされない (または必要ない) ことに注意してください。Dolby再生の詳細については、「[Dolby Integration Guidelines][amazon-fire-tv-dolby-integration-guidelines]」を参照してください。


Q:  端末では、ファームウェアとOSを検証するためのセキュアブートがサポートされますか?
:   A: はい。


Q:  OSはアプリの署名確認を強制しますか?
:   A: はい。


Q:  出力はHDMI上のセキュアチャネルを通してHDCP保護されますか?
:   A: はい。


Q: Amazon Fire TVでは、カスタマイズ可能なクローズドキャプション (CEA-708) はサポートされますか?
:   A: Fire TVはプラットフォームレベルでクローズドキャプションをサポートしていますが、現時点では機能が限られています。メディアアプリでは、視聴者がキャプションをカスタマイズできるようにするUIを、サードパーティ製のメディア再生SDKまたは独自の実装を通して実装する必要があります。


Q: Amazon Fire TVでは、クローズドキャプションにどのようなフォントを使用できますか?
:   A: Amazon Fire TVで利用できるプラットフォームフォントは限られています。CEA-708 で定義されているスタイルカテゴリのそれぞれについて、独自のフォントファイルのライセンスを取得し、そのフォントファイルを埋め込む必要があります。

## メディアの再生 {#mediaplayback}

Q:  メディアの再生に関してリソースとメモリを節約するうえでのベストプラクティスを教えてください。

:   A: Amazon Fire TVでメディアの再生に使用されるリソースはハードウェアベースで、メモリに限りがあるため、アプリ側の適切な振る舞いが要求されます。使い終えたメディアリソースはアプリ側で確実に解放してください。

    具体的には、ビデオを再生するアプリは、再生を一時停止し、デコーダーなどのすべてのメディアリソースを`onPause()` メソッド内ですぐに解放する必要があります。詳細については、「[Releasing the Media Player][release-media-player]」および「<a href="https://developer.android.com/reference/android/media/MediaPlayer.html#release()"><code>MediaPlayer.release()</code></a>」を参照してください。

    HTMLの `<video>` タグを使用して、Androidの[WebView](http://developer.android.com/reference/android/webkit/WebView.html)経由でビデオを再生する場合、アプリが一時停止または停止しても、現時点ではハードウェアメディアリソースが自動的には解放されません。この問題を避けるために、`onPause()` メソッドと`onStop()` メソッドの実装コードの中で、次のようにプロセスを強制的に終了して、これらのリソースを明示的に解放してください。

    ```java
    public void onStop() {
       super.onStop();
       android.os.Process.killProcess(android.os.Process.myPid());
    }
    ```

Q: バックグラウンドで音楽を再生するアプリを作成しました。このアプリが一時停止すると、オーディオ再生が停止するのはなぜでしょうか?また、このアプリを起動しても、他のアプリのオーディオが再生され続けるのはなぜですか?
:   A: そのアプリは、オーディオフォーカスを正しく管理していません。アプリは再生を開始したときに、オーディオフォーカスを要求し、また、メディアボタンイベントのレシーバーとして自身を登録する必要があります。オーディオフォーカスを放棄した (再生が終了したか、または他のアプリがオーディオフォーカスを要求したために) ときには、メディアボタンイベントのレシーバーから自身を登録解除する必要があります。具体的には、次のようにします。

    * アプリが再生を開始するときには、[`AudioManager.requestAudioFocus()`](http://developer.android.com/reference/android/media/AudioManager.html#requestAudioFocus(android.media.AudioManager.OnAudioFocusChangeListener,%20int,%20int)) を使用してオーディオフォーカスを要求します。
    * オーディオフォーカスを取得したら、[`AudioManager.registerMediaButtonEventReceiver()`](http://developer.android.com/reference/android/media/AudioManager.html#registerMediaButtonEventReceiver(android.content.ComponentName)) を使用してメディアボタンのレシーバーとして登録します。
    * [`AudioManager.onAudioFocusChangeListener()`](http://developer.android.com/reference/android/media/AudioManager.OnAudioFocusChangeListener.html) を使用して、オーディオフォーカスが失われるのをリッスンします。
    * オーディオフォーカスが失われたら、再生を停止し、[`AudioManager.unregisterMediaButtonEventReceiver()`](http://developer.android.com/reference/android/media/AudioManager.html#unregisterMediaButtonEventReceiver(android.content.ComponentName)) を使用してメディアボタンのレシーバーとしての登録を解除します。

    詳細については、以下のドキュメントを参照してください。

    *  [Managing Audio Focus][managing-audio-focus] (Fire TV関連ドキュメント)
    *  [Managing Audio Focus](http://developer.android.com/training/managing-audio/audio-focus.html) (Android関連ドキュメント)
    *  [Allowing applications to play nice(r) with each other: Handling remote control buttons](http://android-developers.blogspot.com/2010/06/allowing-applications-to-play-nicer.html) 

Q:  音楽アプリをバックグラウンドで実行して音楽を再生しているときに、不規則に停止するのはなぜでしょうか?
:   A: 音楽再生アプリをフォアグラウンドサービスとして実行してください。バックグラウンドサービス (デフォルト) は、リソースが少なくなると、システムによって自動的にシャットダウンされます。詳細については、Androidガイド (「[Media Playback](http://developer.android.com/guide/topics/media/mediaplayer.html)」と[Service](http://developer.android.com/reference/android/app/Service.html)クラスの[`startForeground()`](http://developer.android.com/reference/android/app/Service.html#startForeground(int,%20android.app.Notification)) メソッド) を参照してください。

Q:  TVの電源が消されたり、HDMIケーブルが抜かれたりした場合には、どのように対処すべきでしょうか?
:   A: HDMIとの接続が解除されたときに求められる動作は、オーディオとビデオとで異なります。詳細については、「[Handling HDMI Events][fire-tv-handling-hdmi-events]」を参照してください。

Q:  メディアを再生している際に、端末がスタンバイまたはスリープモード (スクリーンセーバー) に遷移するのを防止するにはどうすればよいですか?
:   A: メディアの再生中にAmazon Fire TVとテレビがスタンバイまたはスリープモード (スクリーンセーバー) に遷移しないようにする (オンの状態を維持する) には、アクティビティに [`KEEP_SCREEN_ON`](http://developer.android.com/reference/android/view/View.html#KEEP_SCREEN_ON)フラグを設定するか、PowerManagerからWakeLockを取得します。オーディオまたはビデオの再生を行っていないときには、すべてのWakeLockをアプリで解放して、端末とテレビがスリープモードに遷移し、電力消費が少なくなるようにする必要があります。WakeLockを使用する方法の詳細については、[`PowerManager`](http://developer.android.com/reference/android/os/PowerManager.html)および[`PowerManager.WakeLock`](http://developer.android.com/reference/android/os/PowerManager.WakeLock.html)クラスを参照してください。

    アプリがオーディオを再生する際にPartialWakeLock ([`PARTIAL_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#PARTIAL_WAKE_LOCK)) を設定すると、テレビはオンの状態を維持し、端末はアイドルになるとスリープモードに遷移します (スクリーンセーバーが表示されます)。これは、モバイル端末でのPartialWakeLockの動作と異なります。モバイル端末では、CPUはオンに維持され、画面は消灯します。Amazon Fire TVの動作は、HDMI経由でオーディオを再生する場合に、テレビがオンである必要があるためです。繰り返しになりますが、アプリがオーディオの再生を行っていないときには、WakeLockを解放して、テレビがスリープモードに遷移できるようにします。

    [`SCREEN_BRIGHT_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#SCREEN_BRIGHT_WAKE_LOCK)フラグまたは[`SCREEN_DIM_WAKE_LOCK`](http://developer.android.com/reference/android/os/PowerManager.html#SCREEN_DIM_WAKE_LOCK)フラグを設定しても、端末の動作は影響を受けません。

Q:  端末がスリープモード (スクリーンセーバーがオン) に遷移したことを検知するにはどうすればよいですか?
:   A: スリープモードはAndroidの機能です。テレビがオンのときにAmazon Fire TVがスリープモードに遷移すると、スクリーンセーバーが表示されます。端末がスリープモードに遷移したか、スリープモードから他のモードに遷移したことを検知するには、`Intent.ACTION_DREAMING_STARTED`および`Intent.ACTION_DREAMING_STOPPED`のレシーバーとしてアプリを登録します。

    音楽アプリではないアプリでオーディオを再生する場合、端末がスリープモードに遷移したときには、オーディオの再生を一時停止する必要があります。

ビデオを再生するのに、Androidの[WebView](http://developer.android.com/reference/android/webkit/WebView.html)およびHTMLの`<video>`タグを使用できますか?
:   A: 答えは "はい" ですが、制限があります。現時点では、アプリが一時停止または停止したときに、ハードウェアメディアリソースはシステムによって解放されません。この問題を回避するには、次の`onStop()`メソッドを実装して、アプリのプロセスを明示的に停止します。

    ```java
    public void onStop() {
    super.onStop();
    android.os.Process.killProcess(android.os.Process.myPid());
    }
    ```

    この問題によって、アプリのユーザーエクスペリエンスとナビゲーションが不安定になることがあります。端末でメディアを再生する方法としては、サードパーティ製のメディアプレーヤーSDKの使用が推奨されます。

Q:  アプリで 4K Ultra HDビデオを再生できますか? 

:   はい。Fire TV (第 2 世代) が 4K対応TVに接続されていれば、4K Ultra HDビデオを再生できます。Fire TV (第 1 世代) とFire TV Stickでは、4Kビデオが再生されません。

    4Kのコンテンツを再生する前に、(1) 端末がFire TV (第 2 世代) であるかどうかと、(2) 接続されているテレビが 4Kに対応しているかどうかをアプリ側でチェックする必要があります。Amazon 4K Extension Libraryは、4Kモードスイッチを開始できるように開発されています。

    Ultra HDビデオに対応したアプリは、必要なユーザーエクスペリエンスを確実に満たすために、Amazonによって認定されます。通常、認定は数週間以内に完了します。詳細については、「[Playing 4K Ultra HD Videos][fire-tv-4k-ultra-hd]」を参照してください。


## コントローラ {#controllers}

Q:  Amazon Fire TVゲームコントローラに、以前のバージョンにはあったメディアボタンがありません。メディアを再生するにはどうすればよいですか?
:   A: Amazon Fire TVでは、アナログスティック ([Play/Pause]) を押したとき、および左側のショルダー ([Rewind]) ボタン (L1) や右側のショルダー ([Fast Forward]) ボタン (R1) を押したときに、メディア入力イベントが生成されます。これらのボタンを使用しないアプリまたはゲームでは、ユーザーがメディアの再生をバックグラウンドでコントロールできるように、これらのボタンイベントをアプリでキャプチャや破棄などの処理の対象にしないでください。

    Amazon Fireゲームコントローラのメディアボタンをアプリで他の目的に転用している場合、新しいAmazon Fire TVゲームコントローラのユーザーは、ボタンがなければその機能を使用できません。両方のゲームコントローラに共通するボタンを使用するようにアプリを更新することや、画面上に表示されるヒントを更新することを検討してください。

Q: Amazon Fire TVゲームコントローラから音量制御を行うには、どうすればよいですか?
:   A: Amazon Fire TVでは、オーディオをAmazon Fire TVゲームコントローラのヘッドホンジャックにストリーミングできます (現行世代の端末のみ)。音量制御には、左側/右側のトリガーボタン (L2/R2) ボタンを使用します。

    音量制御はシステム機能であり、アプリの他のボタンにマップできません。トリガーボタンを使用しないアプリまたはゲームでは、ユーザーがメディアの再生をバックグラウンドでコントロールできるように、これらのボタンイベントをアプリでキャプチャや破棄などの処理の対象にしないでください。これらのボタンをアプリまたはゲームで他の目的に転用している場合、GameCircle画面またはシステムランチャーから音量を制御できることを、画面上に表示されるヒントで知らせることを検討してください。

Q:  アプリでFire TV Voice Remoteの [Microphone] ボタンをオーバーライドできますか?
:   A: [Microphone] ボタンは、**一時的オーディオフォーカス**を要求してシステム全体の音声機能を起動するもので、オーバーライドすることはできません。開発するアプリケーションは、このオーディオフォーカス変更イベント (`AUDIOFOCUS_LOSS_TRANSIENT`) に加え、他のオーディオフォーカス変更イベント (`AUDIOFOCUS_LOSS`、`AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK`など) も処理する必要があります。各種オーディオフォーカス要求は、音声に限らず、あらゆるアプリケーションによって行われることが考えられるためです。つまり、すべてのオーディオフォーカスのユースケースが適切に処理される必要があります。詳細については、「[Handling Audio Focus with Voice Search][managing-audio-focus#audiofocusvoice]」を参照してください。

Q:  [Menu] ボタンにはどのような機能がありますか? また、アプリ内でオーバーライドできますか?
:   A: デフォルトでは、[Menu] ボタンはAndroidコンテキストメニューを起動し、画面の中央にメニューアイテムのリストを表示します。[Menu] ボタンをオーバーライドして、独自のカスタムメニューユーザーインターフェースを提供するなどの目的に使用できます。

    メニューアイテムが 1 つだけ必要な場合は、単純な切り替えスイッチ (たとえば、クローズドキャプションのオン/オフ) として [Menu] ボタンを使用し、この機能をユーザーに知らせるためにヒントを画面に表示することもできます。

Q:  Fireゲームコントローラの [GameCircle] ボタンを押しても、ゲームのGameCircleオーバーレイが表示されないのはなぜですか?
:   A: GameCircleオーバーレイは、次の条件が満たされる場合にだけ表示されます。

    * ゲームにGameCircle APIが実装されている。
    * ゲームがAmazonアプリストアに申請されており、ゲームとして分類されている。
    * ゲームがAmazonアプリストアからFire TV端末にインストールされている。


    テストのためにゲームをFire TV端末にサイドロードしている場合、GameCircleオーバーレイは表示されません。アプリのGameCircle実装は、[ライブアプリテスト](https://developer.amazon.com/public/resources/development-tools/live-app-testing)を使用することにより、申請前にテストできます。

    GameCircleオーバーレイは、いずれのリーダーボードまたはアチーブメントも公開しておらず、しかもテストアカウントを使用していない場合にも表示されないことがあります。このシナリオでは、1 つ以上のアチーブメントまたはリーダーボードをAmazon Developer Consoleで公開するか、またはテストアカウントを (Amazon Developer Consoleで) セットアップし、そのアカウントを使用して暫定のアチーブメントおよびリーダーボードをGameCircleオーバーレイで表示します。

Q:  コントローラを接続、切断、またはスリープ状態にすると、アクティビティが初めから再起動するのはなぜですか?
:   A: これらのイベントは、Androidによって実行時設定変更として処理されます。これらのイベントをアプリで無視するには、`AndroidManifest.xml`を変更して`android:configChanges`属性を追加し、この属性に`keyboard`、`keyboardHidden`、`navigation`の各キーを含めます。

    ```java
    <activity android:name="MyActivity"
    android:configChanges="keyboard|keyboardHidden|navigation">
    ```

    `configChanges`属性について、および必要に応じて設定変更を処理する方法の詳細については、Androidガイド「[実行時の変更の処理](http://developer.android.com/guide/topics/resources/runtime-changes.html)」を参照してください。

Q:  ゲームコントローラの切断をアプリまたはゲームでどのように処理すればよいですか?
:   A: Amazon Fire TVゲームコントローラは、5 分を超えてアイドルであるかスティックが同じ角度に保持されていると、バッテリー残量を維持するためにシステムから切断される可能性があります。他のコントローラも、アイドルであるか、バッテリー残量が無くなると切断される可能性があります。コントローラ切断イベントを処理するには、Android [`OnInputDeviceRemoved`](http://developer.android.com/reference/android/hardware/input/InputManager.InputDeviceListener.html#onInputDeviceRemoved(int)) リスナーを使用します。コントローラが利用できないことをユーザーに知らせるために、ゲームを一時停止するか、またはダイアログを表示することを検討してください。

## Amazon Fire TV Stick {#amazonfiretvstick}

Q:  Amazon Fire TVとFire TV Stickの違いは何ですか?
:   A: [Fire TV端末の仕様][device-and-platform-specifications]に、すべてのFire TV端末の仕様が記載されています。


Q:  Amazon Fire TVアプリをAmazon Fire TV Stickに適応させるにはどうすればよいですか?
:   A: Amazon Fire TVとFire TV Stickは両方とも、同じプラットフォームソフトウェアを実行します。ただし、Fire TV Stickではハードウェアの条件が厳しいので、パフォーマンスと安定性に関してアプリを最適化する必要があります。ハードウェアアクセラレーションとパフォーマンスに関するAndroidの[ベストプラクティス](http://developer.android.com/training/best-performance.html)に従うことが重要です。特に、OpenGLとテクスチャについては注意が必要です。Fire TV StickのGPUは`MAX_TEXTURE_SIZE`が 2048 x 2048 のOpenGL 2.0 をサポートします。

Q:  Fire TV Stick端末の識別方法は?
:   A: `amazon.hardware.fire_tv`という機能が存在するかどうかを確認してください。Fire TV端末の機種 (Stick) まで厳密に調べる必要がある場合は、MODELの戻り値 (`AFTM`) をチェックします。詳細については、「[Identifying Amazon Fire TV Devices][identifying-amazon-fire-tv-devices]」を参照してください。

Q:  アプリの一部の画像/背景色が表示されないか、画像の代わりに灰色のボックスが表示されます。
:   A: この現象は、通常、ビットマップ画像またはテクスチャが非常に大きい場合に発生します。Fire TV Stickは、最大 2048 x 2048 のテクスチャをサポートします。アプリにこの問題がある場合は、次のようなエラーがログに表示されます。

    ```
    W/OpenGLRenderer( 8941): Bitmap too large to be uploaded in a texture (3840x2160, max=2048x2048)
    ```

    また、Fire TV用の画像は、`drawables/` フォルダーではなく、`drawables-xhdpi/` フォルダーに存在する必要があります。デフォルトのドローアブルをプラットフォームに応じて拡大すると、テクスチャ制限を超える大きな画像が作成されることがあります。


[release-media-player]: https://developer.android.com/guide/topics/media/mediaplayer.html#releaseplayer
[mediaplayer-android]: https://developer.android.com/reference/android/media/MediaPlayer.html#release()

{% include links.html %}
