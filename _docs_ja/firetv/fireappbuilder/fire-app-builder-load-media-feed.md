---
title: メディアフィードをロードする
permalink: fire-app-builder-load-media-feed.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

メディアフィードはアプリの核となる部分です。このフィードは、各メディアオブジェクトのタイトル、説明、サムネイル、その他の詳細情報が含まれたビデオコンテンツで構成されています。 

メディアフィードごとに構造 (および各種プロパティや要素を指す用語) が異なるため、Fire App Builderでは、メディアフィードに対して必要なコンポーネントを問い合わせ、その結果をFire App Builderのコンテンツモデルに合致した構造と用語に変換します。その問い合わせ用のクエリを記述する ([カテゴリ][fire-app-builder-set-up-recipes-categories]レシピと[コンテンツ][fire-app-builder-set-up-recipes-content]レシピでクエリを指定する) には、まず、ここで説明する手順に従ってメディアフィードをロードする必要があります。

メディアフィードのロードと構成には、DataLoaderとDynamicParserという 2 つのFire App Builderコンポーネントを使用します。Fire App Builderで読み取れるように、DataLoaderモジュールでデータフィードをロードし、DynamicParserでフィードの構造とキー値を構成します。Javaで操作するのではなく、JSONファイルを通じてこれら両方のモジュールを構成します。Javaコードでは、各種JSONファイルの値を読み取ります。

* TOC
{:toc}

## フィードの種類

以下に挙げる種類のフィードをロードすることができます。

