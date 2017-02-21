---
title:  Omniture Analyticsコンポーネント
permalink: fire-app-builder-omniture-analytics-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Omnitureでは、(APIキーを使用する代わりに) アプリに組み込んで使用するJARファイルが提供されています。JARファイルには、セキュリティキーなどの構成情報が格納されています。

{% include note.html content="以下の手順では、Android Studioで**Project**ビューが選択されている状態を想定しています。" %}

**Omniture Analyticsコンポーネントを設定するには:**

1.  Omniture Analyticsコンポーネントをアプリにロードします。コンポーネントをアプリにロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。2. アプリにロードされている他の分析コンポーネント (FlurryAnalyticsComponent、CrashlyticsComponent、LoggerAnalyticsComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。    

    {% include_relative componentnote_analytics.html %}
    
2.  アプリのフォルダー内に**AdobeMobileLibrary**というフォルダーを作成します。
3.  この新しい**AdobeMobileLibrary**フォルダー内に、Adobeから提供されたJARファイルを置きます。

    たとえば、JARファイルがadobeMobileLibrary-4.6.1.jarという名前の場合、ファイルへのパスは次のようになります: (アプリ名) > AdobeMobileLibrary > adobeMobileLibrary-4.6.1.jar。

4.  **AdobeMobileLibrary**フォルダーで**build.gradle**ファイルを開き、JARファイルへの参照を追加します。

    ```
    configurations.create("default")
    artifacts.add("default", file('adobeMobileLibrary-4.6.1.jar'))
    ```

5.  アプリの**settings.gradle**ファイル内にAdobeMobileLibraryコンポーネントを追加します。

    ```
    include  ':app',
             ':AdobeMobileLibrary',
    ```
6.  **OmnitureAnalyticsComponent**フォルダーで**build.gradle**ファイルを開き、**AdobeMobileLibrary**の依存関係があることを確認します。

    ```xml
    dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile project(':ModuleInterface')
    compile project(':AnalyticsInterface')
    compile project(':AdobeMobileLibrary')
    }
    ```

    これで、Omniture Analyticsを使用したアクティビティの追跡がアプリで開始されます。

{% include links.html %}
