---
title: Facebook認証コンポーネント
permalink: fire-app-builder-facebook-auth-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Facebook認証コンポーネントを使用すると、ユーザーがメディアを視聴する (または他のアクションを実行する) 前に、Facebookを使用してログインするようユーザーに求めることができます。

* TOC
{:toc}

## Facebook認証のユーザーエクスペリエンス

メディアを再生する前にログインするようユーザーに求める場合は、ユーザーが [Content Details] 画面で [Watch Now] をクリックした後に次の画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_loginprompt" type="png" caption="PlaybackActivityに対してverifyScreenAccessをtrueに設定した場合に表示されるログインプロンプト" %}

[**Later**] をクリックすると、ユーザーはFacebookにログインしないでメディアを視聴できます(現時点では、後でユーザーに再度たずねるプロンプトは表示されません)。

ユーザーが [**Now**] をクリックすると、次のように、コンピューターのブラウザでFacebookにログインするよう求められます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbprompt" type="png" caption="Facebookにログインするための画面" %}

ログイン後、Facebookで次のスクリーンショットが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbloginsuccessbrowser" type="png" caption="ブラウザでFacebookに正常にログイン" %}

ブラウザウィンドウで [**Continue**] をクリックした後、ユーザーはFire TV画面に表示されたFacebookのログイン画面で [**Submit**] をクリックします。

## 構成の概要

Facebook認証コンポーネントを構成するには、次の手順に従います。

