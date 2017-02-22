---
title: コントローラ動作のガイドライン
permalink: controller-behavior-guidelines.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Amazon Fire TVプラットフォーム向けのアプリを開発する際、各種コントローラからの入力をサポートできます。各種コントローラには、Amazon Fire TV RemoteとVoice Remote、Amazon Fire TVゲームコントローラ、Bluetooth HIDゲームパッドプロファイルをサポートするその他のコントローラが含まれます。


アプリにコントローラ入力を実装するには、「[リモート入力][amazon-fire-tv-remote-input]」と「[ゲームコントローラ入力][amazon-fire-game-controller-input]」に記載されているモーションイベントと入力イベントを使用します。

このページでは、さまざまなコントローラに共通するアプリ機能の推奨事項について説明します。以下のガイドラインは、Amazon Fire TV用アプリを公開する要件ではありません。開発者は、アプリに最適の方法でコントローラ入力を設計できます。ただし、異なるコントローラ、アプリ、ゲームでも一貫したユーザーエクスペリエンスを提供するために、これらのガイドラインに従うことをおすすめします。

{% include note.html content="[Microphone] ボタンを除けば、すべてのFire TVのリモコンの動作は同じです。Fire TV Remoteに関するこのドキュメントのガイドラインは、Fire TV Voice Remoteにも適用されます。" %}

* TOC
{:toc}

## コア動作

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="40%" />
</colgroup>
  <thead>
    <tr>
      <th>アクション</th>
      <th>Amazon Fire TV Remoteのボタン</th>
      <th>Amazon Fire TVゲームコントローラのボタン</th>
      <th>その他のゲームコントローラのボタン</th>
      <th>動作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ホーム</td>
      <td>Home</td>
      <td>Home</td>
      <td>Home (存在する場合) </td>
      <td>これはシステムイベントで、アプリではキャプチャできません。このボタンを押すと、ホームに戻ります。Amazonアプリストアでゲームに分類されるアプリの場合は、GameCircleオーバーレイまたは [Game Paused] 画面が表示されます。もう一度 [Home] ボタンを押すと、ホームに戻ります。<br /><br /> 予期せずホームに戻った場合にアプリまたはゲームの状態を保持するには、<a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"><code>onPause()</code></a> を実装します。アプリを続きから再開できるようにするには、<a href="http://developer.android.com/reference/android/app/Activity.html#onResume()"><code>onResume()</code></a> を実装します。<br /><br />オーディオアプリの場合、<a href="http://developer.android.com/training/managing-audio/audio-focus.html">オーディオフォーカス</a>をリクエストして、バックグラウンドで再生を続けることができます。</td>
    </tr>
    <tr>
      <td>戻る</td>
      <td>Back</td>
      <td>Back</td>
      <td>B</td>
      <td>直前の操作または画面 (アクティビティ) に戻るか、現在の操作またはプロンプトを取り消します。<br /><br />確認ダイアログを表示するには、このイベントをキャプチャします。たとえば、アプリのメイン画面に [Do you want to Quit] ダイアログを表示するには、[Back] をキャプチャします。</td>
    </tr>
    <tr>
      <td>メニュー</td>
      <td>Menu</td>
      <td>Menu</td>
      <td>Y</td>
      <td>Androidの標準コンテキストメニュー (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>) が起動されます。<br /><br />独自のメニューを提供するか、他の目的に使う場合は、このイベントをキャプチャします。提供するメニューに 1 つしかオプションがない場合は、[Menu] ボタンをそのオプションの切り替えボタンとして使用できます。<br /><br />メディアアプリでは、[Menu] ボタンを使って再生パネルを表示または非表示にできます。</td>
    </tr>
    <tr>
      <td>検索</td>
      <td>Microphone (Voice Remoteのみ)</td>
      <td>該当なし</td>
      <td>該当なし</td>
      <td>これはシステムイベントで、アプリではキャプチャできません。このボタンを押すと、音声検索が起動されます。<br /><br />音声検索の起動時にアプリまたはゲームの状態を保持するには<a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"><code>onPause()</code></a> を実装し、検索完了後にアプリやゲームを続行できるようにするには<a href="http://developer.android.com/reference/android/app/Activity.html#onResume()"><code>onResume()</code></a> を実装します。<br /><br />オーディオアプリでは、音声検索の起動中は、再生を一時停止するか、音量を小さくします。<br /><br />ビデオアプリでは、音声検索の起動中は、音声をミュートするか、再生を一時停止します。</td>
    </tr>
    <tr>
      <td>GameCircle</td>
      <td>該当なし</td>
      <td>GameCircle</td>
      <td>該当なし</td>
      <td>これはシステムイベントで、アプリではキャプチャできません。このボタンを押すと、GameCircleオーバーレイ (GameCircleをサポートしているゲームの場合)、または [Game Paused] ダイアログ (GameCircleをサポートしていないゲームの場合) が表示されるか、ランチャーの [Games] ページに戻ります (他のすべてのアプリの場合)。<br /><br /> <strong>注意:</strong> サイドロードされたアプリは、GameCircleを実装していても必ずランチャーの [Games] ページに戻ります。GameCircleの正しい動作を示すのは、Amazonアプリストアに申請されたアプリだけです。アプリを申請する前に、<a href="https://developer.amazon.com/public/resources/development-tools/live-app-testing">ライブアプリテスト</a>を使用してGameCircleの統合をテストできます。<br /><br /> [GameCircle] ボタンが押されたときにアプリまたはゲームの状態を保持するには、<a href="http://developer.android.com/reference/android/app/Activity.html#onPause()"> <code>onPause()</code></a> を実装します。ユーザーがゲームに戻ったときに続きから再開できるようにするには、<a href="http://developer.android.com/reference/android/app/Activity.html#onResume()">onResume()</a> を実装します。</td>
    </tr>
  </tbody>
