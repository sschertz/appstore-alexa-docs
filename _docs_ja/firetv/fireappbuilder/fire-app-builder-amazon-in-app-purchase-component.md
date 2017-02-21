---
title: Amazonアプリ内課金のコンポーネント
permalink: fire-app-builder-amazon-in-app-purchase-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Amazonアプリ内課金コンポーネントでは、Amazonの[アプリ内課金](https://developer.amazon.com/public/apis/earn/in-app-purchasing) (IAP) APIを使用して、アプリに次の 2 つの購入オプションを組み込みます。

* **Daily Pass**: ユーザーは 1 日間 (購入時から 24 時間) アプリ内のメディアを視聴できます。これは 24 時間見放題の映画レンタルに似たオプションです。* **Go Premium**: ユーザーは定期購読料を払って購読期間中にアプリ内のすべてのメディアを視聴できます。購読期間は開発者がAmazon開発者コンソールでアプリ内アイテムを設定する際に定義します。

アプリ内課金コンポーネントを使用すると、ユーザートランザクションの処理を気にする必要がありません。ユーザーはAmazonアカウントを通じて購入を行います。このアカウントはユーザーがFire TVを設定する際にセットアップします。アプリ内のトランザクションの詳細はアプリ内課金APIによって処理されます。

{% include note.html content="\"Daily Pass\" または \"Go Premium\" と表示されるボタンテキストをカスタマイズできます。UI文字列をカスタマイズする方法の詳細については、「[ボタンテキストをカスタマイズする](#buttontext)」を参照してください。" %}

* TOC
{:toc}

## Amazonアプリ内課金コンポーネントのユーザーエクスペリエンス

Amazonアプリ内課金コンポーネントをアプリに実装すると、ユーザーには [Content Details] 画面に ([Watch Now] ボタンの代わりに) [Daily Pass] ボタンと [Go Premium] ボタンが表示されます。  

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iapprompt" type="png" caption="<b>図 1.</b> [Content Details] 画面に、定期購読するか 1 日限りのパスを購入するかを選択するボタンが表示されます。" %}

ユーザーが [Go Premium] ボタンか [Daily Pass] ボタンをクリックすると、Fire TVアカウントを使用して購入するための画面が表示されます。たとえば、ユーザーが [Go Premium] をクリックすると、次の画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iapsubscriptionoptions" type="png" caption="<b>図 2.</b> 定期購読オプションのダイアログボックスが表示されます。" %}

[Daily Pass] をクリックした場合は、次の画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" type="png" caption="<b>図 3.</b> レンタルオプションのダイアログボックスが表示されます。" %}

{% include note.html content="開発者はアプリ内課金アイテムを設定する際に、これらのダイアログボックスに表示されるテキストをカスタマイズできます。" %}

ユーザーはFire TVダイアログボックスで、Fire TVのセットアップ時に入力したAmazonアカウントを使用してトランザクションを完了します。トランザクションが完了すると、次の画面が表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iappromptcomplete" type="png" caption="<b>図 4.</b> 注文が完了しました。" %}

この後、ユーザーはコンテンツの視聴を開始できます。

## アプリ内課金に関する注意事項と制限事項

アプリ内課金コンポーネントを使用する際は、下記の点に注意してください。

*  メディアには、Daily Pass (24 時間のアクセス) とGo Premium (定期購読) の両方のオプションを提供する必要があります。
*  Daily PassまたはGo Premiumを購入したユーザーは、アプリ内の*すべて*のメディアにアクセスできます。アイテムを個別に指定して定期購読やレンタルから除外し、残りを無制限で提供するということはできません。
*  映画を視聴している途中でDaily Passや定期購読の期限が切れた場合、ユーザーはその映画を最後まで視聴できます。アプリ内課金コンポーネントによって映画が停止されることはありません。
*  1 つのメディアに対して複数の定期購読タイプを提供することはできません。たとえば、コンテンツの再生にSDとHDのオプションを提供したり、月間購読と年間購読を提供したりすることはできません。
*  Daily Passは購入操作を行った端末でのみ使用できます。別のFire TV端末にログインしても、購入済みのDaily Passは機能しません。
*  Go Premium定期購読はすべての端末で機能します (あるFire TV端末で購入した定期購読を別のFire TV端末で利用できます)。ただし、別の端末にログインしたユーザーは、アプリを再起動して定期購読を有効にする必要があります。
*  Daily Passを購入したユーザーのFire TVアプリに障害が発生し、データが失われた場合、ユーザーはそのアプリからDaily Passにアクセスできなくなります。ただし、Go Premium (定期購読) では、障害が発生した後にアプリを再起動すれば引き続きアクセスできます。
*  ユーザーがコンテンツを定期購読した後で [Back] ボタンをクリックすると、[Go Premium] ボタンが再び表示されます。ただし、ユーザーがもう一度定期購読しようとすると、アプリによってユーザーがすでに定期購読をしていることが認識され、メディアへのアクセスが提供されます。

## Amazonアプリ内課金コンポーネントのワークフロー

以下の手順は、Amazonアプリ内課金コンポーネントの支払いワークフローを示しています。

1.  アプリを起動すると、アプリ内課金APIを通じて、現在ログインしているユーザーの既存の購入情報が取得され、最新のレシートに基づいて購入商品が更新されます。
2.  ユーザーが [Content Details] 画面で [Daily Pass] ボタンか [Go Premium] ボタンをクリックします (ユーザーはまだメディアを購入していない前提です)。
3.  アプリによってアプリ内課金APIが呼び出され、対象アイテムに関するユーザートランザクションが処理されます。ユーザーは自分がセットアップしたFire TVアカウントオプションを使用して購入を完了します。
4.  トランザクションが正常に完了すると、アプリ内課金APIによってトランザクションからアプリにレシートIDが返されます。
5.  (省略可能) レシートが有効であることを確認するために、自分のサービスに変更を加え、Receipt Verification Serviceにリクエストを送信してレシートIDが有効かどうかを確認するよう設定できます。6.  レシートIDが有効であれば、アプリによってメディアが表示されます。

## 構成の概要

アプリ内課金コンポーネントをセットアップするには、開発者ポータルの [In-App Items] セクションでDaily Passアイテム (消耗品) とGo Premiumアイテム (定期購読) を作成します。これらのアイテムのSKU情報を使用してアプリを構成します。必要な情報はコンポーネントによってアプリ内課金APIに送信されます。

Amazonアプリ内課金コンポーネントでは、IAP APIのラッパーが使用されます。利用できる機能は本来のIAPの一部のみです。そのため、本ドキュメントでは、[IAPに関するドキュメント](https://developer.amazon.com/public/ja/apis/earn/in-app-purchasing)の一部に変更を加えて記載しています。

### Daily Pass (消耗品) とGo Premium (定期購読)

アプリ内課金では、消耗品、権限、定期購読の 3 種類のアイテムが提供されています。Fire App Builderのアプリ内課金コンポーネントに関連するのは、消耗品と定期購読のみです (このコンポーネントは、IAP APIをベースにしたラッパーであるため、IAP API機能のすべてが適用されるわけではありません)。消耗品アイテムと定期購読アイテム (権限アイテム以外) はどちらもAmazonアプリ内課金コンポーネントにマップされています。

消耗品と定期購読の相違点は以下のとおりです。

*  **消耗品** (**Daily Pass**に相当) はアプリ内で消費され、何回でも購入できます。消耗品アイテムは、Fire App Builder内で "Daily Pass" にマップされています。ユーザーがすべてのメディアに 24 時間アクセスできようにする場合は、この種類を選択します。
*  **定期購読** (**Go Premium**に相当) では、購読期間中にコンテンツや機能のプレミアムセットにアクセスできます。定期購読アイテムは、Fire App Builder内で "Go Premium" にマップされています。ユーザーが購読期間中すべてのメディアにアクセスできようにする場合は、この種類を選択します。

### Amazonアプリ内課金コンポーネントの構成

アプリ内課金コンポーネントを構成するには、次の手順に従います。

*  [手順 1. 開発者ポータルでアプリを作成する](#createapp)
*  [手順 2. 消耗品 (Daily Pass) アプリ内アイテムを作成する](#creategopassitem)
*  [手順 3. 定期購読 (Go Premium) アプリ内アイテムを作成する](#creategopremiumitem)
*  [手順 4. JSONデータファイルをダウンロードしてプッシュする](#downloadjson)
*  [手順 5. アプリでアプリ内課金コンポーネントを有効にする](#enableiap)
*  [手順 6. アプリでアプリ内アイテムをコンテンツにマップする](#mapinappitems)
*  [手順 7. ボタンテキストをカスタマイズする](#buttontext)
*  [手順 8. App Testerを設定する](#apptester)

## 手順 1. 開発者ポータルでアプリを作成する {#createapp}

すでにアプリを作成済みの場合は、このセクションをスキップして[次へ](#creategopassitem)進んでください。

1.  開発者ポータルにログインします。
2.  [**新しいアプリを追加する**] をクリックし、[Choose a Platform] ダイアログボックスで [**Android**] を選択します。[**Next**] をクリックします。
3.  基本的なフィールドにアプリの情報を入力します。

    *  **アプリのタイトル**: アプリの名前。
    *  **アプリSKU**: アプリの一意の識別子 (例: `mycalypsomediapp1234`)。SKUの最大文字数は 150 文字です。使用できる文字は、a～z、A～Z、0～9、アンダースコア、ピリオド、およびダッシュです。SKUでは大文字と小文字が区別されます。
*  **カテゴリ**: アプリとメディアに適したカテゴリを選択します。{% comment %} これは、アプリストアに申請されるアプリと同じですか?ユーザーはアプリストアに申請する際、最終的にここに戻ってアプリを追加する必要がありますか?{% endcomment %}
4.  [**カスタマーサポートの連絡先**] では、[**デフォルトのサポート情報を使用する**] を選択するか、フィールドをカスタマイズします。[**保存**] をクリックします。

## 手順 2. 消耗品 (Daily Pass) アプリ内アイテムを作成する {#creategopassitem}

1.  開発者ポータルにログインし、[**ダッシュボード**] に移動して、当該アプリをクリックします。
2.  アプリの [**アプリ内アイテム**] タブをクリックします。
4.  [**Add a Consumable**] ボタン (Daily Pass用) をクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" type="png" caption="<b>図 5.</b> レンタルオプションのダイアログボックスが表示されます。"%}

5.  次の表に定義されているフィールドに値を入力します。

    <table class="grid">
      <thead>
        <tr>
          <th>フィールド</th>
          <th>説明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Title</strong></td>
          <td>このアイテムを参照するための名前。このタイトルはユーザーには表示されません。わかりやすいように、「<strong>Daily Pass</strong>」と入力します。</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>このアイテムのIDとなる、アプリに固有の文字列。例: "com.amazon.example.iap.mydailypassitem"。アプリが使用するSKUは、Amazonアプリストアに申請するSKUと同じである必要があります。前述の (アプリを作成した際と同じ) SKU文字に関する要件が、ここでも適用されます。</td>
        </tr>
        <tr>
          <td><strong>コンテンツデリバリー</strong></td>
          <td>配信するコンテンツはアプリの一部として含まれるため、[<strong>No additional file required</strong>] を選択します。</td>
        </tr>
      </tbody>
    </table>

6.  [**保存**] をクリックします。

    {% include note.html content="次のタブに移動する前に、必ず、現在のタブに入力した情報を保存してください。" %}

7.  [**配信地域・価格等**] タブをクリックし、次の表に示す詳細情報を入力します。

    <table class="grid">
      <thead>
        <tr>
          <th>フィールド</th>
          <th>説明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Are you charging for this consumable / entitlement?</strong></td>
          <td>[<strong>はい。基本表示価格は...</strong>] を選択します。フィールドが表示され、そこにアイテムの基本表示価格と通貨を設定できます。基本表示価格を設定した後で、他の通貨向けの価格を手動で設定するか、Amazonアプリストアが交換レートと税金に基づいて設定するのを許可することができます。有効な価格 (USDの場合) は、0.00 USDか、0.99～99.00 USDです。</td>
        </tr>
        <tr>
          <td><strong>When would you like this item to be available?</strong></td>
          <td>アプリでこのアイテムの提供を開始する日付を指定します (IAPでは米国太平洋標準時が使用されます)。このフィールドを空白のままにすると、アイテムはAmazonアプリストアの承認後すぐに提供が開始されます。</td>
        </tr>
      </tbody>
    </table>

8.  [**保存**] をクリックします。
9.  [**説明**] タブをクリックします。
10. [**表示タイトル**] フィールドと [**説明**] フィールドに入力します。

    これらのフィールドは、ユーザーが [**Daily Pass**] をクリックするとダイアログボックスに表示されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" type="png" caption="<b>図 6.</b> [表示タイトル] フィールドと [説明] フィールドが表示されます。" %}

11. [**保存**] をクリックします。
12. [**画像**] タブをクリックします。
13. サイズの要件に従って、2 つの画像をアップロードします。アプリでユーザーに表示されるDaily Passダイアログボックスには、114pxの画像のみが表示されます。前出のスクリーンショット (図 6) では、画像がオレンジ色の四角形になっています。512pxの画像を作成する際は、便宜的に、こちらの[汎用画像](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/512.png)をアップロードできます。
14. [**保存**] をクリックします。

    {% include warning.html content="まだ [**Submit In-App Item**] ボタンをクリックしないでください。アプリ内アイテムはいったん申請すると、後で編集または削除することができません。このボタンは、機能とコンテンツを十分にテストし、アプリを申請する準備ができた後でのみクリックしてください。"%}

## 手順 3. 定期購読 (Go Premium) アプリ内アイテムを作成する {#creategopremiumitem}

1.  ログインしていない場合は開発者ポータルにログインし、[**ダッシュボード**] に移動して、対象のアプリをクリックします。
2.  アプリの [**アプリ内アイテム**] タブをクリックします。
4.  [**Add a Subscription**] ボタン (Go Premium用) をクリックします("Subscriptions are not supported by Amazon Underground" というメッセージが表示された場合は、[**Add a Subscription**] ボタンをもう一度クリックしてください)。5.  次の表に定義されているフィールドに値を入力します。

    <table class="grid">
      <thead>
        <tr>
          <th>フィールド</th>
          <th>説明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Title</strong></td>
          <td>このアイテムを参照するための名前。このタイトルはユーザーには表示されません。わかりやすいように、「<strong>Go Premium</strong>」と入力します。</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>このアイテムのIDとなる、アプリに固有の文字列。例: "com.amazon.example.iap.mygopremiumitem"。アプリが使用するSKUは、Amazonアプリストアに申請するSKUと同じである必要があります。前述の (アプリを作成した際と同じ) SKU文字に関する要件が、ここでも適用されます。</td>
        </tr>
        <tr>
          <td><strong>コンテンツデリバリー</strong></td>
          <td>配信するコンテンツはアプリの一部として含まれるため、[<strong>No additional file required</strong>] を選択します。</td>
        </tr>
      </tbody>
    </table>

6.  [**保存**] をクリックします。

    {% include note.html content="次のタブに移動する前に、必ず、現在のタブに入力した情報を保存してください。"%}

7.  [**購読期間**] タブをクリックし、次の表に示すフィールドに値を入力します。

     <table class="grid">
      <thead>
        <tr>
          <th>フィールド</th>
          <th>説明</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>購読期間</strong></td>
          <td>適用する購読期間を選択します。購読期間は購入日から始まります。</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>この購読期間に対応する一意のSKUを入力します (他のSKUフィールドと同じガイドラインに準拠してください)。例: "com.amazon.example.calypso.monthly"。このSKUは、アイテムの詳細ページで入力したSKUの子SKUです。</td>
        </tr>
        <tr>
          <td><strong>無料体験</strong></td>
          <td>必要に応じて、定期購読の無料体験期間を指定します。</td>
        </tr>
        <tr>
          <td><strong>定期購読の料金を請求しますか?</strong></td>
          <td>[<strong>はい。基本表示価格は...</strong>] を選択します。フィールドが表示され、そこに基本表示価格と通貨を設定できます。基本表示価格を設定した後で、他の通貨向けの価格を手動で設定するか、Amazonアプリストアが交換レートと税金に基づいて設定するのを許可することができます。有効な価格 (USDの場合) は、0.00 USDか、0.99～99.00 USDです。</td>
        </tr>
        <tr>
          <td><strong>When would you like this item to be available?</strong></td>
          <td>アプリでこのアイテムの提供を開始する日付を指定します (IAPでは米国太平洋標準時が使用されます)。このフィールドを空白のままにすると、アイテムはAmazonアプリストアの承認後すぐに提供が開始されます。</td>
        </tr>
      </tbody>
    </table>

    {% include note.html content="[保存して購読期間を追加] ボタンは無視してください。課金コンポーネントは 1 つの購読期間のみをサポートしています。"%}

8.  [**保存**] をクリックします。
9.  [**説明**] タブをクリックします。
10. [**表示タイトル**] フィールドと [**説明**] フィールドに入力します。

    これらのフィールドは、ユーザーが [**Go Premium**] をクリックするとダイアログボックスに表示されます。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" type="png" caption="<b>図 7.</b> [表示タイトル] フィールドと [説明] フィールドが表示されます。"%}

11. [**保存**] をクリックします。
12. [**画像**] タブをクリックします。
13. 開発者コンソールで画像のアイコンが要求されますが、Daily Pass (消耗品) とは異なり、アプリでユーザーに表示される [Go Premium] ダイアログボックスではどの画像も*使用されません*。便宜的に、こちらの汎用 [114px画像](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/114.png)やこちらの汎用 [512px画像](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/512.png)をアップロードできます。
14. [**保存**] をクリックします。

    {% include warning.html content="まだ [**Submit In-App Item**] ボタンをクリックしないでください。アプリ内アイテムはいったん申請すると、後で編集または削除することができません。このボタンは、機能とコンテンツを十分にテストし、アプリを申請する準備ができた後でのみクリックしてください。"%}

## 手順 4.JSONデータファイルをダウンロードしてプッシュする {#downloadjson}

Daily PassとGo Premiumの両方のアプリ内アイテムを設定したら、アプリ内アイテムの詳細が含まれたJSONファイルをダウンロードします。

1.  ログインしていない場合は開発者ポータルにログインし、[**ダッシュボード**] に移動して、対象のアプリをクリックします。
2.  アプリの [**アプリ内アイテム**] タブをクリックします。4. [**JSON Data File**] ボタンをクリックし、アプリ内アイテムの詳細が含まれたJSONファイルをダウンロードします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_jsonfiledownload" type="png" content="<b>図 8.</b> JSONファイルをダウンロードします。"%}

4.  ターミナルで`cd`を実行し、このファイルが含まれているディレクトリに移動します。
5.  次のコマンドを使用して、JSONファイルをFire TV端末にプッシュします。

    ```bash
    $ adb push amazon.sdktester.json /mnt/sdcard/
    ```

    レスポンスが次のように表示されます。`[100%] /mnt/sdcard/amazon.sdktester.json`

## 手順 5. アプリでアプリ内課金コンポーネントを有効にする {#enableiap}

開発者コンソールでアプリ内アイテムを作成できたら、続いてはアプリでIAPコンポーネントを有効にする必要があります。

1.  Amazonアプリ内課金コンポーネントをアプリにロードします。アプリにコンポーネントをロードする方法の詳細については、「[コンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
2.  Android Studio (Androidビュー) で、**ContentBrowser**フォルダーを展開し、**res > values**に移動して、custom.xmlファイルを開きます。
3.  次の文字列をコピーします。

    ```xml
    <bool name="is_iap_disabled">true</bool>
    ```

    (`override_all_contents_subscription_flag`フィールドは無視してください。)

4.  **app**フォルダーで (**res > values**にあります).**custom.xml**ファイルを開きます
5.  先ほどコピーした文字列を貼り付け、値を`false`に変更します。この文字列は必ず 2 つの`resources`タグの間に配置してください。

    ```xml
    <bool name="is_iap_disabled">false</bool>
    ```

    {% include tip.html content="これは二重否定となっているため、`false`に設定することで実際にはアプリでIAPが有効化されます。" %}

    コンポーネントにすでに設定されている値は、アプリのcustom.xmlの値によって上書きされます。

## 手順 6. アプリでアプリ内アイテムをコンテンツにマップする {#mapinappitems}

1.  Android Studio (Androidビュー) で**PurchaseInterface**フォルダーを展開し、**assets**に移動します。**skuslist.json**ファイルを開きます。

2.  "Go Premium"アイテムの場合は、赤で示されているフィールドを実際のGo PremiumアイテムのSKU値に置き換えます。

    <pre>
    {
      "skusList": [
        {
          "sku": "<span class="red">testSubSku</span>",
          "productType": "SUBSCRIBE",
          "purchaseSku": "<span class="red">testSubSku</span>"
        },
        {
          "sku": "RentUnPurchased",
          "productType": "RENT",
          "purchaseSku": "RentUnPurchased"
        }
      ],
      "actions": {
        "CONTENT_ACTION_DAILY_PASS": "RentUnPurchased",
        "CONTENT_ACTION_SUBSCRIPTION": "<span class="red">testSubSku</span>"
      }
    }
    </pre>

    Go Premiumアイテムは定期購読 (SUBSCRIBE) アイテムです。

2.  "Daily Pass" アイテムの場合は、赤で示されているフィールドを実際のDaily PassアイテムのSKU値に置き換えます。

    <pre>
    {
      "skusList": [
        {
          "sku": "testSubSku",
          "productType": "SUBSCRIBE",
          "purchaseSku": "testSubSku"
        },
        {
          "sku": "<span class="red">RentUnPurchased</span>",
          "productType": "RENT",
          "purchaseSku": "<span class="red">RentUnPurchased</span>"
        }
      ],
      "actions": {
        "CONTENT_ACTION_DAILY_PASS": "<span class="red">RentUnPurchased</span>",
        "CONTENT_ACTION_SUBSCRIPTION": "testSubSku"
      }
    }
    </pre>

    Daily Passアイテムはレンタル (RENT) アイテムです。

## 手順 7.ボタンテキストをカスタマイズする {#buttontext}

[Content Details] 画面のボタンに表示される "Daily Pass" または "Go Premium" というテキストをカスタマイズできます。

テキストをカスタマイズするには:

1.  **ContentBrowser > res > values**に移動し、**strings.xml**ファイルを開きます。
2.  次の文字列をコピーし、アプリの**custom.xml**ファイルに貼り付けます。

    ```xml
    <string name="premium_1">Go</string>
    <string name="premium_2">Premium</string>
    <string name="daily_pass_1">Daily</string>
    <string name="daily_pass_2">Pass</string>
    ```

    また、課金コンポーネントに関連のある他の文字列もコピーします。

    ```xml
    <string name="iap_error_dialog_title">Purchase Error</string>
    <string name="subscription_expired">Your subscription expired!</string>
    <string name="ok">Ok</string>
    ```
3. 必要に応じて、文字列の値をカスタマイズします。

## 手順 8.App Testerを設定する {#apptester}

アプリ内アイテムをセットアップし、アプリを構成できたら、統合状態をテストし、IAPとメディアが正しく連携するかを確認します。

アプリストアにアプリを申請する前に、実際の購入を行わずにアプリのアプリ内アイテムをテストするには、IAP App Testerを使用します。App Testerは、サンドボックスサーバーを使用してアプリ内課金APIの呼び出しをシミュレートします。App Testerはアプリ内課金APIへの発信呼び出しをインターセプトし、有効なレシートを返して、トランザクションを許可します(詳細については、「[App Testerをインストールして設定する](https://developer.amazon.com/public/ja/apis/earn/in-app-purchasing/docs-v2/installing-and-configuring-app-tester)」を参照してください)。

App Testerを設定するには:

1.  コンピューターのブラウザで[アプリストアのApp Tester](https://www.amazon.com/gp/product/?ie=UTF8&ASIN=B00BN3YZM2&ref=mas_ty)にアクセスし、このアプリを入手して (無料)、端末に配信します。

    アプリを端末に配信するには、Fire TVをセットアップしたときに使用したものと同じ認証情報を使用してAmazonにサインインする必要があります。[Deliver to] オプションを使用して、アプリを端末に配信します。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_apptesterdelivery" type="png" caption="<b>図 9.</b> [Deliver to] オプションを使用して、Fire TV端末にアプリを送信できます。" %}

2.  Fire TVで [**Settings**] > [**My Account**] > [**Sync Amazon Content**] の順に選択して、購入情報を端末と同期します。
3.  Fire TVホーム画面から [**アプリ**] に移動し、[**Your Apps Library**] を選択します。
4.  [**Amazon App Tester**] アプリを選択してダウンロードし、クラウドからアプリをインストールします。
5.  ダウンロードが完了したら、アプリを起動します。
6.  [**アプリ内課金API**] セクションをクリックします。

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaptester" type="png" caption="<b>図 10.</b> アプリ内課金APIオプション。" %}

7.  [**5. IAP Items in JSON File**] を選択し、amazon.sdktester.jsonファイルに含まれるアイテムがここに表示されていることを確認します。

### 呼び出しをシミュレートする

アプリとテストツールを構成できたので、IAPサービスの呼び出しをシミュレートできます。

1.  [ADB][fire-app-builder-connecting-adb-to-fire-tv]を使用して、アプリを起動します
2.  アプリのメディアを参照します。
2.  [**Daily Pass**] ボタンまたは [**Subscribe**] ボタンをクリックします。

### アプリ内アイテムを調整する

[説明] タブの [タイトル] フィールドと [説明] フィールドを、アプリに表示されるテキストに合わせて調整できます。

アプリ内アイテムのテキストやその他の機能を調整するには、管理コンソールにログインして、対象のアプリをクリックし、[In-App Items] タブを開きます。アプリを申請する前であれば、すべてのフィールドを編集できます。

### 購入情報をリセットする

機能のテスト時に、次の手順に従ってDaily PassやGo Premiumの購入情報をリセットできます。

1.  [**設定**] > [**アプリ**] > [**インストールしたアプリの管理**] > [**Amazon App Tester**] に移動します。
2.  [**Clear data**] および [**Clear cache**] を選択します。

## Receipt Verification Serviceを使用したセキュリティの強化 {#additionalsecurity}

ユーザーがメディアのDaily Passまたは定期購読を購入すると、トランザクションが有効かどうかを示すレシートがIAP APIによってアプリに返されます。Fire TV端末はユーザーが所有しているため、IAP APIへの発信呼び出しをハッカーが再ルーティングしてレシートを偽造する可能性があります。

IAP APIから返されたレシートが有効であることを確認するために、*開発するサービス*がAmazonの[Receipt Verification Service](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/verifying-receipts-in-iap-2.0)を呼び出すように設定できます。

以下は、Fire App Builderを使用してReceipt Verification Serviceでレシートを確認するための一般的なプロセスです。

*  お使いのサーバーを (任意の言語/プラットフォームを使用して) レシートおよび付随するシークレットをReceipt Verification Serviceに送信してレシートが有効かどうかを問い合わせるように設定します。
*  アプリによって、必要なプロパティと共有シークレットが含まれたリクエストがReceipt Verification Serviceに送信され、レシートが確認されます。必要な値は**AmazonInAppPurchaseComponent > res > values values.xml**に記載されています。
*  Receipt Verification Serviceでは、シークレットを検証した後、レシートが有効であるかどうかを判断し、レスポンスでtrueまたはfalseを返します。

このワークフローはFire App Builderには含まれていません。Fire App Builderはデフォルトで、常に有効を返すダミーサービスを呼び出します。具体的には、Fire App Builderの (AmazonInAppPurchaseComponentフォルダー内にある) `AReceiptVerifier`クラスは抽象クラスで、開発者はそのサブクラスを拡張してReceipt Verification Serviceを呼び出します。

Fire App Builderに用意された`DefaultReceiptVerificationService`は、`AReceiptVerifier`を拡張したサンプルサブクラスです。`DefaultReceiptVerificationService`内の`validateReceipt`メソッドによって、Receipt Verification Serviceに必要なパラメータがすべて実装されます。

*  `context`:   アプリのコンテキスト
*  `requestId`: このリクエストのリクエストID
*  `sku`:       レシートのSKU
*  `userData`:  現在のユーザーのユーザーデータ
*  `receipt`:   検証するレシート
*  `listener`:  このリクエストのリスナー
*  `return`: リクエストID

ただし、実際には`validateReceipt`によってReceipt Verification Serviceは呼び出されません。代わりに、成功のステータスが返されます。

開発者は自分でサブクラスを作成し、実際のReceipt Verification Service呼び出しで`AReceiptVerifier`を拡張する必要があります(または、`DefaultReceiptVerificationService`サブクラスをカスタマイズすることもできます)。

サブクラスを作成する際は、次の手順に従ってアプリのcustom.xmlファイルにサブクラス名を指定する必要があります。

1.  **AmazonInAppPurchaseComponent > res > values**に移動し、**strings.xml**ファイルを開きます。
2.  次の`receipt_verification_service_iap`文字列をコピーし、アプリの**custom.xml**ファイル (app > resourcesの内側) に貼り付けます。

    ```xml
    <string name="receipt_verification_service_iap">com.amazon.inapppurchase.DefaultReceiptVerificationService</string>
    ```

3.  アプリの**custom.xml**ファイルで、文字列の値をサブクラスへの完全なパスに置き換えます。デフォルトでは、`DefaultReceiptVerificationService`が構成されています。新しいサブクラスを作成するか、`DefaultReceiptVerificationService`クラスを上書きできます。

Receipt Verification Serviceの呼び出しの詳細については、「[Receipt Verification Service](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/verifying-receipts-in-iap-2.0)」を参照してください。

{% include note.html content="実装するセキュリティレベルは、正当なトランザクションの確認に費やすリソースによって異なります。お使いのサーバーでレシートを確認することが適切である (サーバー管理コストが収益を上回らない) 場合に、Receipt Verification Serviceを使用したこのセキュリティ強化を実装してください。アプリの収益がサーバー管理のコストや手間に見合わない場合は、この追加の設定を省略できます。"%}

## アプリ内アイテムを本番環境に申請する

ここまでは、テスト環境でアプリを構成する方法について説明してきました。アプリストアにアプリを申請する準備が整ったら、アプリ内アイテムも申請する必要があります。

アプリ内アイテムを申請するには:

1.  開発者ポータルにログインし、[**ダッシュボード**] に移動して、当該アプリをクリックします。
2.  アプリの [**アプリ内アイテム**] タブをクリックします。3.  [**Submit In-App Item**] ボタンをクリックします。

アプリ内アイテムはいったん申請すると、後で編集することができません。すでに申請したアプリ内アイテムを変更する必要が生じた場合は、[アプリの新しいバージョンを作成する](https://developer.amazon.com/public/support/submitting-your-app/tech-docs/submitting-your-app#Updating an Existing App)か、新しいアプリを作成して、そのアプリに新しいアプリ内アイテムを追加する必要があります。

{% include links.html %}
