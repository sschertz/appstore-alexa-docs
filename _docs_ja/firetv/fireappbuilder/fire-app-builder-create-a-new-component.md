---
title: 新しいFire App Builderコンポーネントを作成する
permalink: fire-app-builder-create-a-new-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

既存のコンポーネントを使用する以外に、インターフェースを実装するコンポーネントを自分で作成することもできます。独自のコンポーネントを作成する場合は、インターフェースを実装するJavaコードの記述が必要になります。

**独自のコンポーネントを作成するには:**

1.  実装するインターフェースを特定します。各種コンポーネントと、各コンポーネントが実装するインターフェースの一覧については、「[コンポーネントの概要][fire-app-builder-interfaces-and-components]」を参照してください。
    
    たとえば、分析コンポーネントを実装するのであれば、各分析コンポーネントに`IAnalytics`インターフェースが実装されていることが「コンポーネントの概要」に記載されています。広告コンポーネントであれば、`IAds`インターフェースが実装されています。
    
2.  インターフェースのフォルダーを開き、インターフェースの内容を確認します (または、**Shift**キーを 2 回押し、インターフェース名を入力して直接検索します)。
    
    引き続き同じサンプルを使用した場合、`IAnalytics.java`ファイルは**AnalyticsInterface** > **java** > **com.amazon.analytics** > **IAnalytics**に見つかります。
    
3.  インターフェースのそれぞれのフィールドとメソッドを実装する新しいコンポーネントを作成します。各インターフェースのJavadocは、ZIPファイルとしてダウンロードできます。 
    
    Interface IAnalytics Javadocを見ると、次のフィールドが必須であることがわかります。
    
    *  `major`
    *  `minor`
    
    また、次のメソッドも必須です。
    
    *  `collectLifeCycleData`
    *  `configure`
    *  `trackAction`
    *  `trackCaughtError`
    *  `trackState`

    {% include tip.html content="サンプルとして、他のコンポーネントがインターフェースをどのように実装しているかを確認してください。" %}
    
4.  作成するコンポーネントのAndroidManifest.xmlファイルでは、他の分析コンポーネントのパターンに従います。このマニフェストファイルには、少なくとも次の要素を (実際のコンポーネント名にカスタマイズして) 含める必要があります。

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
              package="com.amazon.analytics.yourcomponentname">
    
        <application
                android:allowBackup="true"
                android:label="@string/app_name"
                android:supportsRtl="true">
    
            <receiver android:name="com.amazon.analytics.yourcomponentname.YourComponentNameModuleInitReceiver"
                      android:enabled="true"
                      android:exported="false">
                <intent-filter>
                    <action android:name="android.amazon.module.init"/>
                </intent-filter>
            </receiver>
        </application>
    
    </manifest>
    ```
    
5.  コンポーネントのbuild.gradleファイルでも、既存のコンポーネントの例に従います。コンポーネントのbuild.gradleファイルには、少なくとも次の依存関係を含める必要があります。

    ```js
    dependencies {
        compile fileTree(include: ['*.jar'], dir: 'libs')
    
        compile project(':ModuleInterface')
        compile project(':AuthInterface')
        compile project(':Utils')

    }
    ```
6.  コンポーネントの作成が完了したら、アプリにコンポーネントをロードすることができます。「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。 

{% include links.html %}
