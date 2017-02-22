---
title:  Login with Amazonコンポーネント
permalink: fire-app-builder-login-with-amazon-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Login with Amazonコンポーネントを使用すると、ユーザーがメディアの視聴などの特定のアクションを行う前に、Amazonアカウントの認証情報を使ってAmazon Fire TVアプリにログインするよう求めることができます。詳細については、「[Login with Amazon](https://developer.amazon.com/ja/login-with-amazon)」を参照してください。

Amazon Fire TVでは、Fire TVのセットアップおよび登録時に、すでにユーザーに対するAmazonアカウントでのログイン要求が行われているため、このコンポーネントの実際のメリットは、ユーザーがAmazonアカウントでログインできるという点ではありません。このコンポーネントのメリットは、ユーザーに対して、アプリとのAmazonユーザー名とEメールアドレスの共有に同意を求めることができる点です。これにより、ユーザー情報をより詳しく把握できるようになります。

* TOC
{:toc}

## Login with Amazonコンポーネントのユーザーエクスペリエンス

ユーザーがメディアを視聴しようとして [Watch Now] ボタンをクリックすると、ログインプロセスを開始するための [Login with Amazon] ボタンが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_lwaprompt" type="png" caption="図 1. [Login with Amazon] ボタン" %}

このボタンをクリックすると、(Amazonでホストされている) セキュリティで保護されたログイン画面に、(Fire TVアカウントに関連付けられている) ユーザー名とEメールアドレスが表示されます。ユーザーは、このデータ (名前とEメールアドレス) をアプリと共有することに同意するよう求められます。同意画面には、アプリの名前、ロゴ、プライバシーに関する通知が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_privacynotice" type="png" caption="図 2. プライバシーに関する同意画面。" %}

[**I agree**] をクリックしたユーザーは、メディアを視聴するためのアクセス許可を付与するトークンとともに、アプリにリダイレクトされます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_privacynoticesuccess" type="png" caption="図 3. ログインに成功してアプリに戻ります。" %}

アプリに戻ったユーザーは、[**Return**] をクリックします。ユーザーが選択したメディアの再生が開始されます。

自分の情報をアプリと共有することに同意しなかった場合、ユーザーはアプリにリダイレクトされ、前と同じLogin with Amazon画面 (図 1) が表示されます。

ログイン後は、アプリの下部にある [Logout] ボタンを使用するとログアウトできます。ユーザーが次にメディアを視聴しようとする際、再度ログインするよう求められます (図 1)。すでにユーザーが自分の情報をアプリと共有することに同意しているため、プライバシー画面 (図 2) は表示されません。

アプリによるユーザーデータへのアクセスを無効にしたい場合、ユーザーは以下の操作を行います。

1.  [https://amazon.com](amazon.com)にログインし、[**Your Account**] > [**Your Account**] の順に移動します(または、直接[こちら](https://www.amazon.com/gp/css/homepage.html)にアクセスします)。
2.  [**Settings**] で、[**Manage Login with Amazon**] をクリックします。
3.  アプリの横にある [**Remove**] をクリックします。

## 構成の概要

Login with Amazonコンポーネントを構成するには、以下の手順に従います。

*  [手順 1. アプリを構成する](#configurefireappbuilderfb)
*  [手順 2. Developer Consoleでセキュリティプロフィールを作成する](#securityprofile)
*  [手順 3. Login with Amazon APIキーを取得する](#apikey)

## 手順 1. アプリを構成する {#configurefireappbuilderfb}

Login with Amazonコンポーネントを構成するには:

1.  他のコンポーネントと同様に、[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]ための通常の手順に従います。
2.  アプリにロードされている他の認証コンポーネント (AdobepassAuthComponentやFacebookAuthComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。
    
    {% include_relative componentnote_authentication.html %}
    
2.  アプリで**Navigator.json**ファイル (**app > assets**にあります) を開きます。
3.  ユーザーにログインを求める画面に対して`verifyScreenAccess`値を`true`に設定します。たとえば、ユーザーがメディアを再生する前にログインを求める場合は、次のように`PlaybackActivity`で画面へのアクセスを検証します。

    ```json
      "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": true,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
    ```

4.  Login with Amazon画面 (図 1) に表示されるテキストを変更するには、**LoginWithAmazonComponent > res > values > strings.xml**の順に移動します。文字列をアプリの**custom.xml**ファイル (res > valuesにあります) にコピーし、文字列値をカスタマイズします。 

コンポーネントを詳細に構成するには、Login with AmazonサービスのAPIキーを挿入する必要があります。次のセクションでは、このAPIキーの取得方法について説明します。

## 手順 2. Developer Consoleでセキュリティプロフィールを作成する {#securityprofile}

Developer Consoleでアプリのセキュリティプロフィールを作成する必要があります。このセキュリティプロフィールはLogin with Amazonサービスで使用されるほか、APIキーを取得するためにも必要になります。

セキュリティプロフィールにはアプリについての情報が含まれ、プライバシーに関する同意画面 (図 2) でユーザーに表示されます。プライバシーに関する同意画面では、アプリケーションの名前、ロゴ、プライバシーポリシーへのリンクがユーザーに表示されます。

セキュリティプロフィールを設定するには、「[Register your Android app with Login with Amazon](https://developer.amazon.com/public/apis/engage/login-with-amazon/docs/register_android.html)」ページの「Register Your Security Profile」セクションに記載されている手順に従ってください。

## 手順 3.Login with Amazon APIキーを取得する {#apikey}

Developer Consoleでセキュリティプロフィールを作成した後は、そのプロフィールでLogin with Amazonを使用するアプリ用に設定を追加する必要があります。その後はAPIキーを取得して、アプリでのLogin with Amazonコンポーネントの設定を完了できます。

1.  「[Register your Android app with Login with Amazon](https://developer.amazon.com/public/apis/engage/login-with-amazon/docs/register_android.html)」ページの「Adding an Android App to a Security Profile」セクションに記載されている手順を最後まで実行します。以下の点に注意してください。

    *  パッケージ名については、Android Studioで**AndroidManifest.xml**ファイル (**app > manifests**にあります) を開いてください。ファイルの先頭にパッケージ名が表示されます。デフォルトのパッケージ名は**com.amazon.android.calypso**です。ただし、このパッケージ名は多くの場合アプリのカスタマイズ時に変更されています。
    * **Signature**フィールドのMD5 値を取得するには、以下の「[アプリの署名を取得する](#signature)」セクションを参照してください。

2.  「[Register your Android app with Login with Amazon](https://developer.amazon.com/public/apis/engage/login-with-amazon/docs/register_android.html#Android App Signatures and API Keys)」ページの「Retrieving an Android API Key」セクションに記載されている手順に従ってAPIキーを取得します。

3.  APIキーを取得したら、Android Studioの**LoginWithAmazonComponent > assets**フォルダーに移動して**api_key.txt**ファイルを開きます。ファイル内のデフォルトのテキストをすべて削除し、APIキーを貼り付けます。

### アプリの署名を取得する {#signature}

アプリでLogin with Amazonサービスを操作するためには、アプリに署名を行う必要があります。アプリをリリースするまで、署名はデバッグキーストアに格納されています。アプリをリリースすると、署名はリリースキーストアに格納されます。Login with Amazonサービスでは、アプリの署名を使用してLogin with Amazonコンポーネントの構成に必要なAPIキーを作成します。

アプリをテストしている場合は (つまり、まだAmazonアプリストアにリリースしていない場合は)、Android StudioでAPKファイルの署名が自動的に生成されます。コーディングやテストの段階では、この署名を使用してLogin with Amazonの機能をテストできます。アプリを正式にリリースするときは、リリースキーストアの署名をもとに新しいAPIキーを生成することが必要になります。

署名の詳細については、「[Android App Signatures and API Keys](https://developer.amazon.com/public/apis/engage/login-with-amazon/docs/android_app_signatures.html)」を参照してください。また、Androidドキュメントの「[アプリに署名する](https://developer.android.com/studio/publish/app-signing.html)」も併せて参照してください。

署名からMD5 値を取得するには、[keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)と呼ばれるJavaユーティリティを使用します。keytoolを使用して署名のMD5 値を取得するには:

1. keytoolをPATHに追加します。

   1.  keytoolへのパスを特定します。keytoolは、JDKの保存先のHome/binディレクトリ内にあります。たとえば、Macでのパスは **/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/bin**です(正確なパスはJDKのバージョンによって異なります。このパスはご利用のバージョンに基づいて更新してください)。
   2.  この場所をPATHに追加します。Macでは、次のようなコマンドを使用して追加できます。

       ```
       echo 'export PATH=$PATH:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/bin' >> ~/.bash_profile
       ```
        
        Windowsユーザーの場合は、WindowsでPATHにツールを追加する手順に従ってください。
        
2.  keytoolをPATHに追加したら、debug.keystoreの保存先フォルダーに移動します。このパスは次のようになります。

    ````
    /Users/<your username>/.android/debug.keystore
    ```

    `<your username>` は自分のユーザー名に置き換えてください。

3.  次のkeytoolコマンドを実行します。その際、`<alias>` は**androiddebugkey**、`<keystore.filename>` は**debug.keystore**に置き換えてください。

    ````
    keytool -list -v -alias <alias> -keystore <keystore.filename>
    ````

    パスワードは**android**です。

    このデバッグキーストアファイルに対応するエイリアスとパスワードは、Android Studioによって自動的に生成されます。

    以下のような証明書のフィンガープリントが表示されます。

    ```
    Alias name: androiddebugkey
    Creation date: Jun 13, 2016
    Entry type: PrivateKeyEntry
    Certificate chain length: 1
    Certificate[1]:
    Owner: C=US, O=Android, CN=Android Debug
    Issuer: C=US, O=Android, CN=Android Debug
    Serial number: 1
    Valid from: Mon Jun 13 22:56:54 PDT 2016 until: Wed Jun 06 22:56:54 PDT 2046
    Certificate fingerprints:
         MD5:  20:91:A5:45:ED:F7:D5:9A:03:65:33:66:AD:02:27:E8
         SHA1: B7:73:5F:AF:28:68:40:AB:31:59:03:A2:46:AB:D6:44:85:2A:C1:0E
         SHA256: 05:E3:7C:82:42:04:4A:0A:DC:98:6A:1C:B3:21:64:9F:AC:CD:3E:CD:B1:57:3F:EA:C0:35:0E:32:8D:39:D5:A6
         Signature algorithm name: SHA1withRSA
         Version: 1
    ```

4.  MD5 の値をコピーします。
5.  「[Login with Amazon APIキーを取得する](#apikey)」セクションの手順に従い、この値をSignatureフィールドに入力します。


{% include links.html %}
