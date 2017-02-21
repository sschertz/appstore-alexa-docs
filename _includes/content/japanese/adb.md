Android Debug Bridge (ADB) を使用すると、Amazon Fire TV端末またはAmazon Fire TV Stickに開発コンピューターを接続して、アプリのインストール、テスト、デバッグを行うことができます。

ADBでFire TVまたはFire TV Stickに接続する際は、あらかじめ以下のセクションをお読みください。

{% include note.html content="[Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html)は、Amazonではなく、Android Open Source Projectにより提供されます。"%}

* TOC
{:toc}

## 手順 1. Amazon Fire TVでデバッグを有効にする {#turnondebugging}

Fire TV端末に接続するには、端末でADBとデバッグの両方を有効にする必要があります。

1.  Fire TVのメイン画面で [**Settings**] を選択します。
2.  [**Device**] > [**Developer Options**] の順に選択します。
3.  [**ADB Debugging**] を有効にします。
4.  (省略可能) USBケーブルを使用してコンピューターをFire TV端末に接続する場合は、[**USB Debugging**] をオンにします。
    
    {% include note.html content="USBデバッグが有効になっているときは、USBポートを他の用途 (外部ストレージ、入力端末など) に使用することはできません。USBポートを再度有効にするには、USBデバッグをオフにしてください。"%}
    
5.  [**Apps from Unknown Sources**] を有効にします。

## 手順 2. Android Debug Bridgeを設定する {#setupadb}

Android Debug Bridge (ADB) は、ご利用の端末またはエミュレータでAndroidアプリの実行と管理を行うためのコマンドラインユーティリティです。ADBはAndroid Studioをインストールすると利用できますが、Windowsをご利用の場合は、特別なUSBドライバをインストールする必要があります。

### Mac OS X

Mac OS XでADBを使用する場合は、特に必要な作業はありません。

### Windows

Windowsが実行されているコンピューターをUSBケーブルでFire TVに接続する場合、ADBを介してFire TV端末にコンピューターを接続するためには特別なUSBドライバをインストールする必要があります。このドライバは、すべてのFire TVプラットフォームをサポートしています。ドライバをインストールするには:

