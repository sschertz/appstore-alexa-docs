---
title: Crashlyticsコンポーネント
permalink: fire-app-builder-crashlytics-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

(Fabricを使用した) [Crashlytics](http://try.crashlytics.com/)では、アプリのクラッシュを中心とした分析が提供されます。Crashlyticsによると、次のように言われています。

>Crashlyticsは、iOSとAndroid向けの業界随一のクラッシュレポートプラットフォームです。Crashlyticsを使用すると、アプリ内のクラッシュに関するリアルタイムの情報や、最も影響の大きい安定性の問題に真正面から取り組むために必要なすべての詳細情報が得られます。Crashlyticsを導入するだけで、すぐにクラッシュレポートの作成が可能になります。追加のコードを記述する必要はありません。 &ndash; [Crashlytics](https://docs.fabric.io/android/crashlytics/overview.html)

詳細は、[Android向けのCrashlyticsのドキュメント](https://docs.fabric.io/android/crashlytics/overview.html)を参照してください。Crashlyticsコンポーネントには、`IAnalytics`インターフェースが実装されています。このコンポーネントを使用するには、Crashlyticsアカウントが必要です。

Crashlyticsを構成するには、次の手順に従います。

* TOC
{:toc}

## 手順 1. Crashlyticsキーを取得する {#step1}

まだCrashlyticsアカウントを持っていない場合は、アカウントにサインアップし、アプリの詳細を提供します。

1.  [https://fabric.io/sign_up](https://fabric.io/sign_up)にアクセスし、アカウントにサインアップします。
2.  確認画面とようこそ画面を進みます。
3.  "You'll be up and running in 3 steps!" と表示される画面は**何もせずに進みます**。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fabricgettingstarted" type="png" caption="この 3 つの手順でFabricプラグインをダウンロードしてインストールできますが、ここではこの画面はすべて無視してください。Android StudioにFabricプラグインをインストールしたり、Crashlyticsコンポーネントを構成したりする必要はありません。このプラグインでは、単にCrashlyticsが動作できるようにコードが更新されるだけです。以下の手順で、Fire App Builderコードに調整を加えて、同じ目的を達成します。" %}

4.  「[Install Crashlytics via Gradle](https://fabric.io/kits/android/crashlytics/install)」ページにアクセスし、「Add Your API Key」セクションを確認します。

    ログインした状態を維持していれば、コードサンプルにAPIキーが自動的に設定されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fabricapikey" type="png" caption="コードサンプルにAPIキーが自動的に設定されます。" %}

5.  この後の手順で使用するためにこのAPIキーを便利な場所にコピーします。

(すでにCrashlyticsアカウントを持っていて、これまでにCrashlyticsの統計データを表示したことがある場合は、組織の設定からAPIキーを確認することもできます。詳細については、「[Fabric API Keys](https://docs.fabric.io/android/fabric/settings/api-keys.html)」を参照してください。)

## 手順 2. Crashlyticsコンポーネントを構成する {#step2}

次の手順は、Androidビューが表示されていることを前提としています。

1.  Crashlyticsコンポーネントをアプリにロードします。コンポーネントをアプリにロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
2.  アプリにロードされている他の分析コンポーネント (FlurryAnalyticsComponent、OmnitureAnalyticsComponent、LoggerAnalyticsComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。    

    {% include_relative componentnote_analytics.html %}

3.  **Gradle Scripts**を展開し、**build.gradle (Project: Application)** ファイルを開きます (開発するアプリの名前に "Fire App Builder" とは付けないため、実際のアプリ名のbuild.gradleファイルを見つけて開いてください)。
4.  Crashlyticsコードのコメントに記述されているとおり、コードをコメント解除します。次のコードサンプルは、正しくコメント解除されたコードを示しています。

    <pre>
    buildscript {
        repositories {
            mavenCentral()
            jcenter()
            <span class="red">// Uncomment when using CrashlyticsComponent</span>
            maven { url 'https://maven.fabric.io/public' }
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:1.5.0'
            classpath 'me.tatarka:gradle-retrolambda:3.2.3'
        <span class="red">// Uncomment when using CrashlyticsComponent</span>
            classpath 'io.fabric.tools:gradle:1.+'
        //         NOTE: Do not place your application dependencies here; they belong
        //         in the individual module build.gradle files
        }
    }

    allprojects {
        repositories {
            jcenter()
            mavenCentral()
            maven { url 'http://repo.brightcove.com/releases' }
            <span class="red">// Uncomment when using CrashlyticsComponent</span>
            maven { url 'https://maven.fabric.io/public' }
        }
    }
    </pre>

5.  **CrashlyticsComponent** > **manifests**の順に移動し、**AndroidManifest.xml**ファイルを開きます。
6.  `value`プロパティにCrashlyticsキーを挿入します。

    ```xml
    android:name="io.fabric.ApiKey"
    android:value="0c508f569dc758c937d5c0046a17871dcccce2c8"/>
    ```

    {% include note.html content="**CrashlyticsComponent** > **res** > **values** > **strings.xml**には、`your_api_key`という名前の文字列があります。この文字列には何もする必要はありません。コードの上にあるコメントでは、代わりにAndroidManifest.xmlファイルにCrashlyticsキーを挿入するよう指示されています。" %}

7.  *Gradle Scripts**フォルダーを展開し、**build.gradle (Module: app)** ファイルを開きます。
8.  Crashlyticsのコメントで示されているように、次の行をコメント解除します。

    <pre>
    <span class="red">// Uncomment when using CrashlyticsComponent</span>
    apply plugin: 'io.fabric'
    </pre>

9.  **Gradle Scripts**フォルダーを展開し、**build.gradle (Module: app)** ファイルを開きます。
10. Crashlyticsのコメントで示されているように、次の行をコメント解除します。

    <pre>
      androidTestCompile 'com.jayway.android.robotium:robotium-solo:5.3.1'

        compile project(':TVUIComponent')
        compile project(':UAMP')
        compile project(':AMZNMediaPlayerComponent')
        compile project(':PassThroughAdsComponent')
        compile project(':FacebookAuthComponent')
        compile project(':AmazonInAppPurchaseComponent')
        compile project(':LoggerAnalyticsComponent')
        <span class="red">// Uncomment when using CrashlyticsComponent</span>
        compile('com.crashlytics.sdk.android:crashlytics:2.6.1@aar') {
            transitive = true;
        }
    </pre>
11.  アプリをビルドして実行します。

アプリをビルドして実行すると、Crashlyticsにより、コードが正しく構成されていることが検出されます。この時点で、[Crashlyticsのオンボード画面](https://www.fabric.io/onboard/pending)でブロックされなくなり、Crashlyticsダッシュボードに進むことができるようになります。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_crashlyticsonboardsuccess" type="png" caption="(アプリのビルドに基づいて) Crashlyticsの構成が正しいことがFabricによって検出されると、このような画面が表示され、ここからダッシュボードに進むことができます。ダッシュボードには、クラッシュの分析が表示されます。" url="https://www.fabric.io/onboard/pending" %}

## 手順 3. テストクラッシュを発生させる {#step3}

Crashlyticsの構成をテストするために、次の手順でテストクラッシュを発生させます。

1.  Androidビューで、**TVUIComponent** > **java** > **com.amazon.android** > **tv.tenfoot** > **ui** > **fragments**の順に移動し、**ContentDetailsFragment.java**ファイルを開きます。
2.  先頭付近にある`onStart()` メソッド内に次の行を追加します。

    ```java
    if(true) {
    throw new RuntimeException("Causing fake crash for Crashlytics test");
    }
    ```

3.  [**Run 'app'**] ボタン {% include inline_image.html alt="Run 'app' button" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %} をクリックしてアプリを起動します。

アプリは、起動するとすぐにクラッシュします。logcatコンソールをキーワード"crashlytics"でフィルタリングすると、次のログが表示されます。

```
07-01 13:04:02.021 28688-28688/com.amazon.android.calypso D/CrashlyticsAnalyticsModuleInitReceiver: IAnalyticsModule initialized.
07-01 13:04:02.064 28688-28688/com.amazon.android.calypso I/CrashlyticsCore: Initializing Crashlytics 2.3.8.97
07-01 13:04:04.296 28688-28787/com.amazon.android.calypso I/CrashlyticsCore: Crashlytics report upload complete: 5776CC9E029B-0001-6E05-6C83E6F7D1F2.cls
```

## 手順 4. Crashlyticsダッシュボードを探索する {#step4}

コードの設定、アプリの実行、クラッシュの発生が完了したら、Crashlyticsでは、[オンボードページ](https://www.fabric.io/onboard/pending)を介さずに、クラッシュの詳細を確認できるダッシュボードに進むことができます。

1.  [Crashlytics](https://fabric.io/kits/android/crashlytics)にログインします。
2.  右上隅にある [**Dashboard**] をクリックします。
3.  ダッシュボードで情報と設定を確認します。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_crashlytics_dashboard" type="png" caption="Crashlyticsダッシュボードに表示されているサンプルクラッシュ" %}

{% include links.html %}
