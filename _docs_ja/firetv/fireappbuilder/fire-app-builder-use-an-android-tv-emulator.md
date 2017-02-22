---
title: "Android TVエミュレーターを使用してアプリを実行する"
permalink: fire-app-builder-use-an-android-tv-emulator.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire App Builderでアプリを開発する際は、実際のFire TV端末を使用してアプリをテストする必要があります。詳細については、「[ADBを使用してFire TVに接続する][connecting-adb-to-fire-tv-device]」を参照してください。ただし、エミュレーターしか使用できない場合でも、いくつかある制限事項を許容できれば、エミュレーターでテストすることもできます。エミュレーターも問題なく機能しますが、マウスでメディアプレーヤーボタンをクリックすることができません。

マウスクリックによって生成される動作イベントは、Fire App Builderのメディアプレーヤーではサポートされていません (logcatに "java.lang.ClassCastException: android.view.MotionEvent cannot be cast to android.view.KeyEvent" というエラーが表示されます)。そのため、マウスでメディアプレーヤーのボタンをクリックすると、アプリがエミュレーターでクラッシュします。

メディアを再生した後に、メディアの再生画面でマウスを使用せずに前の画面に戻るには、エミュレーターの右にある [**Back**] ボタンをクリックします (以下のスクリーンショットの矢印が指しているボタン)。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_emulator" type="jpg" %}

メディアプレーヤーのボタンをマウスでクリックしないようしてください。メディア再生画面の外側であれば、どこでもマウスを使用してクリックできます。

**エミュレーターを構成するには:**

Android TVエミュレーターを構成する際は、少なくともAPIレベル 23 または 24 を選択する必要があります。その他の設定 (解像度、サイズなど) は自由に設定できます(APIレベル 24 を選択した場合、このAPIレベルの要件であるInstant Runをインストールするよう求められます)。

開発するアプリ用にAndroid TVエミュレーターを設定するには:

1.  [**Tools**] > [**Android**] > [**AVD Manager**] の順に移動するか、上部のナビゲーションバーにある [**AVD Manager**] ボタン{% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_avdmanager" type="png" %}を選択します。
2.  [**+ Create Virtual Device**] ボタンをクリックします。

    {% include note.html content="デフォルトのTVプロファイルのいずれかを選択するか、以下の手順で設定をカスタマイズできます。デフォルトのTVプロファイルを選択した場合は、手順 12 へスキップし、システムイメージを選択します。" %}

3.  [**Category**] 列で [**TV**] を選択します。
4.  [**New Hardware Profile**] ボタンをクリックします。
5.  [**Device Name**] に「**fire_tv_emulator**」のように名前を入力します(エラーが発生することがあるため、名前に括弧を使用しないようにしてください)。
6.  [**Device Type**] で [**Android TV**] を選択します。
7.  [**Screen size**] で、必要な画面サイズ (「**40**」など) を入力します。
8.  [**Resolution**] で、必要な解像度 (「**1280 x 720**」など) を入力します。
9.  [**Supported device states**] で、[**Landscape**] だけを選択します ([Portrait] チェックボックスをオフにします)。
10. [**Finish**] をクリックします。
11. [Choose a device definition] ダイアログボックスで、今作成したエミュレーターを選択して [**Next**] をクリックします。
12. [Release Name] 列で、[**Marshmallow API Level 23**] 以上を選択します。このシステムイメージをまだダウンロードしていない場合は、[**Download**] をクリックしてダウンロードします(APIレベル 22 以下を選択すると、エミュレーターでメディアの再生が失敗します)。
13. [**Next**] をクリックして [**Finish**] をクリックします。

仮想デバイスでエミュレーターが選択肢として表示されるようになります。

{% capture avdname_note %}デフォルトのTVプロファイルを選択した場合、デバイス名を変更して名前から括弧を削除する必要があります。括弧があると、アプリ実行時にエラーが発生します(\"virtual device name contains invalid characters.\" というエラーが表示されます)。仮想デバイスの名前を変更するには、仮想デバイスが表示されているときに右側の [Actions] 列の [**Edit**] ボタン{% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_editthisavd" type="png" %}をクリックします。次に、デバイス名を変更します。{% endcapture %}

{% include note.html content=avdname_note %}

[**Run 'app'**] ボタン{% include inline_image.html alt="[Run 'app'] ボタン" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %}をクリックしてアプリを実行します。作成した仮想デバイスを選択します。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_selectingemulator" type="png" max-width="80%" %}

これでエミュレーターを使用できるようになりました。メディアを再生する際は注意してください。メディアを再生するときにメディアプレーヤーのボタンをマウスでクリックしないでください。代わりにキーボードを使用するか、上記のスクリーンショットで示したとおり、エミュレーターの右側にあるボタンを使用してください。


{% include links.html %}