1.  [USBファイルをダウンロード](https://s3.amazonaws.com/android-sdk-manager/redist/fire_devices_usb_driver.zip)してzipファイルの内容を抽出します。
2.  **FireDevices_Drivers**をダブルクリックします。
3.  ダイアログボックスの指示に従ってインストールを実行します。

{% include note.html content="このUSBドライバはWindows 8.1 に対してのみ認定されています。Windows 10 をご利用の場合は、\"信頼されていない発行元\"からのインストールに明示的に同意する必要があります。" %}

## 手順 3. 環境変数パスにAndroid Debug Bridgeを追加する {#addadbpath}

`adb`のコマンドを実行しやすくするために、環境変数PATHにADBを追加する必要があります。(PATH環境変数は、プログラムの実行ファイルの場所を指定するものです。ADBをPATHに追加しなかった場合、`adb`コマンドを実行するには、`<Android SDK>/platform-tools`ディレクトリに移動したうえで`adb`を実行する必要があります。)

### Mac OS X

Macで環境変数PATHにADBを追加するには:

1.  Android SDK platform-toolsディレクトリのパスを確認します。

    1.  Android Studioを起動して [**SDK Manager**] ボタン {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_androidsdkmanagericon" type="png" %} をクリックします。
    
        上部の [**Android SDK Location**] の横にAndroid SDKの場所が表示されます(例: `/Users/<your username>/Library/Android/sdk`)。
    
        Android Studioを初めて起動する場合は、[SDK Manager] ボタンが表示されません。[Welcome to Android Studio] というプロンプトで、[**Configure**] > [**SDK Manager**] の順にクリックし、Android SDKの場所を指定してください。
    
    2.  SDKのパスをコピーして、テキストエディターなどに貼り付けます。
    3.  直前の手順でコピーしたパスの末尾に**/platform-tools**を追加します。"platform-tools" は`adb`実行ファイルが格納されているディレクトリです。
    4.  フルパスをクリップボードにコピーします。 
    
2.  以下のコマンドを使用してadbを**.bash_profile**に追加します。`/Users/<your username>/Library/Android/sdk/platform-tools/` の部分は、Android SDKの実際のパスに置き換えてください。  

    ```bash
    echo 'export PATH=$PATH:/Users/<your username>/Library/Android/sdk/platform-tools/' >> ~/.bash_profile
    ```
    
    .bash_profileファイルは通常、ユーザーディレクトリにあります。ユーザーディレクトリは、「`cd ~`」(change directory) と入力して確認できます。次に、「`ls -a`」(list all) と入力して、非表示のファイルも含めたすべてのファイルを表示します。 
       
    .bash_profileファイルが存在しない場合は、新規に作成します。そのうえで「`open .bash_profile`」と入力すると、パスが一覧表示されます。各行は、`export PATH=$PATH:/Users/<your username>/Library/Android/sdk/platform-tools/` のようになっています。

3.  ターミナルセッションを再起動して「**adb**」と入力します。PATHにADBが正しく追加されると、ADBのヘルプ情報が表示されます。正しく追加されていないと、"command not found." と表示されます。


### Windows

Windowsで環境変数PATHにADBを追加するには:

1.  Android SDK platform-toolsディレクトリのパスを確認します。

    1.  Android Studioを起動して [**SDK Manager**] ボタン {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_androidsdkmanagericon" type="png" %} をクリックします。

        上部の [**Android SDK Location**] の横にAndroid SDKの場所が表示されます(例: `C:\Users\<your user name>\AppData\Local\Android\Sdk\platform-tools`)。

        Android Studioを初めて起動する場合は、[SDK Manager] ボタンが表示されません。[Welcome to Android Studio] というプロンプトで、[**Configure**] > [**SDK Manager**] の順にクリックし、Android SDKの場所を指定してください。

    2.  SDKのパスをコピーして、テキストエディターなどに貼り付けます。3.  直前の手順でコピーしたパスの末尾に**/platform-tools**を追加します。"platform-tools" は`adb`実行ファイルが格納されているディレクトリです。4.  フルパスをクリップボードにコピーします。
    
2.  [**スタート**] ボタンをクリックし、検索ボックスに「**システムの詳細設定の表示**」と入力します。
3.  [**システムの詳細設定の表示**] をクリックします。
4.  [システム設定] ダイアログが表示されたら、[**環境変数**] ボタンをクリックします。
5.  [*システム環境変数*] (下のペイン) で [**Path**] を選択し、[**編集**] をクリックします。
6.  次のいずれかの作業を行います。
    
    * *Windows 7 または 8* の場合: カーソルを右端に移動し、「`;`」と入力して**Ctrl+V**キーを押し、先ほどコピーしたSDKのパスを挿入します(例: `;C:\Users\<your user name>\AppData\Local\Android\Sdk\platform-tools`)。開いている 3 つのダイアログボックスをそれぞれ [**OK**] ボタンで閉じます。
    * *Windows 10* の場合: [**新規**] ボタンをクリックしてこの場所を追加します。  
    
8.  ターミナルセッションを再起動して「**adb**」と入力します。PATHにADBが正しく追加されると、ADBのヘルプ情報が表示されます。正しく追加されていないと、"command not found." と表示されます。

## 手順 4: ADBの接続方法 {#connectingadboptions}

ADBを使用してFire TVまたはFire TV Stickをコンピューターに接続するには、次の 2 つの方法を利用できます。

*   [ネットワーク経由でADBを接続する](#networkconnect)。有線イーサネットまたはWiFiネットワーク接続を使用した接続方法です。ネットワークADB接続が機能するには、コンピューターとFire TV端末が同じネットワーク上に存在する必要があります。*   [USB経由でADBを接続する](#usbconnect)。USBケーブル (A-to-A) を使用して直接USB接続を確立する方法です。

{% include note.html content="以降の手順は、ユーザーインターフェースが刷新された第 2 世代の端末を想定して書かれています。第 1 世代の端末をご使用の場合は、メニューの場所が若干異なります。"%} 

### ネットワーク経由でADBを接続する {#networkconnect}

Fire TV端末にADBを接続するには、ネットワークにあるFire TV端末のIPアドレスが必要となります。

1.  まだ接続していない場合は、Fire TV端末をネットワークに接続します (対象のコンピューターと同じネットワークに接続すること)。Fire TVのホーム画面で [**Settings**] > [**Network**] の順に選択して目的のネットワークを選択してください。2.  Fire TVのホーム画面で [**Settings**] を選択します。
3.  [**Device**] > [**About**] > [**Network**] の順に移動します。画面に表示されるIPアドレスをメモします。
    
    {% include tip.html content="日常的にネットワーク経由で接続する場合は、いつでも確認できるようにこのIPアドレスをコピーしておいてください。" %}
    
4.  ターミナルウィンドウを開きます。
    
    Macの場合、ターミナルを開くには、**Cmd + Spaceキー**を押して「**Terminal**」と入力します。Windowsの場合は、通常、プログラムの検索画面で「**cmd**」と入力してコマンドプロンプトを開きます(厳密な手順は、Windowsのバージョンによって異なります)。
    
5.  Fire TV端末とコンピューターが同じネットワーク上にあることを確認します。WiFiネットワークと有線ネットワークのどちらも使用できます。
6.  次のコマンドを実行します。`<ipaddress>` の部分には、前のセクションでメモしたFire TV端末のIPアドレスを指定してください。

    ```
    adb kill-server
    adb start-server
    adb connect <ipaddress>
    ```

    {% include note.html content="ADBが環境変数PATHに追加されていることを確認してください (「[環境変数パスにAndroid Debug Bridgeを追加する](#addadbpath)」を参照)。追加されていない場合は、「`cd`」 (change directories) でplatform-toolsディレクトリに移動してから、「`./adb`」 (Macの場合) または「`adb`」 (Windowsの場合) を使用して`adb`のコマンドを実行する必要があります。"%}

    接続に成功すると、ADBから次のようなメッセージが返されます。

    ```
    connected to <ipaddress>:5555
    ```

7.  次のコマンドを実行して、端末のリストにFire TV端末が表示されることを確認します。

    ```
    adb devices
    ```

    ADBから次のメッセージが返されます。

    ```
    List of devices attached
    <ipaddress>:5555  device
    ````

`adb devices`を実行してもシリアル番号が表示されない場合は、[ADBを更新](#updateadb)してから、再度この手順を実行してください。

{% include tip.html content="必ずしもADBサーバーを停止 (kill) して起動 (start) する必要はありません。通常は単に`adb connect <ipaddress>`コマンドを実行してください。"%}

### USB経由でADBを接続する {#usbconnect}

コンピューターをUSBでFire TVに接続するには、A-to-AタイプのUSBケーブルが必要となります。利用できるのはFire TVのみで、Fire TV Stickでは利用できないためご注意ください。USBケーブルポートはFire TV (ボックス) にしか存在しません。

1.  Windowsコンピューターを使用する場合は、USBドライバをインストールします (「[Android Debug Bridgeを設定する](#setupadb)」を参照)。
2.  コンピューターのUSBポートにFire TVを接続します。
2.  次のコマンドを実行します。

    ```
    adb kill-server
    adb start-server
    adb devices
    ```

最後のコマンドを入力すると、ADBから次のメッセージが返されます。`<serialno>` は端末のシリアル番号です。

```bash
List of devices attached
<serialno> device
```

`adb devices`を実行してもシリアル番号が表示されない場合は、[ADBを更新](#updateadb)してから、再度この手順を実行してください。

コンピューターとFire TV端末の接続がADBによって確立された後、開発中のアプリを実行すると、次のようなダイアログボックスが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_aftsoptions" type="png" max-width="60%" border="true" %}

この例では、"Amazon AFTS" はFire TV (第 2 世代) を表しています。


## ADBを更新する {#updateadb}

「`adb devices`」と入力してもご利用の端末が表示されない場合は、次の手順に従ってADBを更新してみてください。

1.  ターミナルセッションを開いて、Android SDKの**tools**ディレクトリに移動します。

    インストールディレクトリは、Android Studioで [**Tools**] > [**Android**] > [**SDK Manager**] の順に選択するか、[**SDK Manager**] ボタン {% include inline_image.html file="firetv/getting_started/images/androidsdkmanagericon" type="png" %} をクリックして確認できます。[**Android SDK Location**] の横にディレクトリが表示されます。

2.  ターミナルまたはコマンドプロンプトから`cd`コマンドでAndroid SDKディレクトリに移動し、1 階層下の**tools**に移動 (`cd`) します (toolsディレクトリはSDKディレクトリに格納されています)。

2.  次のいずれかのコマンドを実行してADBを更新します。

    **Mac**:

    ```
    ./android update adb
    ```

    **Windows**:

    ```
    android update adb
    ```

    ADBが更新されたことを示すメッセージが返されます。

## トラブルシューティング

 次のようなメッセージが表示される場合があります。

 ```
 unable to connect to 192.168.0.6:5555: Operation timed out
 ```

この場合は、次のことを試してください。

* Fire TVとコンピューターで、どちらも同じネットワークとルーターが使用されていることを確認します。
* [ADBを更新](#updateadb)し、サーバーを停止してから再起動します。
* Fire TVとルーターを再起動します。
* IPアドレスがドット (`.`) も含め、正しく入力されていることを確認します。
