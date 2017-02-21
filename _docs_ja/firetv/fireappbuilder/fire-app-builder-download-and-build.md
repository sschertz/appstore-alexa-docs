---
title: Fire App Builderをダウンロードして、アプリをビルドする
permalink: fire-app-builder-download-and-build.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Fire App Builderを使用してサンプルアプリをビルドするには、以下のセクションの手順を完了します。

* TOC
{:toc}

## 1. Fire App Builderプロジェクトのクローンを作成する {#clone}

Fire App Builderは、Githubリポジトリ ([https://github.com/amzn/fire-app-builder](https://github.com/amzn/fire-app-builder)) でダウンロードするか、クローンを作成できます。リポジトリのクローンを作成すると、更新の提供が開始されたときにリポジトリからその更新を入手できます。

リポジトリのクローンを作成するには:

1.  まだ[git](https://git-scm.com/downloads)をインストールしていない場合は、入手します。
2.  Windowsを使用している場合は、次のようにシンボリックリンク (symlink) を許可するようgitを構成します。

    ```bash
    git config –global core.symlinks true
    ```

    {% include note.html content="Windowsでは、gitのシンボリックリンクが`true`に構成されていない状態でリポジトリのクローンを作成した場合、Fire App Builderではビルドされません。" %}

3.  [https://github.com/amzn/fire-app-builder](https://github.com/amzn/fire-app-builder)にアクセスします。
4.  [**Clone or Download**] をクリックし、クローンのURL `https://github.com/amzn/fire-app-builder.git`をコピーします。
5.  コマンドラインを開き、プロジェクトに便利なディレクトリを参照します。
6.  次のようにプロジェクトのクローンを作成します。

    ```
    git clone https://github.com/amzn/fire-app-builder.git
    ```

    gitはfire-app-builderという名前のディレクトリで初期化され、Fire App Builderプロジェクトがダウンロードされます。

    {% include tip.html content="既存の (空の) フォルダーにリポジトリのクローンを作成する場合は、最初にコマンドラインでそのフォルダーを参照してから、`git clone https://github.com/amzn/fire-app-builder.git`を実行して既存のフォルダーにリポジトリをコピーします。または、`git clone https://github.com/amzn/fire-app-builder.git myspecialfolder`を実行して、`myspecialfolder`という名前のフォルダーにプロジェクトをダウンロードします。" %}

## 2. JDKをセットアップする {#jdk}

ご使用のコンピューターでJavaアプリをコンパイルするには、Oracleから提供されているJava Development Kit (JDK) が必要です。

まず、JDKがすでにインストールされているかどうかを確認します。ターミナルまたはコマンドプロンプトを開いて「`java -version`」と入力します。JDKがインストールされている場合、次のようなメッセージが返されます。

```
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
```

Windowsでは、手動で`C:\Program Files\Java\jdk1.8.0\`などのディレクトリを調べて、そこにJDKがインストールされているかどうかを確認することもできます。

JDKがインストールされていない場合は、ご使用のコンピューターに合ったバージョンのJDKインストーラを[Java SE Development Kit Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)からダウンロードして実行してください。

詳細については、以下のページを参照してください。

* **Mac**: [10 JDK 8 Installation for OS X](https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#CHDBADCG)
* **Windows**: [JDK Installation for Microsoft Windows](https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html) (このページの「Running the JDK Installer」と「Updating the PATH Environment Variable」のセクションをご覧ください)。
* その他のオペレーティングシステムと情報については、「[JDK 8 and JRE 8 Installation Start Here](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)」を参照してください。

## 3. Android Studioおよび必要なツールとSDKをインストールする {#androidstudio}

Fire App Builderを使用するには、Androidプロジェクトの公式IDEである[Android Studio](https://developer.android.com/studio/index.html)をインストールする必要があります。

ご使用のコンピューターへのAndroid Studio開発環境のセットアップについては、[Android Studioの概要に関するページ](https://developer.android.com/sdk/installing/studio.html)と「[Android Studioのインストール](https://developer.android.com/sdk/installing/index.html)」を参照してください。

Fire App Builderプロジェクトでは、特定のSDKツールとAPIをAndroid Studioにインストールしておく必要があります。スタンドアロンのSDK Managerからこれらのツールを選択する必要があるわけではありません。Fire App Builderプロジェクトを開いたときに (次のセクション「[サンプルアプリを開く](#openFireAppBuilder)」を参照)、Android Studioでは、不足しているビルドツールやAPIをインストールするよう求められます。たとえば、次のようなメッセージが表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_sdktoolsneeded" max-width="70%" type="png" border="true" %}

または、Gradle Consoleでは、次のように表示されることがあります。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_androidprompts"  max-width="70%" type="png" %}

これらのリンクをクリックすると、不足しているツールがインストールされ、プロジェクトの再ビルドが試行されます。Android Studioでメッセージが表示されなくなるまで、引き続き、プロジェクトを開き、指示に従って不足しているツールをインストールすることができます。

ただし、前もって、スタンドアロンのSDK Managerを開き、すべてのツールとSDKの要件をインストールすることもできます。必要なツールをインストールするには:

1.  [**Tools**] > [**Android**] > [**SDK Manager**] の順に移動します (または [**SDK Manager**] {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_androidsdkmanagericon" type="png" %} ボタンをクリックします)。
2.  表示された [Preferences] ダイアログボックスで、[**Launch Standalone SDK Manager**] リンクをクリックします。
3.  少なくとも以下のツール、API、およびSDKが選択され、インストールされていることを確認します。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_standalonesdkinstalls" type="png" max-width="75%" %}

4.  チェックボックスをオンにした後、[**Install packages...**] ボタンをクリックします。


## 4. サンプルアプリを開く {#openFireAppBuilder}

{% include warning.html content="Android Studioで初めてアプリをダウンロードしてビルドする場合は約 30～40 分かかります。" %}

この手順では、Android StudioでFire App Builderから "Application" プロジェクトを開き、アプリをビルドします。

1.  **Android Studio**を起動します。
2.  ようこそ画面で、[**Open an existing Android Studio project**] をクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_androidstudiowelcome" type="png" max-width="75%" %}

    このようこそ画面が表示されない場合は、JDKまたは任意のAndroid SDKを使用してAndroid Studioを構成していない可能性があります。

3.  Fire App Builderプロジェクトフォルダーで、**Application**フォルダーを選択し、[**OK**] をクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_androidstudioproject" type="png" max-width="75%" %}

    Gradleの最新リリースによっては、Gradleを更新するよう求められる場合があります。求められた場合は、[**Don't remind me again for this project**] をクリックします。

    Gradleによってプロジェクトのインポートとビルドが開始されます。

    {% include note.html content="初めてプロジェクトを開くと、Gradleがプロジェクトをビルドするまでに **20~40 分**程度かかる場合があります。これは、Gradleがダウンロードする必要があるアセットと、ネットワークやプロセッサの速度によって異なります。その後は、1 分もかからなくなります。待っている間、アプリについてよく知るために[アプリの概要を確認すること][fire-app-builder-app-tour]を検討してください。さらに、Fire TV端末の [Settings] オプションを確認することも検討してください。" %}

4.  Gradleによるビルドの進捗状況を監視できるように、右下隅にある [**Gradle Console**] をクリックしてGradle Consoleを開きます。これにより、ビルドが成功したかどうか、またはAndroid Studioで他のダウンロードが必要かどうかがわかります。Gradleによるビルドが完了するまで待ちます。

    Android Studioで不足しているライブラリやSDKが強調表示されたら、指示に従ってダウンロードし、問題を修正します。

    完了すると、`BUILD SUCCESSFUL`というメッセージがGradle Consoleに表示されます。Gradleがプロジェクトのビルドを完了すると、Android Studioでは、*Androidビューに*次のディレクトリが表示されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_androidstudiosampledir" max-width="70%" type="png" alt="Android Studioのディレクトリ" %}

    Androidでプロジェクトを開くと、デフォルトで[Androidビュー](http://developer.android.com/sdk/installing/studio-androidview.html)が表示され (前出のスクリーンショットでは赤い丸で囲まれています)、"Application" フォルダーは単に "app" と表示されています。Androidビューでは、プロジェクトのファイルがフラット化され、最もよく使用されるファイルがより使いやすい位置に表示されますが、お使いのコンピューターディスク上の実際のファイル構造は異なります ("Project" ビューには、すべてのフォルダーとファイルの実際の配置が表示されます)。

    {% include note.html content="特に指定のない限り、このドキュメントでは**Androidビュー**を使用してファイルの場所を指します。" %}

5.  「[ADBを使用してFire TVに接続する][fire-app-builder-connecting-adb-to-fire-tv]」の手順に従って、Fire TV端末にコンピューターを接続します。
6.  [**Run 'app'**] ボタン {% include inline_image.html alt="[Run 'app'] ボタン" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %} をクリックします。
7.  [Select Deployment Target] ダイアログボックスで、[**Amazon**] を選択し、[**OK**] をクリックします。

    {% include image.html max-width="75%" alt="エミュレーターまたは端末の選択" file="firetv/fireappbuilder/images/fireappbuilder_aftsoptions" type="png" %}

    [Select Deployment Target] ダイアログボックスのAFTSはAmazon Fire TV (第 2 世代) を表しています。Fire TV Stickの場合は、AFTT (第 2 世代) またはAFTM (第 1 世代) と表示されます。

    {% include tip.html content="Amazonのオプションが表示されない場合は、[コンピューターがADBを使用してFire TV端末に接続][fire-app-builder-connecting-adb-to-fire-tv]されていません。Fire TVがない場合は、[エミュレーターの使用][fire-app-builder-use-an-android-tv-emulator]についてサポートが制限されます。" %}

    アプリが正常にビルドされると、Fire TV端末にそのアプリがロードされます。アプリの各画面の説明については、「[アプリの概要][fire-app-builder-app-tour]」を参照してください。

    ビルドエラーが発生した場合は、プロジェクトをクリーンし、再ビルドしてみることができます ([**Build**] > [**Clean Project or Build**] > [**Rebuild Project**])。

    Fire TVでアプリを閉じる場合は、リモコンを使用して [**Settings**] > [**Applications**] > [**Manage Installed Applications**] > [**Fire App Builder**] の順に移動すると、Fire TVのUIを使用して再起動できます。

    ADBは、接続された端末の*一時*フォルダーでアプリをビルドします。端末の接続を切断すると、アプリはFire TVで使用できなくなります。Fire TVにアプリを永続的にインストールする場合は、端末にアプリをサイドロードする必要があります。「[アプリのインストールと実行][installing-and-running-your-app]」を参照してください。  

## 5. Fire App Builderサンプルプロジェクトをカスタマイズする {#customize}

アプリを作成する最初の手順として、"Application" フォルダーをカスタマイズします。Applicationフォルダーには、Fire App Builderで作成されたサンプルアプリが含まれています。Applicationフォルダーをカスタマイズするためのオプションは 2 つあります。選択するオプションは、Githubリポジトリから取得する新しい更新を処理する方法によって異なります。

*  **オプション 1: Applicationフォルダーとそのファイルを複製する**: `git pull`を実行してFire App Builderリポジトリから新しい更新を取得するときに、アプリケーションのファイルに考えられる更新についてマージの競合を解決する必要はありません。ただし、Applicationフォルダーとその内容に対する更新がある場合は、更新をプロジェクトにマージするよう求められることはありません。また、他のフォルダーにあるコードがApplicationフォルダーの更新を必要とする場合は、更新しないとプロジェクトが壊れる可能性があります。
*  **オプション 2: Applicationフォルダーとそのファイルを直接カスタマイズする**: `git pull`を実行してFire App Builderリポジトリから新しい更新を取得するときに、マージの競合が表示されるため、それを調べる必要があります。更新の取得はより面倒な作業になり、gitリポジトリとのマージの競合を解決する方法を理解することが必要になります。ただし、Applicationフォルダーとそのファイルに対する更新を見逃すことはありません。

Applicationフォルダーをカスタマイズするには:

1.  Android Studioで、[**File**] > [**Close Project**] の順に移動して、Fire App Builderプロジェクトを閉じます。
2.  Finder (Mac) またはエクスプローラー (Windows) を使用して、Fire App Builderをダウンロードしたディレクトリを参照します。次のいずれかの操作を行います。
     * Applicationフォルダーをコピーします (オプション 1)。次に、そのコピーの名前を変更します (「Gizmoapp」など)。
     * Applicationフォルダーの名前を直接変更します (オプション 2)(「Gizmoapp」など)。
3.  カスタマイズしたApplicationフォルダー ("Gizmoapp") 内で、(生成されたアプリが保存される) **build**フォルダーを削除します。
4.  Android Studioで、表示されたようこそ画面の [**Open an existing Android Studio project**] をクリックします。Fire App Builderフォルダーで、カスタマイズしたApplicationフォルダー ("Gizmoapp") を選択し、[**OK**] をクリックします。
5.  Gradleプラグイン設定のアップグレードに関する画面が表示されたら、[**Don't remind me again for this project**] をクリックします。
6.  左上隅にある [**Project**] ペイン (Projectビューではありません) を展開して、ファイルを表示します。
7.  **app**フォルダーで、**res** > **values**の順に展開し、**strings.xml**ファイルを開きます。
8.  **app_name**の文字列に、アプリの名前を入力します。

    AndroidManifest.xmlファイル (**app** > **manifests**にあります) は、編集した文字列からアプリケーションの名前を読み取ります (Androidマニフェストで、この文字列を参照するコードは`android:label="@string/app_name"`です。このようにして、Fire App Builderの大部分が設定されます。カスタマイズするコードはXMLファイルに抽出されるため、Javaを直接編集する必要はありません)。

9.  アプリの**AndroidManifest.xml**ファイル (**app** > **manifests**にあります) を開き、**com.amazon.android.calypso**というパッケージ名を、アプリの新しい名前を反映させたパッケージ名に更新します。たとえば、アプリの名前が "Gizmoapp" となった場合、次のように変更できます。

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.amazon.android.gizmoapp">
    ```

    Fire App Builderのサンプルアプリの`com.amazon.android.calypso`パッケージにはクラスがありませんが、カスタムクラスを追加して、既存のFire App Builder機能を上書きしたり追加したりできます。

10. [**Build**] > [**Clean Project**] の順に移動して、以前のアプリからすべてのアーティファクトをクリーンアップします (この処理には数分かかります)。
11. [**Run 'app'**] ボタン {% include inline_image.html alt="[Run 'app'] ボタン" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %} をクリックして、新しいアプリをビルドします。

## 次のステップ

Fire App Builderでデフォルトのアプリをビルドしてカスタマイズしたところで、「[アプリの概要][fire-app-builder-app-tour]」を参照して、Fire App Builderの機能について理解を深めてください。

{% include links.html %}
