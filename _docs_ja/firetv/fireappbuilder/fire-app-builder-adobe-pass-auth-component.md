---
title: Adobe Pass認証コンポーネント
permalink: fire-app-builder-adobe-pass-auth-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Adobe Primetime (旧称Adobe Pass) は、メディアを表示する前にユーザーにログインを求める認証メカニズムを提供します。ユーザーは各自のISPまたはコンテンツプロバイダーにサインインし、その認証情報に基づいてアプリで認証されます。Adobe Primetimeの詳細については、[こちら](http://www.adobe.com/marketing-cloud/primetime-tv-platform.html)を参照してください。

{% include note.html content="\"Adobe Primetime\"はコンポーネントのコード作成時には\"Adobe Pass\"と呼ばれていました。Adobe Passという名前はコード内では引き続き使用されており、コンポーネントの名前はAdobePassAuthComponentです。そのため、このドキュメントではAdobe PassとAdobe Primetimeはほぼ同じ意味で使用されています。" %}

* TOC
{:toc}

## Adobe Passを使用した場合のユーザーエクスペリエンス

サンプルアプリでのAdobe Pass/Primetimeの構成例を次に示します。

ユーザーが [Content Details] 画面で [Watch Now] ボタンをクリックすると、Adobe Primetimeのログインプロンプトが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassprompt" type="png" caption="Adobe Primetimeのログインプロンプト" %}

ユーザーはコンピューターでブラウザを開き、指定されたURL (この例では、www.example.com/amazon/firetv) にアクセスし、登録コードを入力します。ケーブルプロバイダーにもサインインします。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassbrowserlogin" type="png" caption="Adobe Passを使用した認証では、ユーザーがケーブルプロバイダーにサインインする必要があります。" %}

ユーザーが登録コードとケーブルプロバイダーの認証情報を入力して、ログインすると、ログインに成功したことを示す次のような画面が表示されます(これらのURLと画面は、Adobe Primetimeアカウントを使用して構成します)。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassloginsuccess" type="png" caption="ログインに成功しました。" %}

ユーザーはFire TVに戻り、[**Submit**] ボタンをクリックして、ログインします。これで、メディアを視聴できます。

ログインに失敗すると、それを示すエラーメッセージが画面に表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepasserror" type="png" caption="Adobe Primetimeのログイン試行に失敗した場合に表示されることがあるエラーメッセージ画面。" %}

## Adobe Pass認証コンポーネントを構成する

Adobe Passコンポーネントを構成するには、次の 5 つの手順を実行します。

*  [手順 1. アプリでAdobe Pass認証コンポーネントを構成する](#configurecomponent)
*  [手順 2. Adobe Passの鍵を暗号化する](#generatingyourkeys)
*  [手順 3. Adobe Passのログインプロンプトの文字列を構成する](#configurestrings)
*  [手順 4. Adobe Passの画面のスタイルをカスタマイズする](#customizestyles)
*  [手順 5. ユーザーにログインを求める画面を構成する](#configurescreens)

## 手順 1. アプリでAdobe Pass認証コンポーネントを構成する {#configurecomponent}

Adobe Pass認証コンポーネントには、カスタマイズ可能な 3 つの独立したファイルグループがあります。それらのファイルを使って、Adobe Passの情報と、ユーザーに表示するFire TVユーザーインターフェースを構成します。

Adobe Pass認証コンポーネントを構成するには:

1.  Adobe Pass認証コンポーネントをアプリにロードします。コンポーネントをアプリにロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
2.  アプリにロードされている他の認証コンポーネント (FacebookAuthComponentやLoginWithAmazonComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。

    {% include note.html content="インターフェース 1 つにつきロードできるコンポーネントは *1 つ*だけです。たとえば、Adobe Pass認証コンポーネントとパススルーログインコンポーネントは同じ`IAuthentication`インターフェースを使用しているため、*両方*をロードすることはできません。インターフェースごとのコンポーネントのリストについては、「[コンポーネントの概要][fire-app-builder-interfaces-and-components]」を参照してください。" %}

3.  **AdobePassAuthComponent > res > values**に移動し、**custom.xml**ファイルを開きます。
4.  次の値をコピーしてアプリの**custom.xml**ファイルに貼り付けます。

    ```xml
    <!--Adobe pass clientless API Requestor ID -->
    <string name="adobe_pass_requestor_id">YOUR REQUESTOR ID</string>
    <!-- Encrypted Adobe pass public key for your application, encrypt it using
    KeyEncrypterStandaloneUtility -->
    <string name="encrypted_adobe_pass_public_key">YOUR ENCRYPTED PUBLIC KEY</string>
    <!-- Encrypted Adobe pass secret key for your application, encrypt it using
    KeyEncrypterStandaloneUtility -->
    <string name="encrypted_adobe_pass_private_key">YOUR ENCRYPTED PRIVATE KEY</string>
    <!--Adobe pass clientless API registration URL for second screen login -->
    <string name="adobe_pass_registration_url">YOUR REGISTRATION URL</string>
    <!--Adobe pass clientless API time to live value for the registration token -->
    <string name="adobe_pass_registration_code_ttl">YOUR TIME TO LIVE VALUE</string>
    <!-- URL used by user to authenticate -->
    <string name="adobepass_login_instruction_line_2">Visit YOUR_AUTHENTICATION_URL</string>
    <!-- PseudoRandom strings, used to generate random key for encrypting/decrypting resources.
    These keys should always remain in sync with the keys used by encrypting utility -->
    <string name="random_key_1">random_key_1</string>
    <string name="random_key_2">random_key_2</string>
    <string name="random_key_3">random_key_3</string>
    <string name="random_key_4">random_key_4</string>
    ```

5.  次の表の説明に従って、各プロパティの値をカスタマイズします。

    <table class="grid">
    <colgroup>
    <col width="40%" />
    <col width="60%" />
    </colgroup>
      <thead>
        <tr>
          <th>値</th>
          <th>説明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>adobe_pass_requestor_id</code></td>
          <td>Adobe PrimetimeのリクエスターID。この値はAdobeから提供されます。</td>
        </tr>
        <tr>
          <td><code>encrypted_adobe_pass_public_key</code></td>
          <td>Adobe Primetimeの暗号化された公開鍵 (Adobeから提供されます)。この鍵はAdobeから提供されますが、暗号化する必要があります。この鍵の生成の詳細については、「<a href="#generatingyourkeys">Adobe Primetimeの鍵を暗号化する</a>」を参照してください。</td>
        </tr>
        <tr>
          <td><code>encrypted_adobe_pass_private_key</code></td>
          <td>Adobe Primetimeの暗号化された秘密鍵。この鍵はAdobeから提供されますが、暗号化する必要があります。この鍵の生成の詳細については、「<a href="#generatingyourkeys">Adobe Primetimeの鍵を暗号化する</a>」を参照してください。</td>
        </tr>
        <tr>
          <td><code>adobe_pass_registration_url</code></td>
          <td>登録URL。この値はAdobeから提供されます。</td>
        </tr>
        <tr>
          <td><code>adobe_pass_registration_code_ttl</code></td>
          <td>登録コードの有効期限。</td>
        </tr>
        <tr>
          <td><code>adobepass_login_instruction_line_2</code></td>
          <td>ユーザーがログインする場所に関する情報。</td>
        </tr>
        <tr>
          <td><code>random_key_1</code></td>
          <td>公開鍵と秘密鍵の暗号化に使用されるランダムな文字列。値には、任意の英数字の文字列を入力します。</td>
        </tr>
        <tr>
          <td><code>random_key_2</code></td>
          <td>公開鍵と秘密鍵の暗号化に使用されるランダムな文字列。値には、任意の英数字の文字列を入力します。</td>
        </tr>
        <tr>
          <td><code>random_key_3</code></td>
          <td>公開鍵と秘密鍵の暗号化に使用されるランダムな文字列。値には、任意の英数字の文字列を入力します。</td>
        </tr>
        <tr>
          <td><code>random_key_4</code></td>
          <td>公開鍵と秘密鍵の暗号化に使用されるランダムな文字列。値には、任意の英数字の文字列を入力します。</td>
        </tr>
      </tbody>
    </table>

## 手順 2. Adobe Primetimeの鍵を暗号化する {#generatingyourkeys}

Adobe Primetimeアカウントをセットアップすると、公開鍵と秘密鍵が提供されます。これらの値を安全に保持するために、Fire App BuilderのAdobe Passコンポーネントでは、セキュリティアルゴリズムを使用して鍵を暗号化します。このアルゴリズムは、アプリのUtilsフォルダーにある`ResourceObfuscator`クラスと`ResourceObfuscationStandaloneUtility`クラスを使用して実装されます。

Adobe Primetimeの公開鍵と秘密鍵を暗号化するには:

1.  Androidビューで、**Utils > java > com > amazon > utils > security**フォルダーの順に展開し、**ResourceObfuscationStandaloneUtility**クラスを開きます。
2.  `getRandomStringsForKey()` メソッドで、(コンポーネントのcustom.xmlファイル内にある) `random_key_1`、`random_key_4`、および`random_key_3` に使用した値をそれぞれ入力します。

    たとえば、custom.xmlファイルで次のランダムな文字列を使用したとします。

    ```xml
    <string name="random_key_1">calypso</string>
    <string name="random_key_2">dadadadadappppp</string>
    <string name="random_key_3">more_random_stuff</string>
    <string name="random_key_4">something_random</string>
    ```
    その場合は、`ResourceObfuscationStandaloneUtility`クラスの文字列を次のようにカスタマイズします。

    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "calypso",
                "something_random",
                "more_random_stuff"
        };
    }
    ```

    この例では、値は次のとおりです。

    *  `calypso`は`random_key_1` に使用した値です。
    *  `something_random`は`random_key_4` に使用した値です。
    *  `more_random_stuff`は`random_key_3` に使用した値です。

4.  `getRandomStringsForIv()` メソッドで、`random_key_2` および`random_key_3` に使用した値をそれぞれ入力します。

    ```java
        private static String[] getRandomStringsForIv() {

            return new String[]{
                    "dadadadadappppp",
                    "more_random_stuff"
            };
        }
    }
    ```

    この例では、値は次のとおりです。

    *  `dadadadadappppp`は`random_key_2` に使用した値です。
    *  `more_random_stuff`は`random_key_3` に使用した値です(前述と同じです)。

5.  `getPlainTextToEncrypt()` メソッドで、`Encrypt_this_text`の代わりにAdobe Passの公開鍵を挿入します。

    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```

6.  **ResourceObfuscationStandaloneUtility.java**ファイルを右クリックし、[**Run 'ResourceObfusc...main()**] を選択します。


7.  暗号化された結果がコンソールに出力されていることを確認します。これは次のように表示されます。

    ```
    Encrypted version of plain text 123456789 is gnobHJEIxnkBMobJk7mBaQ==
    ```

8.  暗号化された鍵をコピーします。前のセクションの手順に従って、この鍵をアプリの**custom.xml**ファイルの`encrypted_adobe_pass_public_key`の値に貼り付けます。次に例を示します。

    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">YOUR ENCRYPTED PRIVATE KEY</string>
    ```

9.  Adobe Passの*秘密*鍵を`getPlainTextToEncrypt()` メソッドに挿入し、(同じランダム文字列を使用して) スクリプトを再実行します。暗号化された鍵をコピーして、アプリの**custom.xml**ファイルの`encrypted_adobe_pass_private_key`の文字列値に貼り付けます。次に例を示します。

    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">bhjKDUYhdlkNNbUEYyvbn==</string>
    ```

    {% include tip.html content="会社のWikiなどの安全な場所にランダム鍵を保存しておくと、いつでも簡単に取り出すことができます。" %}

### 他の値を暗号化する

暗号化ユーティリティは、Adobe Primetimeの鍵だけでなく、アプリ内で暗号化するどの鍵にも使用できます。`ResourceObfuscatorStandaloneUitility`クラスを使用して鍵を暗号化し、`ResourceObfuscator`クラスを使用して鍵の暗号化を解除することができます。

Adobe Passコンポーネントでは、鍵の暗号化解除に`ResourceObfuscator`クラスが使用されています。(コンポーネントのcustom.xmlファイルに) 入力したランダムな文字列は、暗号化を行う`ResourceObfuscator`クラスに渡されます。Adobe Pass認証コンポーネントのAdobepassRestClient.javaクラスは、この`ResourceObfuscator`クラスをインスタンス化し、ランダムな文字列を渡します。

```java
  ResourceObfuscator obfuscator = new ResourceObfuscator();

        String plainKey = obfuscator.unobfuscate(key, getRandomStringsForKey(appContext),
                                                 getRandomStringsForIv(appContext));
        return plainKey;
    }
```

この暗号化手法にはハッキング耐性がないことに注意してください。もっと強力な暗号化方法はありますが、このアルゴリズムは悪意のあるユーザーが鍵を簡単に見つけて使用するのを防ぐのに役立ちます。

## 手順 3. Adobe Primetimeのログインプロンプトの文字列を構成する {#configurestrings}

Adobe Primetimeのログインプロンプト画面に表示される文字列を構成できます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassprompt" type="png" caption="Adobe Primetimeのログインプロンプト" %}

ログインに失敗した場合にエラーメッセージ画面に表示されるテキストを制御することもできます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepasserror" type="png" caption="Adobe Passのエラーメッセージ" %}

これらの画面に表示されるテキストをカスタマイズするには:

1.  **AdobepassAuthComponent > res > values**に移動し、**strings.xml**ファイルを開きます。
2.  各要素の文字列値をカスタマイズします。

    ```xml
    <string name="app_name">AdobepassAuthComponent</string>

       <string name="title_activity_adobe_authentication">Authentication</string>
       <string name="adobepass_login_instruction_line_1">Go to your computer or mobile device</string>
       <string name="adobepass_login_instruction_line_3">Enter the following case-sensitive code:</string>
       <string name="adobepass_login_instruction_line_4">Loading...</string>
       <string name="btn_submit">SUBMIT</string>
       <string name="btn_get_new_code">GET NEW CODE</string>
       <string name="adobe_pass_error_authentication_message">There was an error authenticating the account. Please try again later.</string>
       <string name="adobe_pass_error_registration_message">There was an error authenticating the account. Please try again later.</string>
       <string name="adobe_pass_no_authorization_message">Your subscription package does not include this video.
    ```

## 手順 4. Adobe Passの画面のスタイルをカスタマイズする {#customizestyles}

アプリ内のAdobe Primetimeのログインユーザーインターフェースのロゴと色をカスタマイズできます。

スタイルをカスタマイズするには:

1.  **AdobepassAuthComponent > res > values**に移動し、**styles.xml**ファイルを開きます。
2.  各要素の文字列値をカスタマイズします。要素名とテキストが表示にどのように影響するかは、上のスクリーンショットを参照してください。

    ```xml
        <style name="AppTheme" parent="android:Theme.Holo.Light.DarkActionBar">
        </style>

        <drawable name="company_logo">@drawable/logo</drawable>
        <drawable name="splash_background">@drawable/bg_generic_nopreview</drawable>
        <drawable name="action_button_focused">@drawable/btn_generic_focused</drawable>
        <color name="action_button_text_color">#E6FFFFFF</color>
        <color name="action_button_text_color_focused">#E6FFFFFF</color>
        <drawable name="action_button_normal">@drawable/btn_normal</drawable>
    ```

## 手順 5. ユーザーにログインを求める画面を構成する {#configurescreens}

どの画面に認証を実装するかを構成する必要があります。たとえば、[Content Renderer] 画面の`PlaybackActivity`に対してのみ認証を要求することで、アプリ内のメディアに興味を持った未認証のユーザーにログインを促すことができます。

認証を求める画面を構成するには:

1.  **Navigator.json**ファイル (**app > assets**にあります) を開きます。
2.  `graph`オブジェクトで、制限するアクティビティ (`PlaybackActivity`など) を探し、`verifyScreenAccess`をtrueに変更します。次に例を示します。

    ```json
    "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": true,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
    ```

{% include links.html %}
