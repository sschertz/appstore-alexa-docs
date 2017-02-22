---
title: アプリのインストールと実行
permalink: installing-and-running-your-app.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Amazonアプリストアに申請するFire TVアプリは、事前にテストとデバッグを済ませる必要があります。この作業にはAndroid Debug Bridge (ADB) を使用し、Fire TV端末にアプリをインストールして実行します。開発したアプリを (アプリストアを介さずに) インストールすることを、アプリを "サイドロードする" とも
言います。 

作業を行う前に、Fire TV端末がADB経由で開発コンピューターにあらかじめ接続されている必要があります。詳細については、「[ADBを使用してFire TVに接続する][connecting-adb-to-fire-tv-device]」を参照してください。

{% include note.html content="このページで参照されている一部の開発ツールやリソースは、Amazonではなく、サードパーティによって提供されています。これらのツールやリソースへのリンクは、サードパーティのサイトを指しています。" %}

* TOC
{:toc}

## アプリをインストールする (コマンドライン)

コマンドラインからアプリをFire TV端末にインストールするには、次のコマンドを使用します。ここで、`<path-to-apk-file>` はアプリのAPKへのファイルシステムパスです。

```
adb install <path-to-apk-file>
```

インストールが正常に終了すると、ADBによって次のようなメッセージが表示されます。

```
764 KB/s (217246 bytes in 0.277s)
pkg: /data/local/tmp/HelloWorld.apk
Success
```

デバイスにすでに存在するアプリを再インストールするには、次のコマンドのように、`-r`オプションを指定してアプリを再インストールします。

```
adb install -r <path-to-apk-file>
```

アプリを再インストールしても、既存のユーザーデータやキャッシュは上書きされません。このデータをクリアするには、古いアプリをアンインストールしてから新しいバージョンをインストールするか、[**System**] > [**Applications**] で手動でデータをクリアします。

## アプリを実行する (端末)

サイドロードされたアプリは、[Apps] セクションの [Recent] 行と [My Library] 行に表示されます。また、[Settings] メニューにもアプリが表示されます。

1.  Fire TVのメイン画面から、[**Settings**] > [**Applications**] > [**Manage Installed Applications**] の順に選択します。
2. アプリを選択します。
3.  [**Launch application**] を選択します。

{% include note.html content="第 1 世代の端末をご使用の場合、一部のメニューが若干異なる場合があります。" %}

## アプリを実行する (コマンドライン)

Amazon Fire TV端末のアプリに起動インテントを送信するには、次のコマンドを使用します。ここで、`com.amazon.sample.helloworld`はアプリのパッケージ名で、`MainActivity`はアプリのプライマリアクティビティの名前です。

```
adb shell am start -n com.amazon.sample.helloworld/.MainActivity
```

ADBから次のようなメッセージが返され、アプリが実行を開始します。

```
Starting: Intent { cmp=com.amazon.sample.helloworld/.MainActivity }
```

## アプリをアンインストールする (端末)

Amazon Fire TV端末自体を使用してアプリを端末からアンインストールするには、次の手順に従います。

1.  Fire TVのメイン画面から、[**Settings**] > [**Applications**] > [**Manage Installed Applications**] の順に選択します。
2. アプリを選択します。
3.  [**Uninstall**] > [**Uninstall**] を選択します。


## アプリをアンインストールする (コマンドライン)

コマンドラインからアプリをアンインストールするには、APKのパッケージ名が必要です。次のコマンドを使用してアプリをアンインストールします。ここで、`com.amazon.sample.helloworld`はアプリのパッケージです。

```
adb uninstall com.amazon.sample.helloworld
```

アプリのパッケージ名が不明な場合は、次のコマンドを使用すると、インストール済みのすべてのAPKとその
パッケージ名が一覧表示されます。

```
adb shell pm list packages -f
```

{% include links.html %}
