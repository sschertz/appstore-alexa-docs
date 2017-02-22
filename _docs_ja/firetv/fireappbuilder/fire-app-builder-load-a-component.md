---
title: アプリにコンポーネントをロードする
permalink: fire-app-builder-load-a-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

利用可能なコンポーネントの概要については、「[コンポーネントの概要][fire-app-builder-interfaces-and-components]」を参照してください。コンポーネントごとにドキュメントが用意されていますが、ロードする方法はどのコンポーネントも同じです。

* TOC
{:toc}

## コンポーネントを削除する {#removeacomponent}

分析用コンポーネントを除いて、インターフェース 1 つにつきロードできるコンポーネントは 1 つだけです。たとえば、VAST広告コンポーネントとFreewheel広告コンポーネントは同じ`IAds`インターフェースを使用しているため、両方をロードすることはできません。

ただし、すべてのコンポーネントの要件として、コンポーネントをアプリにロードしないとそのインターフェースを使用できません。そのため、この要件を満たすためだけのダミーコンポーネントが用意されています。以下はダミーコンポーネントです。

*  LoggerAnalyticsComponent
*  PassThroughAdsComponent

これらのダミーコンポーネントは何の動作もしません。コンポーネントインターフェースグループの一覧については、「コンポーネントの概要」の「[利用可能なインターフェースとコンポーネント][fire-app-builder-interfaces-and-components#componentgroups]」を参照してください。

アプリにコンポーネントをロードする前に、そのインターフェースに対応するコンポーネントが 1 つしかないことを確認してください。別のコンポーネントがすでにロードされている場合 (ダミーコンポーネントも含めて、よくあります)、それを削除する必要があります。

アプリからコンポーネントを削除するには、次のセクション「アプリにコンポーネントをロードする」に記載された各手順を逆向きに行います。たとえば、`FacebookAuthComponent`などのコンポーネントへの参照を追加する代わりに、コンポーネントから参照を削除します。

## アプリにコンポーネントをロードする {#loadcomponent}

アプリにコンポーネントをロードするには:

*  [1. コンポーネントを定義する](#define)
*  [2. プロジェクト内のコンポーネントをコンパイルする](#compile)
*  [3. コンポーネントの値を設定する](#configure)

### 1. コンポーネントを定義する {#define}

{% include note.html content="以下の手順では、Androidビューが表示されている状態を想定しています。" %}

まず、実装するコンポーネントを "settings.gradle (Project Settings)" ファイルに定義する必要があります。

1.  Android Studioで、[**Gradle Scripts**] を展開します。
2.  **settings.gradle (Project Settings)** ファイルを開きます(このファイルは一覧の末尾付近に表示されます)。
3.  アプリに加えたい実装を定義します。settings.gradle (Project Settings) には、コンポーネントを定義する場所が 2 つあります。それぞれの場所は `/* Implementations */` コメントで特定できます。次の例では<span class="red">赤</span>で強調されている部分です。

    <pre>
    include ':app',
        /* Frameworks */
        ':TVUIComponent',
        ':UAMP',
        /* Libraries */
        ':ContentModel',
        ':ContentBrowser',
        ':DynamicParser',
        ':DataLoader',
        ':Utils',
        /* Interfaces */
        ':PurchaseInterface',
        ':AuthInterface',
        ':AdsInterface',
        ':AnalyticsInterface',
        ':ModuleInterface',
        <span class="red">/* Implementations */</span>
        ':PassThroughAdsComponent',
        ':AMZNMediaPlayerComponent',
        ':FlurryAnalyticsComponent',
        ':LoginWithAmazonComponent',
        ':AmazonInAppPurchaseComponent'

    /* Frameworks */
    project(':TVUIComponent').projectDir = new File(rootProject.projectDir, '../TVUIComponent/lib')
    project(':UAMP').projectDir = new File(rootProject.projectDir, '../UAMP')

    /* Libraries */
    project(':ContentModel').projectDir = new File(rootProject.projectDir, '../ContentModel')
    project(':ContentBrowser').projectDir = new File(rootProject.projectDir, '../ContentBrowser')
    project(':DynamicParser').projectDir = new File(rootProject.projectDir, '../DynamicParser')
    project(':DataLoader').projectDir = new File(rootProject.projectDir, '../DataLoader')
    project(':Utils').projectDir = new File(rootProject.projectDir, '../Utils')

    /* Interfaces */
    project(':ModuleInterface').projectDir = new File(rootProject.projectDir, '../ModuleInterface')
    project(':AdsInterface').projectDir = new File(rootProject.projectDir, '../AdsInterface')
    project(':AuthInterface').projectDir = new File(rootProject.projectDir, '../AuthInterface')
    project(':AnalyticsInterface').projectDir = new File(rootProject.projectDir, '../AnalyticsInterface')
    project(':PurchaseInterface').projectDir = new File(rootProject.projectDir, '../PurchaseInterface')

    <span class="red">/* Implementations */</span>
    project(':AMZNMediaPlayerComponent').projectDir = new File(rootProject.projectDir, '../AMZNMediaPlayerComponent')
    project(':PassThroughAdsComponent').projectDir = new File(rootProject.projectDir, '../PassThroughAdsComponent')
    project(':FlurryAnalyticsComponent').projectDir = new File(rootProject.projectDir, '../FlurryAnalyticsComponent')
    project(':FacebookAuthComponent').projectDir = new File(rootProject.projectDir, '../FacebookAuthComponent')
    project(':AmazonInAppPurchaseComponent').projectDir = new File(rootProject.projectDir, '../AmazonInAppPurchaseComponent')
    </pre>

    Fire App Builderプロジェクトのソースディレクトリで各コンポーネントの正確なディレクトリ名を確認するか、各コンポーネントの名前をリストアップした上記のテーブルを参照してください。

    Fire App Builderのサンプルアプリには、以下のコンポーネントがデフォルトで実装されています。

    * AMZNMediaPlayerComponent    
    * PassThroughAdsComponent
    * FlurryAnalyticsComponent
    * FacebookAuthComponent
    * AmazonInAppPurchaseComponent

4.  アプリで使用したいコンポーネントを追加または削除して実装を調整します。必ず <span class="red">`/*Implementations*/`</span> のコメントがある両方の場所で更新を行ってください。

    コンポーネント名については、プロジェクトのダウンロード先ディレクトリに移動 (エクスプローラーまたはFinderを使用) してコンポーネントフォルダーを確認するか、上記のテーブルにリストアップされている名前を使用することができます。

5.  インターフェースごとに 1 つのコンポーネントしか実装できないため、同じインターフェースについて実装済みのコンポーネントがあれば削除します。

    たとえば、`IAuthentication`インターフェースを実装するLoginWithAmazonComponentを追加した場合は、他の認証コンポーネント (AdobepassAuthComponentやFacebookAuthComponentなど) があれば削除する必要があります。

### 2. プロジェクト内のコンポーネントをコンパイルする {#compile}

settings.gradleファイルにコンポーネントを定義した後は、"build.gradle (Module: app)" ファイルを通じてそのコンポーネントをロードする必要があります。

1.  Android Studioで、[**Gradle Scripts**] を展開します。
2.  **build.gradle (Module: app)** ファイルを開きます。
3.  **dependencies**オブジェクトに、アプリに加えたいコンポーネントを含めます。

    Fire App Builderには、デフォルトで以下のコンポーネントが表示されます。

    ```
    compile project(':TVUIComponent')
    compile project(':UAMP')
    compile project(':AMZNMediaPlayerComponent')
    compile project(':PassThroughAdsComponent')
    compile project(':FlurryAnalyticsComponent')
    compile project(':FacebookAuthComponent')
    compile project(':AmazonInAppPurchaseComponent')
    ```

    上記と同じパターンに従って実装したいコンポーネントを追加または削除してください。

4.  インターフェースごとに 1 つのコンポーネントしか実装できないため、同じインターフェースについて実装済みのコンポーネントがあれば削除します。

    たとえば、`IAuthentication`インターフェースを実装するLoginWithAmazonComponentを追加した場合は、他の認証コンポーネント (AdobepassAuthComponentやFacebookAuthComponentなど) があれば削除する必要があります。

5.  [**Resync Project with Gradle Files**] ボタン {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_syncgradlebutton" type="png" %} をクリックし、プロジェクトをGradleと再同期します。

    プロジェクトを再ビルドすると、[Project] ペインに新しいコンポーネントが反映されるのがわかります。

6.  [**Build**] > [**Rebuild Project**] の順に移動して、古いビルドアーティファクトを以前のコンポーネントから削除します。


### 3.コンポーネントの値を設定する {#configure}

各コンポーネントにはカスタマイズ可能な文字列のリストがあり、XMLファイルに抽出されています。そのため、アプリに合わせてコンポーネントをカスタマイズする際には、文字列に適切な値を指定するだけで済みます。Javaクラスに変更を加える必要はありません。

ただし、この操作は通常、サードパーティサービスの実装と設定を伴うため、コンポーネントによっては設定の手間が少し余計にかかる場合があります。コンポーネントの設定方法の詳細については、以下に挙げる各コンポーネントのドキュメントを参照してください。

*  [AdobepassAuthComponent][fire-app-builder-adobe-pass-auth-component]
*  [FacebookAuthComponent][fire-app-builder-facebook-auth-component]
*  [FlurryAnalyticsComponent][fire-app-builder-flurry-analytics-component]
*  [OmnitureAnalyticsComponent][fire-app-builder-omniture-analytics-component]
*  [CrashlyticsComponent][fire-app-builder-crashlytics-component]
*  [FreeWheelAdsComponent][fire-app-builder-freewheel-ads-component]
*  [VastAdsComponent][fire-app-builder-vast-ads-component]
*  [AmazonInAppPurchasingComponent][fire-app-builder-amazon-in-app-purchase-component]
*  [AMZNMediaPlayerComponent][fire-app-builder-amazon-media-player-component]
<!--*  [BrightCoveMediaPlayerComponent][fire-app-builder-brightcove-media-player-component]-->

コンポーネントごとに抽出されている文字列は、よく使用されるパラメータのみです。文字列への抽出が行われていない機能をコンポーネントに追加する場合は、インターフェースを実装するコンポーネントのクラスでカスタマイズする必要があります。

各コンポーネントのXMLファイルは、通常はコンポーネントの**res > values**フォルダー内にあります。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_stringcustomization" type="png" caption="コンポーネントごとにカスタマイズする必要のある一般的な値がXMLファイルに抽出されます。" %}

{% include note.html content="コンポーネントのファイル内で値を直接カスタマイズするのではなく、文字列を**アプリ**のcustom.xmlファイルにコピーしてください。コンポーネント内のXMLファイルの値は、アプリのcustom.xmlファイルの設定値によって上書きされます。これにより、新しいリリースが提供された場合にコンポーネントに更新内容を組み込むことができます。" %}

{% include links.html %}