</table>

## ナビゲーションと選択

次の表では、ユーザーインターフェースのナビゲーションと選択で推奨される動作について説明します。複数のボタンを表示するアイテムでは、すべてのボタンを対象にしてください。

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="40%" />
</colgroup>
  <thead>
    <tr>
      <th>アクション</th>
      <th>Amazon Fire TV Remoteのボタン</th>
      <th>Amazon Fire TVゲームコントローラのボタン</th>
      <th>その他のゲームコントローラのボタン</th>
      <th>動作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>選択/メインアクション</td>
      <td>D-Padの [Center]</td>
      <td>A</td>
      <td>A</td>
      <td>フォーカスされたアイテムを選択するか、メニューオプションまたはプロンプトを確定するか、メインゲームアクションを実行します。</td>
    </tr>
    <tr>
      <td>キャンセル/戻る</td>
      <td>Back</td>
      <td>Back <br />B</td>
      <td>B</td>
      <td>現在の操作を取り消すか、直前の画面に戻ります。<br /><br />確認ダイアログ ("Are you sure you want to quit?") を表示するには、このアクションをインターセプトします。</td>
    </tr>
    <tr>
      <td>上<br />下<br />左<br />右</td>
      <td>D-Pad</td>
      <td>D-Pad<br />左側のジョイスティック</td>
      <td>D-Pad<br />左側のジョイスティック</td>
      <td>入力フォーカスを該当する方向に移動します。<br /> <br />Amazon Fireゲームコントローラやその他のゲームコントローラでは、左側のジョイスティックがD-Padと同じ動作をします。</td>
    </tr>
  </tbody>
</table>

## メディアの再生

次の表では、メディアの再生に関する推奨動作を説明します。次の点に注意してください。

* メディアを再生しないアプリや、アナログスティックまたはショルダーボタン (L1/R1) を使用するアプリの場合には、それらのボタンのイベントをキャプチャしないでください。他の機能にキャプチャすると、システムがバックグラウンドで再生しているメディアをコントロールできなくなる可能性があります。
* Unityなどのフレームワークはシステム経由で主要なイベントを渡す機能がサポートされていないため、これらのフレームワークを使用しているアプリの場合、この推奨を無視できます。
* アプリやゲームでこれらのボタンを他の目的のために使用する場合、GameCircleオーバーレイ (GameCircleボタン) から、またはFire TVランチャーで、システムメディアコントロールにアクセスできます。