* [手順 1. FacebookアプリIDとクライアントトークンを取得する](#getappidtoken)
* [手順 2. アプリを構成する](#configurefireappbuilderfb)
* [手順 3. FacebookアプリIDとトークン値を暗号化する](#step3)
* [手順 4. ユーザーにログインを求めるタイミングを決定する](#step4)
* [手順 5. UIテキストをカスタマイズする](#step5)

## 手順 1. FacebookアプリIDとクライアントトークンを取得する {#getappidtoken}

FacebookアプリIDとクライアントトークンを取得するには、まずアプリをセットアップする必要があります。設定したアプリの名前は、ユーザーがコンピューターのブラウザでログインすると表示されます。ユーザーに表示したい名前 (一般的には、作成するFire TV対応アプリの名前)を選択してください。

この手順は 2016 年 6 月に記載されました。その時点以降に、Facebookでボタン名、手順、ワークフローが変更されている可能性があります。

Facebookアプリを作成するには:

1.  Facebook for developersサイトの「[アプリの登録と構成](https://developers.facebook.com/docs/apps/register)」に移動し、Facebookアプリを作成します。必ず、手順 1、2、3 に従ってください。
2.  手順 3 の [**新しいFacebookアプリを作成する**] をクリックした後で、[**Android**] を選択します。
3.  [Quick Start for Android] 画面で、アプリの名前を入力します (Fire TV対応アプリと同じ名前を使用します)。次に、[**Create New Facebook App ID**] をクリックします。
4.  必須フィールドに入力し、Facebookアプリを作成します。[Tell us about your Android project] セクションでは、[Package Name] と [Default Activity Class Name] が必須ですが、これらのフィールドはGoogle Playに関連しているため、実際には使用されません。
5. [**Next**] をクリックすると、[Google Play Package Name] ダイアログボックスが表示され、Google Playでパッケージ名が見つからないことが通知されます。[**Use this package name**] をクリックして、メッセージを無視します。
6.  [Add your development and release key hashes.] という追加のセクションが表示されます。このセクションの手順に従い、キーハッシュを生成して [**Key Hashes**] フィールドに追加します。[**Next**] をクリックします。
7.  これで、Quick Start for Androidが完了しました。画面の右上にある [**My Apps**] を選択し、新しいアプリを選択します。
8. 左側のサイドバーの [**PRODUCTS**] の下で、[**+Add Product**] をクリックします。
9.  [**Facebook Login**] の横にある、[**Get Started**] をクリックします。
10.  [**Client OAuth Settings**] セクションで、[**Login from Devices**] を有効にします。
    
    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbloginsetting" type="png" caption="Facebookアプリ向けのクライアントOAuth設定" %}
    
11. [**Save Changes**] をクリックします。
12. 左側のサイドバーで、[**Settings**] > [**Basic**] の順にクリックします。**アプリのID**を便利な場所にコピーします。
13. 左側のサイドバーで、[**Settings**] > [**Advanced**] の順にクリックします。**クライアントトークン**を便利な場所にコピーします。

## 手順 2. アプリを構成する {#configurefireappbuilderfb}

Fire App BuilderのサンプルアプリではFacebook認証コンポーネントがすでにロードされていますが、構成する必要があります。

1.  Fire App Builderに組み込まれたサンプルアプリではFacebook認証コンポーネントがすでにロードされています。更新を行ったためにロードされていない場合は、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
2.  アプリにロードされている他の認証コンポーネント (AdobepassAuthComponentやLoginWithAmazonComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。
    
    {% include_relative componentnote_authentication.html %}

## 手順 3. FacebookアプリIDとトークン値を暗号化する {#step3}

暗号化されたFacebookアプリIDとクライアントトークンをアプリに挿入する必要があります。アプリIDとクライアントトークンを暗号化するには:

1.  **FacebookAuthComponent > res > values**フォルダーを展開し、**strings.xml**ファイルを開きます。
2.  次の文字列をコピーして、アプリの**custom.xml**ファイルに貼り付けます。
    
    ```xml
    <string name="encrypted_fb_client_token">YOUR_ENCRYPTED_FB_APP_CLIENT_TOKEN</string>
    <string name="encrypted_fb_app_id">YOUR_ENCRYPTED_FB_APP_ID</string>
    
    <string name="fb_key_1">fb_random_key_1</string>
    <string name="fb_key_2">fb_random_key_2</string>
    <string name="fb_key_3">fb_random_key_3</string>
    <string name="fb_key_4">fb_random_key_4</string>
    <string name="fb_key_5">fb_random_key_5</string>
    <string name="fb_key_6">fb_random_key_6</string>
    ```
     
     {% include tip.html content="今後の更新を組み込む際のベストプラクティスとして、このコンポーネントのXMLファイルからアプリのcustom.xmlファイルに値をコピーします。アプリのXML値によって、コンポーネントのXMLファイルのすべての値が上書きされます。" %}
      
     `encrypted_authentication_client_token`は、Facebookアプリの作成時に生成した、暗号化されたクライアントトークンです。`encrypted_authentication_app_id`は、暗号化されたFacebookアプリIDです。これらの値の暗号化は、この後の手順で行います。暗号の作成にはランダムなキーを使用します。
    
3.  これらの`fb_key_[#]` 値ごとにランダムな英数字の文字列を入力します。例:
    
    ```xml
    <string name="fb_key_1">odysseusgrEEk2000bc</string>
    <string name="fb_key_2">helengreekFAce5000ships</string>
    <string name="fb_key_3">homer@1storyTllr20</string>
    <string name="fb_key_4">latinusOdysseusson332</string>
    <string name="fb_key_5">calypsoIslandShipBLLd99</string>
    <string name="fb_key_6">athenazeusEPICodysseY77</string>
    ```
    
4.  Androidビューで、**Utils > java > com > amazon > utils > security**フォルダーを展開し、**ResourceObfuscationStandaloneUtility**クラスを開きます。
5.  `getRandomStringsForKey()` メソッドで、`fb_key_1`、`fb_key_2`、`fb_key_3` に対して使用した値をそれぞれ入力します。 
    
    たとえば、上記のうち最初の 3 つのキーが前述のコードサンプルに表示されている場合は、次のように入力します。
        
    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "odysseusgrEEk2000bc",
                "helengreekFAce5000ships",
                "homer@1storyTllr20"
        };
    }
    ```
    
    この例の各値は次のとおりです。
     
    *  `odysseusgrEEk2000bc`は、`fb_key_1` に使用されている値です。
    *  `helengreekFAce5000ships`は、`fb_key_2` に使用されている値です。
    *  `homer@1storyTllr20` は、`fb_key_3` に使用されている値です。
    
6.  `getRandomStringsForIv()` メソッドで、`fb_key_4`、`random_key_5`、`random_key_6` に対して使用した値をそれぞれ入力します。例:
    
    ```java
        private static String[] getRandomStringsForIv() {
    
            return new String[]{
                    "latinusOdysseusson332",
                    "calypsoIslandShipBLLd99",
                    "athenazeusEPICodysseY77"
            };
        }
    }
    ```
    
    この例の各値は次のとおりです。
     
    *  `latinusOdysseusson332` は、`fb_key_4` に使用されている値です。
    *  `calypsoIslandShipBLLd99` は、`fb_key_5` に使用されている値です。
*  `athenazeusEPICodysseY77` は、`fb_key_6` の値です。
    
7.  `getPlainTextToEncrypt()` メソッドで、`Encrypt_this_text`の代わりに、次のようにFacebookクライアントトークンを挿入します。
    
    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```
    
8.  **ResourceObfuscationStandaloneUtility.java**ファイルを右クリックして、**Run 'ResourceObfusc...main()** を選択します。
    
 
9.  暗号化された結果がコンソールに出力されていることを確認します。この結果は次のようになります。

    ```
    Encrypted version of plain text 123456789 is mTWxLhZeHslQFwpN3irjfQ==
    ```
    
10.  アプリの**custom.xml**ファイルにある`encrypted_fb_client_token`文字列の値に、暗号化されたアプリのIDをコピーして貼り付けます。例:
                                                                                        
    ```xml
    <string name="encrypted_fb_client_token">rneiu89EIxnk9489faoPoaQ</string>
    <string name="encrypted_fb_app_id">YOUR_ENCRYPTED_FB_APP_ID</string>
    ```
    
11. FacebookアプリIDを`getPlainTextToEncrypt()` メソッドに挿入し、(同じランダムな文字列を使用して) もう一度スクリプトを実行します。アプリの**custom.xml**ファイルの`encrypted_fb_app_id`文字列値に、暗号化されたキーをコピーします。例:
    
    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">AQ/9Qtc26GzLVSHRe1ftPw==</string>
    ```
    
    {% include tip.html content="ランダムなキーは会社のWikiなど安全な場所に保存し、いつでも簡単に取得するできるようにします。これらのキーは値の暗号化解除に必要です。" %}
      
## 手順 4. ユーザーにログインを求めるタイミングを決定する{#step4}

Facebookにログインするようユーザーに求める画面を構成できます。

1.  **Navigator.json**ファイル (アプリの**assets**フォルダーにあります) を開きます。
2.  ユーザーにログインを求める画面に対して`verifyScreenAccess`値を`true`に設定します。たとえば、ユーザーがメディアを再生する前にログインを求める場合は、次のように`PlaybackActivity`で画面へのアクセスを検証します。

    ```xml
      "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": true,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
    ```
    
    これで、ユーザーはアプリを起動してメディアを視聴しようとすると、ログインするよう求められます。

## 手順 5.UIテキストをカスタマイズする{#step5}

ユーザーにログインを求めるダイアログボックスに表示するテキストを変更するには:

1.  **ContentBrowser > res > values**の順に移動し、**strings.xml**ファイルを開きます。
2.  アプリの**custom.xml**ファイル (res > valuesにあります) に次の文字列をコピーします。

    ```xml
    <string name="optional_login_dialog_title">Login</string>
    <string name="optional_login_dialog_message">Do you want to get most of your app by logging in now?</string>
    <string name="now">Now</string>
    <string name="later">Later</string>
    ```
3.  文字列値をカスタマイズします。
    
## ユーザーがログインを延期できないようにする
    
デフォルトでは、ユーザーはFacebookへのログインを延期できます。現在、ユーザーがこの画面で [**Later**] をクリックしても、実際には後でログインを求められることがありません。これはバグです。ユーザーがFacebookへのログインを延期できないようにするには:

1.  **FacebookAuthComponent > res > values**フォルダーを展開して、**custom.xml**ファイルを開きます。
2.  アプリの**custom.xml**ファイル (res > valuesにあります) に次の文字列をコピーします。
    
    ```xml
    <bool name="is_authentication_can_be_done_later">true</bool>
    ```
    
3.  文字列の値を`false`に変更します。 

## Facebook認証コンポーネントのアクションをログで確認する

ユーザーがすでにログインしているときにFire TV対応アプリを再起動すると、Facebook認証コンポーネントは、ユーザーがすでにログインしているかどうかを自動的に確認します。logcatをフィルタリングして"facebook"のみを表示すると、ユーザーがログインしていない場合は、次のような内容が表示されます。

```
06-24 17:39:18.602 29089-29089/com.amazon.android.calypso D/FacebookAuthenticationModuleInitReceiver: IAuthenticationModule initialized.
06-24 17:39:18.684 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Facebook configured and previous access token is:
06-24 17:39:43.566 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Checking if user is logged in
06-24 17:39:43.569 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Access token is null. User not logged in.
06-24 17:39:43.612 29089-29089/com.amazon.android.calypso D/Navigator: FacebookAuthenticationActivity onActivityCreated
```

このような場合、ユーザーはログインしていないため、FacebookAuthenticationActivityが作成されます。

ユーザーがログインしている場合は、logcatに次のような内容が表示されます。

```
06-24 17:46:27.933 29089-29148/com.amazon.android.calypso D/FacebookApi: Making http call to Facebook url: https://graph.facebook.com/v2.6/device/login_status
06-24 17:46:28.235 29089-29148/com.amazon.android.calypso D/FacebookApi: Response from HTTP call: {"access_token":"AAOA4zsiSjMBAJa42JB2LTTyIjq3hQTAl9RTq5FVA8QxKQFhhBlGlqGXpsqQYX9Puo5ZAZBW2eUgoyYquifpTaZAKS9SJhvJefv0hUMjGqAmjuOVNOxNjZCxQQmA23dc4Xcqhs8goZBIuYmbYKuJnltAopk5dQF4ZD","expires_in":5183946}
06-24 17:46:28.235 29089-29089/com.amazon.android.calypso D/com.amazon.android.auth.facebook.FacebookAuthentication: Storing access token: AAOA4zsiSjMBAJa42JB2LTTyIjq3hQTAl9RTq5FVA8QxKQFhhBlGlqGXpsqQYX9Puo5ZAZBW2eUgoyYquifpTaZAKS9SJhvJefv0hUMjGqAmjuOVNOxNjZCxQQmA23dc4Xcqhs8goZBIuYmbYKuJnltAopk5dQF4ZD
06-24 17:46:28.257 29089-29089/com.amazon.android.calypso D/Navigator: FacebookAuthenticationActivity onActivityPaused
06-24 17:46:28.257 29089-29089/com.amazon.android.calypso D/AnalyticsManager: FacebookAuthenticationActivity onActivityPaused, analytics tracking.
```

この場合、セッションはまだアクティブであるため、アクセストークンが取得されます。その結果、ユーザーは自動的にログインします。

イベントのレポート方法を変更したい場合は、**FacebookAuthComponent > java > com.amazon.android.auth.facebook > FacebookApi.java**の文字列値を手動でカスタマイズできます。たとえば、分析に`name`という用語を使用したくない場合は、次のように、この値を別の値にカスタマイズできます。

```
public static final String NAME = "name";
```

ログイン設定をクリアするには、Fire TV対応アプリの下部までスクロールし、[**Logout**] ボタンをクリックします。

{% include links.html %}
