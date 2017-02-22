---
title: 開発環境のセットアップ
permalink: setting-up-your-development-environment.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Amazon Fire TVプラットフォームを対象にAndroidベースのFire TV対応アプリ開発を行うには、あらかじめ開発環境をセットアップしておく必要があります。

* TOC
{:toc}

## JDKのセットアップ

ご使用のコンピューターでJavaアプリをコンパイルするには、Oracleから提供されているJava Development Kit (JDK) が必要です。 

まず、JDKがすでにインストールされているかどうかを確認します。ターミナルまたはコマンドプロンプトを開いて「`java -version`」と入力します。JDKがインストールされている場合、次のようなメッセージが返されます。

```
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
```

JDKがインストールされていない場合は、ご使用のコンピューターに合ったバージョンのJDKインストーラを[Java SE Development Kit Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)からダウンロードして実行してください。 

詳細については、以下のページを参照してください。

* [10 JDK 8 Installation for OS X](https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html)
* [JDK Installation for Microsoft Windows](https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html)。このページの「Running the JDK Installer」と「Updating the PATH Environment Variable」のセクションをご覧ください。

その他のオペレーティングシステムと情報については、「[JDK 8 and JRE 8 Installation Start Here](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)」を参照してください。

## Android Studioのセットアップ {#setupandroidstudio}

Androidプロジェクトの公式IDEである[Android Studio](https://developer.android.com/studio/index.html)をインストールします。ご使用のコンピューターへのAndroid Studio開発環境のセットアップについては、[Android Studioの概要に関するページ](https://developer.android.com/sdk/installing/studio.html)と「[Android Studioのインストール](https://developer.android.com/sdk/installing/index.html)」を参照してください。

## 次のステップ

ADBを使用してFire TV端末を開発コンピューターに接続する方法については、「[ADBを使用してFire TVに接続する][connecting-adb-to-fire-tv-device]」を参照してください。

開発したアプリをFire TV端末にインストールして実行する方法については、「[アプリのインストールと実行][installing-and-running-your-app]」を参照してください。

{% include links.html %}
