---
title: Unityによるコントローラ入力
permalink: controller-input-with-unity.html
sidebar: firetv_ja
product: Fire TV
toc: false
---

[Unity開発ツール](http://unity3d.com/unity)を使用すれば、Android端末用のアプリやゲームを作成するのと同様に、Amazon Fire TV用のアプリやゲームを作成できます。

Fire TV開発用のUnityプラグインは提供されていませんが、ゲームコントローラをサポートするためのパッケージがUnity Asset Storeにあります。特に[Gallant GamesのInControl](http://www.gallantgames.com/incontrol)には、Amazonの開発者も大きな恩恵を受けています。InControlは、広く利用されている各種コントローラの制御マッピングを標準化する、Unity3Dのクロスプラットフォーム入力マネージャーです。

Unity入力マネージャーを使って、ゲームのコントローラ入力を設定することもできます。Amazon Fire TVのリモコン/ゲームコントローラのボタンとUnity入力マネージャーのボタン/軸との対応関係については、以下の表を参照してください。

{% include note.html content="このドキュメントの入力リファレンスはUnity 4.3.x以降に適用されますが、今後のUnityのリリースで変更される可能性があります。" %}

* TOC
{:toc}

## リモート入力

Unityの以下の値を使用して、Amazon Fire TV RemoteとAmazon Fire TV Voice Remoteの両方でボタンをマップします。Unity KeyCodeの値の詳細については、「<a href="http://docs.unity3d.com/ScriptReference/KeyCode.html">KeyCode</a>」を参照してください。

{% include image.html title="Unityリモコン" file="firetv/getting_started/images/remote-unity" type="png" %}

<table class="grid">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
  <thead>
    <tr>
      <th>ボタン</th>
      <th>Unity入力マネージャーの値</th>
      <th>Unity KeyCodeの値</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>1 (システムイベント) </td>
      <td>なし (システムイベント)</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>なし (非サポート)</td>
      <td><code>KeyCode.Escape</code></td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>なし (非サポート)</td>
      <td><code>KeyCode.Menu</code></td>
    </tr>
    <tr>
      <td>Microphone (検索)</td>
      <td>なし (システムイベント)</td>
      <td>なし (システムイベント)</td>
    </tr>
    <tr>
      <td>Select (D-Padの [Center])</td>
      <td>ジョイスティックボタン 0</td>
      <td><code>KeyCode.JoystickButton0</code></td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td>第 5 軸</td>
      <td><code>KeyCode.LeftArrow</code></td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td>第 5 軸</td>
      <td><code>KeyCode.RightArrow</code></td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td>第 6 軸</td>
      <td><code>KeyCode.UpArrow</code></td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td>第 6 軸</td>
      <td><code>KeyCode.DownArrow</code></td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
  </tbody>
</table>


## ゲームコントローラ入力

Unityの以下の値を使って、Amazon Fire Game Controllerでボタンをマップします。Unity KeyCodeの値の詳細については、「<a href="http://docs.unity3d.com/ScriptReference/KeyCode.html">KeyCode</a>」を参照してください。

{% include image.html title="ゲームコントローラUnity" file="firetv/getting_started/images/gamecontrollr-unity" type="png" %}

{% include image.html title="ゲームコントローラUnity" file="firetv/getting_started/images/gamecontrollr-unity-second-view" type="png" %}

<table class="grid">
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
  <thead>
    <tr>
      <th>ゲームコントローラボタン</th>
      <th>Unity入力マネージャーの値</th>
      <th>Unity KeyCodeの値</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>なし (システムイベント)</td>
      <td>なし (システムイベント)</td>
    </tr>
    <tr>
      <td>Back</td>
      <td>なし (システムイベント)</td>
      <td><code>KeyCode.Escape</code></td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>なし (システムイベント)</td>
      <td><code>KeyCode.Menu</code></td>
    </tr>
    <tr>
      <td>GameCircle</td>
      <td>なし (システムイベント)</td>
      <td>なし (システムイベント)</td>
    </tr>
    <tr>
      <td>A</td>
      <td>ジョイスティックボタン 0</td>
      <td><code>KeyCode.JoystickButton0</code></td>
    </tr>
    <tr>
      <td>B</td>
      <td>ジョイスティックボタン 1</td>
      <td><code>KeyCode.JoystickButton1</code></td>
    </tr>
    <tr>
      <td>X</td>
      <td>ジョイスティックボタン 2 </td>
      <td><code>KeyCode.JoystickButton2</code></td>
    </tr>
    <tr>
      <td>Y</td>
      <td>ジョイスティックボタン 3</td>
      <td><code>KeyCode.JoystickButton3</code></td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td>第 5 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td>第 5 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td>第 6 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td>第 6 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>左側のジョイスティック (左/右)</td>
      <td>X軸 第 1 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>左側のジョイスティック (上/下)</td>
      <td>Y軸 第 2 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>左側のジョイスティックの押し下げ</td>
      <td>ジョイスティックボタン 8</td>
      <td><code>KeyCode.JoystickButton8</code></td>
    </tr>
    <tr>
      <td>右側のジョイスティック (左/右)</td>
      <td>第 3 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>右側のジョイスティック (上/下)</td>
      <td>第 4 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>右側のジョイスティックの押し下げ</td>
      <td>ジョイスティックボタン 9</td>
      <td><code>KeyCode.JoystickButton9</code></td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td>なし (非サポート)</td>
      <td>なし (非サポート)</td>
    </tr>
    <tr>
      <td>左側のトリガー (L2) </td>
      <td>第 13 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>左側のショルダー (L1)</td>
      <td>ジョイスティックボタン 4</td>
      <td><code>KeyCode.LeftShift KeyCode.JoystickButton4</code></td>
    </tr>
    <tr>
      <td>右側のトリガー (R2)</td>
      <td>第 12 軸</td>
      <td>なし</td>
    </tr>
    <tr>
      <td>右側のショルダー (R1)</td>
      <td>ジョイスティックボタン 5</td>
      <td><code>KeyCode.RightShift KeyCode.JoystickButton5</code></td>
    </tr>
  </tbody>
</table>


## コントローラの名前

Unityでは、[`Input.GetJoystickNames()`](http://docs.unity3d.com/ScriptReference/Input.GetJoystickNames.html) メソッドでコントローラの名前を使用できます。各コントローラに使用する値は次の通りです。

*   Remote: `"Amazon Fire TV Remote"`
*   Voice Remote: `"Amazon Fire TV Remote"`
*   Game controller: `"Amazon Fire Game Controller"`

{% include links.html %}
