---
title: Flurry Analyticsコンポーネント
permalink: fire-app-builder-flurry-analytics-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Flurryは、Androidアプリ向けの分析を行い、メディアの再生に関連した詳細情報を示します。[Flurry Analytics](https://developer.yahoo.com/flurry/docs/analytics/)によると、次のように言われています。

>Flurry Analytics SDKでは、アプリにおけるユーザーの動作を深く理解するために必要なツールとリソースを提供しています。指標、セグメント、ファンネルを使用して複雑なイベントの高度な分析を設定し、ユーザーの習慣と行動をより詳しく追跡します。 

Flurry Analyticsを構成するには、以下のセクションに従います。

* TOC
{:toc}

## 手順 1. Flurry APIキーを取得する {#step1}

1.  [Flurry Analytics](https://developer.yahoo.com/analytics/)でアカウントを作成し、APIキーを取得します。
2.  Flurry APIキーを便利な場所にコピーします。

## 手順 2. Flurry Analyticsコンポーネントを構成する {#step2}
 
1.  Flurry Analyticsコンポーネントをアプリにロードします。「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
    
2.  アプリにロードされている他の分析コンポーネント (Crashlytics、OmnitureAnalyticsComponent、LoggerAnalyticsComponentなど) を削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。
    
    {% include_relative componentnote_analytics.html %}
    
3.  **FlurryAnalyticsComponent** > **res** > **values** > **custom.xml**の順に移動し、次の文字列をコピーします。

    ```xml
    <string name="encrypted_flurry_api_key">YOUR_ENCRYPTED_FLURRY_API_KEY</string>

    <string name="flurry_key_1">flurry_random_key_1</string>
    <string name="flurry_key_2">flurry_random_key_2</string>
    <string name="flurry_key_3">flurry_random_key_3</string>
    <string name="flurry_key_4">flurry_random_key_4</string>
    <string name="flurry_key_5">flurry_random_key_5</string>
    <string name="flurry_key_6">flurry_random_key_6</string>
    ```
    
4.  アプリの**custom.xml**ファイル (res > valuesにあります) にこれらの文字列を貼り付けます。  

     {% include tip.html content="今後の更新を組み込む際のベストプラクティスとして、このコンポーネントのXMLファイルからアプリのcustom.xmlファイルに値をコピーします。アプリのXML値によって、コンポーネントのXMLファイルのすべての値が上書きされます。"%}
          
     `encrypted_flurry_api_key`文字列は、暗号化されたFlurry APIキーを保持します。暗号の作成には`flurry_key_[#]` 文字列を使用します。

## 手順 3. Flurry APIキーを暗号化する {#step3}

Flurry APIキーを暗号化する必要があります。

1.  アプリの**custom.xmlファイル** (res > valuesにあります) を開きます。
2.  `flurry_key_[#]` 値ごとにランダムな英数字の文字列を入力します。例:
    
    ```xml
    <string name="flurry_key_1">flurryblurry8837</string>
    <string name="flurry_key_2">furryBEAR2999</string>
    <string name="flurry_key_3">homer2YHfurrybaLL28_sneezun</string>
    <string name="flurry_key_4">curry88fl@vrczng</string>
    <string name="flurry_key_5">someFlurryBlurry9911</string>
    <string name="flurry_key_6">yrrulFbackwardZZ44</string>
    ```
    
3.  Androidビューで、**Utils** > **java** > **com** > **amazon** > **utils** > **security**フォルダーを展開し、**ResourceObfuscationStandaloneUtility**クラスを開きます。
4.  `getRandomStringsForKey()`メソッドで、`flurry_key_1`、`flurry_key_2`、`flurry_key_3` に対して使用した値をそれぞれ入力します。 
    
    たとえば、上記のうち最初の 3 つのキーが前述のコードサンプルに表示されている場合は、次のように入力します。
        
    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "flurryblurry8837",
                "furryBEAR2999",
                "homer2YHfurrybaLL28_sneezun"
        };
    }
    ```
    
    この例の値は次のとおりです。
     
    *  `flurryblurry8837` は、`flurry_key_1` に使用されている値です。
    *  `furryBEAR2999` は、`flurry_key_2` に使用されている値です。*  `homer2YHfurrybaLL28_sneezun`は、`flurry_key_3` に使用されている値です。
    
5.  `getRandomStringsForIv()` メソッドで、`flurry_key_4`、`random_key_5`、`random_key_6` に対して使用した値をそれぞれ入力します。例:
    
    ```java
        private static String[] getRandomStringsForIv() {
    
            return new String[]{
                    "someFlurryBlurry9911",
                    "calypsoIslandShipBLLd99",
                    "yrrulFbackwardZZ44"
            };
        }
    }
    ```
    
    この例の各値は次のとおりです。
     
    *  `someFlurryBlurry9911` は、`flurry_key_4` に使用されている値です。
    *  `someFlurryBlurry9911` は、`flurry_key_5` に使用されている値です。
*  `yrrulFbackwardZZ44` は、`flurry_key_6` に使用されている値です。
    
6.  `getPlainTextToEncrypt()` メソッドで、`Encrypt_this_text`の代わりに、次のようにFlurry APIキーを挿入します。
    
    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```
    
7.  **ResourceObfuscationStandaloneUtility.java**ファイルを右クリックして、**Run 'ResourceObfusc...main()** を選択します。
8.  暗号化された結果がコンソールに出力されていることを確認します。この結果は次のようになります。

    ```
    Encrypted version of plain text 123456789 is Hgei944983ljdfHoaQ==
    ```
    
9.  アプリの**custom.xml**ファイルにある`encrypted_flurry_api_key`文字列の値に、暗号化されたFlurry APIキーをコピーして貼り付けます。次に例を示します。
                                                                                        
    ```xml
    <string name="encrypted_flurry_api_key">Hgei944983ljdfHoaQ==</string>
    ```
    
    {% include tip.html content="会社のWikiなどの安全な場所にランダム鍵を保存しておくと、いつでも簡単に取り出すことができます。これらのキーは値の暗号化解除に必要です。"%}

これでFlurry Analyticsはアプリに組み込まれたので、イベントに関する情報の送信を開始します。

## ログでFlurryを確認する

アプリをビルドする際に、"flurry" という単語でフィルタリングしてlogcatを表示すると、Flurry Analyticsコンポーネントがイベントでどのようにトリガーされるかがわかります。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_flurrytracking" type="png" %}

Flurry Analyticのダッシュボードに行動データが読み込まれるまでには数時間かかりますが、イベントログは数分で確認できます。Flurry Analyticsで、[**Events**] > [**Event Logs**] の順に移動します。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_flurryeventlog" type="png" %}

{% include links.html %}
