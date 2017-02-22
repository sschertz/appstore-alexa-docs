---
title: Googleアナリティクスコンポーネント
permalink: fire-app-builder-google-analytics-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Googleアナリティクスコンポーネントでは、Googleアナリティクスを使用してAndroidアプリから分析データを収集できます。この分析サービスの詳細については、「[Googleアナリティクス](https://developers.google.com/analytics/)」を参照してください。このコンポーネントを構成するには、Googleアナリティクスから生成したgoogle-services.jsonファイルを設定し、GoogleアナリティクスとFire App Builderのコンポーネントの間でディメンションおよび指標のインデックスを一致させる必要があります。必要なディメンションと指標が含まれるカスタムレポートも設定する必要があります。

Googleアナリティクスコンポーネントを構成するには、以下の各セクションを完了してください。

* TOC
{:toc}

## 手順 1. Googleアナリティクスコンポーネントを構成する {#step1}

1.  アプリにロードされている他の分析コンポーネント (Crashlytics、OmnitureAnalyticsComponent、LoggerAnalyticsComponentなど) を削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。

    {% include_relative componentnote_analytics.html %}

2.  Googleアナリティクスコンポーネントをアプリにロードします。コンポーネントをアプリにロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。

    {% include note.html content="次の 2 つの手順を完了するまで [Sync Gradle] ボタンをクリックしないでください。" %}

3.  Androidビューで、**Gradle Scripts**ディレクトリを展開し、**build.gradle (Project: Application)** ファイルを開きます ("Fire App Builder" を実際のプロジェクト名に置き換えます)。
4.  Googleアナリティクスコンポーネントのコメントの下にある`classpath`行をコメント解除します。

    ```
    // Uncomment when using Google Analytics component
    classpath 'com.google.gms:google-services:3.0.0'
    ```

    これで [Sync Gradle] ボタン {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_syncgradlebutton" type="png" %} をクリックすると、プロジェクトのGradleファイルを同期できます。

## 手順 2. google-services.jsonファイルを設定する {#step2}

Googleアナリティクスにより、すべての分析設定がgoogle-services.jsonファイルにパッケージ化されます。このファイルを生成してダウンロードした後、Googleアナリティクスコンポーネントのフォルダーに保存します。

1.  Googleアカウントにサインインします。
2.  Googleアナリティクスサイトの「[アナリティクスをAndroidアプリに追加する](https://developers.google.com/analytics/devguides/collection/android/v4/)」に移動します。
3.  「**設定ファイルを取得する**」セクションで、[**設定ファイルを取得**] をクリックします。
4.  [**App name**] と [**Android package name**] に情報を入力します。
    *  [App name] には、アプリの名前を入力します。
    *  [Android package name] には、GoogleアナリティクスコンポーネントのAndroidManifest.xmlファイルにあるパッケージ名**com.amazon.analytics.google**を使用します。
5.  [**Continue to choose and configure services**] をクリックします。

    画面には "Select which Google services you'd like to add to your app below" と表示されますが、統合に使用できるのはアナリティクスのみです。

6.  [**Continue to generate configuration files**] をクリックします。
7.  [**Download google-services.json**] をクリックし、JSONファイルをダウンロードします。
8.  google-services.jsonファイルを**GoogleAnalyticsComponent**フォルダーに移動します。

## 手順 3. ディメンションを設定する {#step3}

この手順では、Googleアナリティクスで設定したディメンションに対応するディメンションをアプリで設定します。[Googleアナリティクスのドキュメント](https://support.google.com/analytics/answer/1033861?hl=ja)には、"ディメンションはデータの属性です。たとえば、ディメンション「市区町村」はセッションの性質を表し、「横浜」、「川崎」などセッションが発生した市区町村を指定します。ディメンション「ページ」は、閲覧されたページのURLを表します。" という記述があります。

1.  アプリで、**GoogleAnalyticsComponent** > **java** > **com.amazon.analytics.google**を展開し、**GoogleAnalytics**ファイルを開きます。
2.  次のディメンションのインデックスを探します。

    ```
    /**
     * Custom dimension indexes.
     */
    private final int PLATFORM_IDX = 1;
    private final int SEARCH_IDX = 2;
    private final int ERROR_MSG_IDX = 10;
    private final int PLAYBACK_SOURCE_IDX = 3;
    private final int PURCHASE_RESULT_IDX = 8;
    private final int PURCHASE_SKU_IDX = 9;
    private final int TITLE_IDX = 4;
    private final int SUBTITLE_IDX = 5;
    private final int VIDEO_TYPE_IDX = 6;
    private final int PURCHASE_TYPE_IDX = 7;
    ```

    ここにあるディメンションとインデックスに対応する、Googleアナリティクスのディメンションを作成します (ここではまだコードに何もする必要はありません。ファイルは後で調整するため、開いたままにしておいてください)。

3.  [Googleアナリティクスのダッシュボード](https://analytics.google.com/)にサインインします。
4.  上部のナビゲーションバーで [**Admin**] をクリックします。
5.  [**Property**] 列 (中央の列) で、[**Custom Definitions**] > [**Custom Dimensions**] の順にクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_custom_dimensions" type="png" %}

6.  [**New Custom Dimension**] ボタンをクリックします。

    {% include warning.html content="作成できるディメンションは 20 個までです。また、ディメンションは、作成後に編集することができないため、ここでは注意してください。失敗して変更が必要な場合は、別のGoogleアカウントでのサインインが必要になることがあります。" %}

7.  最初のディメンションに、わかりやすい名前を入力します。たとえば、「PLATFORM_IDX」ではなく「Platform」と入力します。
8.  [**Scope**] は [**Hit**] (デフォルト) のままにします。
9.  [**Active**] チェックボックスをオンのままにします。
10. [**Create**] をクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_google_dimensions" type="png" %}

11. [**Done**] をクリックします。
12. 次のすべての値に対してカスタムディメンションを作成するまで、この処理を繰り返します。
    * Platform
    * Search Term
    * Error Message
    * Playback Source
    * Purchase Source
    * Purchase Result
    * Purchase SKU
    * Title
    * Subtitle
    * Video Type
    * Purchase Type

    {% include note.html content="ここにある名前 (\"Platform\" など) は、GoogleAnalytics.javaファイルにある名前 (\"PLATFORM_IDX\" など) と一致させる必要はありません。Googleは、インデックス値に基づいてディメンションを一致させます。" %}

13. **GoogleAnalytics.java**ファイル (GoogleAnalyticsComponent > java > com.amazon.analytics.googleにあります) のインデックス番号を、Googleアナリティクスのダッシュボードで自動的に作成されたディメンションのインデックス番号 (以下のスクリーンショットで黄色で強調表示されています) に対応するよう変更します。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_index_values" type="png" %}

    (Googleアナリティクスではインデックス値をカスタマイズできないため、コンポーネントのインデックス値を、Googleによって作成されたインデックス値に一致させる必要があります。)

    ```java
    /**
     * Custom dimension indexes.
     */
    private final int PLATFORM_IDX = 1;
    private final int SEARCH_IDX = 2;
    private final int ERROR_MSG_IDX = 10;
    private final int PLAYBACK_SOURCE_IDX = 3;
    private final int PURCHASE_RESULT_IDX = 8;
    private final int PURCHASE_SKU_IDX = 9;
    private final int TITLE_IDX = 4;
    private final int SUBTITLE_IDX = 5;
    private final int VIDEO_TYPE_IDX = 6;
    private final int PURCHASE_TYPE_IDX = 7;
    ```

    Googleのディメンションをまったく同じ順序で作成した場合を除き、インデックス値は若干異なる可能性があります。Googleアナリティクスで作成した各ディメンション (例: Error Message, 10) がコンポーネントのディメンションとインデックス (例: `ERROR_MSG_IDX = 10`) に対応していることを確認してください。

## 手順 4. 指標のインデックスを設定する{#step4}

このセクションでは、指標のインデックスに対して、前のセクションと同じ手順を実行します。[Googleアナリティクスのドキュメント](https://support.google.com/analytics/answer/1033861?hl=ja)には、"指標はデータを定量化したものです。指標「セッション」はセッションの合計数です。指標「ページ/セッション」は、セッションあたりの平均閲覧ページ数です。" という記述があります。

1.  アプリで、**GoogleAnalyticsComponent** > **java** > **com.amazon.analytics.google**を展開し、**GoogleAnalytics**ファイルを開きます。2.  次の指標のインデックスを探します。

    ```
    /**
     * Custom metric indexes.
     */
    private final int AD_SECONDS_WATCHED_IDX = 1;
    private final int VIDEO_SECONDS_WATCHED_IDX = 2;
    private final int VIDEO_ID_IDX = 3;
    private final int AD_ID_IDX = 4;
    ```

    ディメンションと同様に、ここにある指標とインデックスに対応する、Googleアナリティクスの指標を作成します (ここではまだコードに何もする必要はありません。ファイルは後で調整するため、開いたままにしておいてください)。

3.  [Googleアナリティクスのダッシュボード](https://analytics.google.com/)にサインインします。4.  上部のナビゲーションバーで [**Admin**] をクリックします。5.  [**Property**] 列 (中央) で、[**Custom Definitions**] > [**Custom Metrics**] の順にクリックします。

6.  [**New Custom Metric**] ボタンをクリックします。

    {% include warning.html content="作成できる指標は 20 個までです。また、指標は、作成後に編集することができないため、ここでは注意してください。失敗して変更が必要な場合は、別のGoogleアカウントでのサインインが必要になることがあります。" %}

7.  最初の指標に、わかりやすい名前を入力します。たとえば、「AD_SECONDS_WATCHED_IDX」ではなく「Ad Seconds Watched」と入力します。
8.  他の値 (Scope、Formatting Type、Minimum Value、Maximum Value、およびActive) は、特に調整が必要な場合を除き、デフォルトのままにします。9.  [**Create**] をクリックします。
10. [**Done**] をクリックします。
11. 次のすべての値に対してカスタムディメンションを作成するまで、この処理を繰り返します。
    * Ad Seconds Watched
    * Video Seconds Watched
    * Video ID
    * Ad ID

    {% include note.html content="前述のとおり、ここにある名前 (\"Ad Seconds Watched\" など) は、GoogleAnalytics.javaファイルにある名前 (\"AD_SECONDS_WATCHED_IDX\" など) と一致させる必要はありません。Googleは、インデックス値に基づいて指標を一致させます。" %}

12. GoogleAnalytics.javaファイル (GoogleAnalyticsComponent > java > com.amazon.analytics.googleにあります) の指標のインデックス番号を、Googleアナリティクスのダッシュボードで自動的に作成された指標のインデックス番号に対応するよう変更します。

    (Googleアナリティクスではインデックス値をカスタマイズできないため、コンポーネントのインデックス値を、Googleによって作成されたインデックス値に一致させる必要があります。)

## 手順 5. レポートを設定する {#step5}

最後の手順では、コンポーネントのディメンションと指標に基づいたレポートを作成します。

{% include note.html content="ここで提供するのは、カスタムレポートを作成するための最小限の情報のみです。完全なドキュメントについては、Googleのドキュメントで「[カスタムディメンション/指標](https://support.google.com/analytics/answer/2709828)」を参照してください。自身にとって重要な情報に基づいて、レポートのディメンションと指標を一致させる方法を選択する必要があります。" %}

1.  Googleアナリティクスで、[**Customization**] をクリックします。
2.  [**New Custom Report**] ボタンをクリックします。
3.  カスタムレポートに**タイトル**を付けます。4.  [**Dimensions**] セクションで、[**+ add dimension**] をクリックし、[**Custom Dimensions**] を展開して、必要なディメンションを選択します。
5.  [Metrics Groups] セクションで、[**+ add metric**] をクリックし、[**Custom Metrics**] を展開して、必要な指標を選択します。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_googleanalytics_customreport" type="png" %}

6.  レポートにその他の調整を加えて、[**Save**] をクリックします。

## 手順 6. すべてが動作していることを確認する {#step6}

次の手順に従って、Googleの統合を確認できます。

1.  Android Studioで、アプリを起動します。
2.  Googleアナリティクスで、[**Reporting**] をクリックし、[**Real-Time**] > [**Overview**] の順に移動します。

    アクティブなユーザーが表示されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_googleanalytics_activeusers" type="png" %}

{% include links.html %}