* [トークンベースのフィード](#tokenbasedconfiguration)
* [オープンフィード](#filebasedconfiguration)

### トークンベースのフィードをロードする {#tokenbasedconfiguration}

トークンによってアクセスが制限されているウェブフィードでメディアの詳細を公開する場合は、以下の手順に従います。 

1.  **DataLoadManagerConfig.json**ファイル (**app > assets > configurations**にあります) を開きます。
    
    {% include tip.html content="Android Studioでは、フォルダーを参照する代わりに**Shift**キーを 2 回押してからファイル名を入力すると、すばやくファイルを検索できます。" %} 
    
2.  **data_downloader.impl**オプションの値は`com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader`のままにしておきます。
2.  必要に応じて、以下の 2 つのプロパティのオプションを更新します。
    * `is_cache_manager_enabled`: フィードをアプリでキャッシュするかどうかを指定します。フィードをキャッシュすると、取得したメディアによる画面のロード時間は短縮しますが、フィードに対する最新の更新は、データローダーが更新されるかフィードの有効期限が切れるまで反映されません。選択肢は`true`または`false`です。通常は`true`のままにしておきます。
    * `data_updater.duration`: データローダーがフィードを更新して最新の更新データを取得する間隔 (秒単位) を指定します。データローダーで更新が行われると、キャッシュは消去されます。デフォルト値は 2000 秒または 5.5 時間です。
    
    {% include note.html content="アプリのキャッシュは、ユーザーがアプリを起動するたびに自動的に消去され、フィードが更新されます。" %}
    
3.  **BasicHttpBasedDownloaderConfig.json**ファイル (**app > assets > configurations**にあります) を開き、値を`com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator`から`com.amazon.dataloader.datadownloader.BasicTokenBasedUrlGenerator`に変更します。

    ```json
    {
      "url_generator_impl": "com.amazon.dataloader.datadownloader.BasicTokenBasedUrlGenerator"
    }
    ```
    
4.  **app > assets > configurations**に**BasicTokenBasedUrlGeneratorConfig.json**というファイルを作成します。ファイル内に、次のような 2 つのキーと値のペアを含むJSONオブジェクトを作成します。

    ```json
    {
      "base_url" : "http://yourcompany.com/mediafeed?id=$$token$$",
      "token_generation_url" : "http://yourcompany.com/url_to_generate_token"
    }
    ```
        
5.  会社で使用している実際の値で`base_url`と`token_generation_url`の両方の値をカスタマイズします。

    `base_url`はメディアフィードへのURLです。`token_generation_url`には、このURLにアクセスするためのトークンを生成するURLへのリンクを含めます。

    この例では、`$$token$$` を使用してトークンをURLに挿入しています(メディアURLには他の方法でトークンを挿入することもできます。その場合は、`$$token$$` を適切な位置に調整してください)。

    このBasicTokenBasedUrlGeneratorConfig.jsonファイルのパラメータ (`base_url`と`token_generation_url`) は、`BasicTokenBasedUrlGenerator`クラス (**DataLoader > java > com.amazon.dataloader**にあります) に渡されます。`BasicTokenBasedUrlGenerator`クラスでURLが作成され、そのURLが`BasicHttpBasedDataDownloader`クラスで使用されます。`BasicHttpBasedDataDownloader`クラスでは、このURLからコンテンツを取得します。

### オープンフィードをロードする {#filebasedconfiguration}

オープンで無制限なウェブフィード、つまりメディアへのアクセスにトークンを必要としないウェブフィードでメディアの詳細を公開する場合は、以下の手順に従います。 

1.  **DataLoadManagerConfig.json**ファイル (**app > assets > configurations**にあります) を開きます。
    
    {% include tip.html content="Android Studioでは、フォルダーを参照する代わりに**Shift**キーを 2 回押してからファイル名を入力すると、すばやくファイルを検索できます。"%} 
    
2.  **data_downloader.impl**オプションの値が`com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader`であることを確認します。
    
    ```json
    {
      "data_downloader.impl": "com.amazon.dataloader.datadownloader.BasicHttpBasedDataDownloader",
      "is_cache_manager_enabled": true,
      "data_updater.duration": 14400
    }
    ```
    
2.  **BasicHttpBasedDownloaderConfig.json**ファイル (**app > assets > configurations**にあります) を開き、`url_generator_impl`の値が`com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator`であることを確認します。
    
    ```json
    {
      "url_generator_impl" : "com.amazon.dataloader.datadownloader.BasicFileBasedUrlGenerator"
    }
    ```
    
3.  **BasicFileBasedUrlGeneratorConfig.json**ファイル (**app > assets > configurations**にあります) を開き、その内容が以下のコードと一致することを確認します。このファイルは、メディアフィードが格納される`url_file`の場所を指定しています。簡略化のために、ファイル名はデフォルトのままにします。

    ```json
    {
      "url_file" : "urlFile.json"
    }
    ```
    
6.  **urlFile.json** (**app > assets**にあります) を開き、メディアフィードのURLをリストに加えます。
    
    ```json
    {
      "urls": [
        "http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=249&app_key=gtn89uj3dsw&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=267&app_key=6tgbfr4edc2x&action=channels_videos",
        "http://www.lightcast.com/api/firetv/channels.php?app_id=273&app_key=u8jnsaq2rfgy&action=channels_videos"
      ]
    }
    ```
    
## その他のフィードをロードする方法

ここで紹介したどの方法でもフィードをロードできない場合は、Dataloaderインターフェースを実装するクラスをDataLoaderフォルダーに追加することで、独自のデータローダーを作成できます。また、フィードがRESTエンドポイントから生成されている場合は、データダウンローダーも自分で作成する必要があります。

## ライブフィード

ライブテレビフィードのロードに特別な操作は必要ありません。他のメディアと同じ方法でロードすることができます。ただし、Navigator.jsonでオプションを設定する場合は、Fire App Builderにライブストリーミングの [Watch from Beginning] ボタンを削除するよう指示するパラメータを追加できます。また、コンテンツレシピの設定時には、タグを使用してライブメディアを特定することもできます。詳細については、「[Navigatorの構成の概要][fire-app-builder-configure-navigator]」を参照してください。

## アプリにパッケージ化された静的フィードをロードする

テスト目的で、アプリ内にパッケージ化された静的フィードをロードすることができます。新しいバージョンのアプリを再申請しない限りフィードを更新することはできないため、このような操作は通常は行いません。この操作はテストのみを目的として行います。

アプリ内にパッケージ化されている静的フィードをロードするには:

1.  **DataLoadManagerConfig.json**ファイル (**app > assets > configurations**にあります) を開きます。
2.  **data_downloader.impl**の値を`com.amazon.dataloader.datadownloader.BasicFileBasedDownloaderConfig`に変更します。
3.  **BasicFileBasedDownloaderConfig.json**ファイル (**app > assets > configurations**にあります) を開きます。
4.  XMLファイルの名前は、次のように必要に応じて変更できます。
    
    ```xml
    {
      "data_file_path": "GenericMediaData.xml"
    }
    ```
    
5.  フィードファイルを**app > assets**フォルダー内に配置します。

## 次のステップ

フィードのロードは最初のステップにすぎません。Fire App Builderでフィード内のカテゴリとコンテンツを特定できるようにする必要があります。詳細については、「[レシピ構成の概要][fire-app-builder-set-up-recipes-overview]」を参照してください。

{% include links.html %}
