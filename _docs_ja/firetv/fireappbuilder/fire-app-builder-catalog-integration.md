---
title: Fire TVカタログにメディアを組み込む
permalink: fire-app-builder-catalog-integration.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire TVのどの画面を表示しているときでも、音声対応リモコンのマイクボタンを押すと、Alexaクラウドサービスを使用したグローバル検索が起動します (アプリのLeanbackライブラリは使用されません)。

* TOC
{:toc}

## 音声によるメディアのリクエスト

音声でメディアをリクエストすると (たとえば、見たいテレビ番組を言うと)、Fire TVカタログからコンテンツが返されます。カタログ内の検索は、アプリではなく、クラウドで実行されます。 

詳細については、Fire TVに検索機能を実装する方法については、[こちら][implementing-search-fire-tv]を参照してください。また、エンドユーザーの視点から見た、Fire TVのAlexa音声機能の詳細については、「[Alexa on Fire TV](https://www.amazon.com/gp/help/customer/display.html?nodeId=201859020)」を参照してください。

グローバル検索の結果にアプリのメディアを表示する場合は、Fire TVカタログにメディアを組み込む必要があります。Fire TVカタログには、Fire TVのすべてのメディアコンテンツのインデックスが含まれています。 

アプリのコンテンツをカタログに組み込むための処理を "カタログ統合" と呼びます。カタログ統合では、コンテンツをカタログサービスに定期的にプッシュする必要があります (カタログサービスでは、フィードからの読み取りは行われません)。これは、開発者が構成する必要のあるタスクです。Fire App Builderはカタログ統合を自動的には実行しません。詳細については、「[カタログとFire TVの統合][integrating-your-catalog-with-fire-tv]」を参照してください。

検索が開始されると、Fire TVはインテントのブロードキャストをアプリに送信します。インテント (intent) ("intention" の省略形) とは、アプリが目的のアクションを実行するためのメッセージです。アプリでは、マニフェストファイルでインテントフィルターが宣言されている必要があります。アプリはインテントフィルターを使ってこのインテントをリッスンし、それに基づいてアクションを実行します(詳細については、Androidのドキュメントの「[インテント フィルタ](https://developer.android.com/guide/topics/manifest/manifest-intro.html#ifs)」を参照してください)。Fire App Builderには、アプリがブロードキャストインテントをリッスンするためのコードが用意されています。インテントのリッスンを開始する準備ができたら、そのコードのコメントを解除してください(後述する「[ブロードキャストインテントをリッスンするようにアプリを構成する](#intents)」を参照してください)。

Fire TVカタログにメディアを組み込み、インテントフィルタリングを使用してアプリのマニフェストを構成すると、ユーザーがアプリをすでにインストールしている場合は、アプリのコンテンツが直接、検索結果に表示されます。ユーザーがアプリをインストールしていない場合は、[More Ways to Watch] オプションが表示されます。ユーザーはこのオプションを使用して、アプリを取得し、そのコンテンツを表示できます。


## a. Fire TVカタログにアプリのメディアを組み込む

Fire TVカタログにアプリのメディアを組み込む手順については、「[カタログとFire TVの統合][integrating-your-catalog-with-fire-tv]」を参照してください。

Fire App Builderプロジェクトでは、「[アプリとFire TVホーム画面ランチャーの統合][launcher-integration]」で説明されている手順がすでに完了しています。そのため、必要となる作業は、次のセクションの説明に従って、マニフェストファイルの一部のコードのコメントを解除することだけです。

## b. フィードとカタログ申請にコンテンツIDを含める

カタログの詳細には、コンテンツアイテムごとに一意のIDが必要です。この一意のコンテンツIDは、メディアフィードのコンテンツIDと一致している必要があります。各メディアコンテンツアイテムの一意のIDがメディアフィードに含まれてない場合は、追加する必要があります。 

また、クラウドにあるカタログの詳細と (アプリに組み込まれる) メディアフィードは、常に同期している必要があります。

## ブロードキャストインテントをリッスンするようにアプリを構成する {#intents}

ブロードキャストインテントをアプリがリッスンするには:

1.  Android Studioで、アプリのフォルダーにある **manifests**を展開し、**AndroidManifest.xml**を開きます。
2.  "Launcher Integration intents" セクションのコメントを解除します。
    
    ```xml
    <!-- Launcher integration intents -->
    <!-- Uncomment the below intent filters to enable launcher integration -->
    
    <intent-filter>
        <action android:name="PLAY_CONTENT_FROM_LAUNCHER"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
    <intent-filter>
        <action android:name="SIGN_IN_FROM_LAUNCHER"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
    ```
    
3.  **ContentBrowser > manifests**に移動して、**AndroidManifest.xml**ファイルを開きます。
4.  ランチャー統合セクションのコメントを解除します。
    
    ```xml
    <!-- Uncomment the below receiver to enable launcher integration -->
    <receiver android:name="com.amazon.android.contentbrowser.helper.LauncherIntegrationBroadcastReceiver" >
        <intent-filter>
            <action android:name="com.amazon.device.REQUEST_CAPABILITIES" >
            </action>
        </intent-filter>
    </receiver>
    ```

## カタログ統合をテストする

Fire TVのホーム画面ランチャーにアプリを統合したら、アプリをAmazonアプリストアに申請する前に、ランチャー統合を検証する必要があります。ランチャー統合テストの詳細については、「[アプリとFire TVホーム画面ランチャーの統合][launcher-integration]」を参照してください。特に、「Step 6: Test Your Launcher Integration」のセクションを参照してください。未公開アプリに対してこのテストで行うには:

1.  **ContentBrowser > java > com.amazon.android.contentbrowser > helper**に移動して、**LauncherIntegrationManager.java**を開きます。
2.  "COM_AMAZON_TV_LAUNCHER" の値を "com.amazon.tv.integrationtestonly" に置き換えます。

    ```java
    //Replace COM_AMAZON_TV_LAUNCHER value with "com.amazon.tv.integrationtestonly" when testing
    // with integration test app.
    private static final String COM_AMAZON_TV_LAUNCHER = "com.amazon.tv.integrationtestonly";
    ```

{% include links.html %}