<table class="grid">
<colgroup>
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="15%" />
<col width="25%" />
</colgroup>
  <thead>
    <tr>
      <th>アクション</th>
      <th>Amazon Fire TV Remoteのボタン</th>
      <th>Amazon Fire TVゲームコントローラのボタン</th>
      <th>Amazon Fireゲームコントローラ (第 1 世代) のボタン</th>
      <th>その他のゲームコントローラのボタン</th>
      <th>動作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>再生/一時停止</td>
      <td>Play/Pause</td>
      <td>A<br />左側/右側のジョイスティックの押し下げ</td>
      <td>Play/Pause</td>
      <td>A</td>
      <td>メディアの再生と一時停止を切り替えます。</td>
    </tr>
    <tr>
      <td>早戻し</td>
      <td>Rewind<br />Left (D-Pad)<br />左側のショルダー (L1)</td>
      <td>左側のショルダー (L1)</td>
      <td>Rewind<br />Left (D-Pad)<br />左側のショルダー (L1)</td>
      <td>Left (D-Pad) <br />左側のショルダー (L1)</td>
      <td>再生中のメディアコンテキストが早戻しされます。実際の動作はそれぞれのメディアによって異なります。ビデオなら再生位置の調整に、音楽なら直前のトラックに戻るために、スライドショーなら直前の写真に移動するために、このボタンを使用できます。</td>
    </tr>
    <tr>
      <td>早送り</td>
      <td>FF<br />Right (D-Pad)<br />右側のショルダー (R1)</td>
      <td>右側のショルダー (R1)</td>
      <td>FF<br />Right (D-Pad)<br />右側のショルダー (R1)</td>
      <td>Right (D-Pad)<br />右側のショルダー (R1)</td>
      <td>再生中のメディアコンテキストが早送りされます。正確な動作はそれぞれのメディアによって異なります。ビデオなら再生位置の調整に、音楽なら次のトラックに進むために、スライドショーなら次の写真に移動するために、このボタンを使用できます。</td>
    </tr>
  </tbody>
</table>


## 音量制御

Amazon Fire TVでは、オーディオをAmazon Fire TVゲームコントローラのヘッドホンジャックにストリーミングできます。オーディオ再生の音量制御は、左側/右側のトリガーボタン (L2/R2) を使用して行うことができます。音量制御はシステム機能であり、アプリの他のボタンにマップできません。

次の点に注意してください。

* これらのボタンを使用しないアプリまたはゲームの場合、それらの入力イベントをキャプチャしないでください。他の機能にキャプチャすると、ユーザーは音量をコントロールできなくなる可能性があります。
* アプリやゲームでこれらのボタンを他の目的のために使用する場合、GameCircleオーバーレイから、またはFire TVランチャーで、システム音量制御にアクセスできます。

<table class="grid">
<colgroup>
<col width="30%" />
<col width="70%" />
<col width="15%" />
</colgroup>
  <thead>
    <tr>
      <th>アクション</th>
      <th>Amazon Fire TVゲームコントローラのボタン</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>音量 +</td>
      <td>左側のトリガー (L2) </td>
    </tr>
    <tr>
      <td>音量 -</td>
      <td>右側のトリガー (R2) </td>
    </tr>
  </tbody>
</table>

## ゲームプレイ

ゲームプレイのユーザーインターフェースはゲームによって大きく違いますが、基本的な推奨事項は次の表の通りです。


<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="30%" />
<col width="30%" />
</colgroup>
  <thead>
    <tr>
      <th>アクション</th>
      <th>Amazon Fire TV Remoteのボタン</th>
      <th>Amazon Fire TVゲームコントローラのボタン</th>
      <th>その他のゲームコントローラのボタン</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>プライマリゲームプレイアクション</td>
      <td>D-Padの [Center]</td>
      <td>A</td>
      <td>A</td>
    </tr>
    <tr>
      <td>セカンダリゲームプレイアクション</td>
      <td>推奨なし</td>
      <td>B</td>
      <td>B</td>
    </tr>
  </tbody>
</table>


{% include links.html %}
