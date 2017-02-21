---
title: ゲームコントローラ入力
permalink: amazon-fire-game-controller-input.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Amazon Fire TVゲームコントローラ (Bluetooth HIDゲームパッドプロファイルに準拠したコントローラ全般) には、各種ボタン、Androidモーション、キーイベント定数があります。アプリ内で発生した入力イベントは、これらの情報を使用してキャプチャします。

サポートされるすべてのコントローラにおけるボタンの動作に関するガイドラインについては、「[コントローラ動作のガイドライン][controller-behavior-guidelines]」を参照してください。Amazon Fire TVリモコンの入力を処理する方法の詳細については、「[Amazon Fire TVリモート入力][amazon-fire-tv-remote-input]」を参照してください。

* TOC
{:toc}

## ボタン

Amazon Fire TV (第 2 世代) ゲームコントローラには次のボタンがあります。

{% include image.html file="firetv/getting_started/images/gamecontroller2" type="png" title="New game controller" %}

Amazon Fire TV (第 1 世代) ゲームコントローラには次のボタンがあります。

{% include image.html file="firetv/getting_started/images/gamepad-callouts" type="png" title="Game controller" %}

{% include image.html file="firetv/getting_started/images/gamepad-callouts-second-view" type="png" title="Game controller, other buttons" %}

## 入力情報を取得する

Amazon Fire TVで使用されるゲームコントローラでは、デジタルボタン ([A] ボタンなど) を押すとAndroid [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)イベントが生成され、アナログコントロール (ジョイスティックなど) を動かすと[`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)イベントが生成されます。

単純なボタン入力は、標準のAndroidイベントリスナーインターフェースとコールバック ([`onClick()`](http://developer.android.com/reference/android/view/View.OnClickListener.html#onClick%28android.view.View%29)、[`onFocusChange()`](http://developer.android.com/reference/android/view/View.OnFocusChangeListener.html#onFocusChange%28android.view.View,%20boolean%29) など) を使用して処理できます。

[`View`](http://developer.android.com/reference/android/view/View.html)で特定のボタンが押されたことを示すイベントをキャプチャするには、[`onKeyDown()`](http://developer.android.com/reference/android/view/View.html#onKeyDown%28int,%20android.view.KeyEvent%29) などの入力イベントハンドラをオーバーライドします。

特定のキーをキャプチャするには、[`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)クラスの入力定数に該当するかどうかをテストします。たとえば、[A] ボタンが押されたことをキャプチャするには、次のコードを使用します。

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event){
    boolean handled = false;

    switch (keyCode){
        case KeyEvent.KEYCODE_BUTTON_A:
                // ... handle selections
                handled = true;
                break;
     }
     return handled || super.onKeyDown(keyCode, event);
}
```

モーションイベントをキャプチャするには、[`View`](http://developer.android.com/reference/android/view/View.html)の[`onGenericMotionEvent()`](http://developer.android.com/reference/android/view/View.html#onGenericMotionEvent(android.view.MotionEvent)) イベントをオーバーライドします。モーションイベントを生成したコントロールを特定するには[`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)クラスの入力定数を使用し、モーションイベントの値を特定するには[`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)クラスの[`getAxisValue()`](http://developer.android.com/reference/android/view/MotionEvent.html#getAxisValue(int)) などのイベントを使用します。

```java
@Override
public boolean onGenericMotionEvent(MotionEvent event){
    // get changes in value from left joystick
    float deltaX = event.getAxisValue(MotionEvent.AXIS_X);
    float deltaY = event.getAxisValue(MotionEvent.AXIS_Y);

    if (deltaX > 0.5 && deltaY > 0.5) {
        // do something
        handled = true;
    }

    return handled || super.onGenericMotionEvent(event);
}
```

## プライマリとセカンダリの入力イベント {#primary_and_secondary}

Fire TV端末の一部のゲームコントローラ操作では、1 つの操作に対して複数の入力イベントが生成されることがあります。たとえば、Amazon Fire TVゲームコントローラのD-Padは、モーションイベントを生成する*アナログ方向コントロール*ですが、Fire TVのリモコンのD-Padは、キーイベントを生成する*デジタルコントロール*です。

同様に、選択操作は、ゲームコントローラでは *[A] ボタン*ですが、Fire TVのリモコンでは*D-Padの [Center] ボタン*です。Amazon Fire TVの一部のゲームコントローラ操作では、最初にプライマリ入力イベント (通常はモーションイベント) が生成されます。これらのイベントがアプリによって処理されなければ、次に、セカンダリ入力イベント (通常はキーイベント) が生成されます。プライマリとセカンダリの両方の入力イベントを、次の「[入力イベントリファレンス](#inputreference)」の表に示します。

セカンダリ入力イベントを利用すると、ゲームコントローラ入力を処理するプロセスを単純化することができます。ゲームコントローラからのボタンイベントおよびD-Padイベントだけをアプリで処理する場合、セカンダリイベントを使用することで、モーションイベントをすべて無視し、キーイベントだけを対象にすることができます。

同様に、[A] ボタンは`KEYCODE_BUTTON_A`と`KEYCODE_DPAD_CENTER`の両方を生成するので、アプリがFire TVリモコンのD-Padの [Center] ボタンをサポートする場合、[A] ボタンもテストする必要はありません。

プライマリ入力イベントを適切に処理しないと、アプリが入力を二重に受信しているように動作する可能性があることに注意してください。イベントがキャプチャされて処理されたとき、入力イベントハンドラは`true`を返す必要があります。プライマリ入力イベントがキャプチャされた場合、セカンダリ入力イベントは生成されません。

## 入力イベントリファレンス {#inputreference}

次の表は、各ゲームコントローラボタンのモーションイベント定数およびキーイベント定数、それらのボタンでの推奨されるユーザーエクスペリエンス動作、およびAmazon Fire TVユーザーインターフェースでのそれらのボタンのデフォルトの動作を示します。

デジタルボタンはキーイベント ([`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)) を、アナログコントロールはモーションイベント ([`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)) を報告します。コントローラ入力に対するアプリの推奨される動作については、「[コントローラ動作のガイドライン][controller-behavior-guidelines]」を参照してください。

{% include note.html content="アプリで使用されないボタンの入力イベントを、アプリでキャプチャや破棄などの処理の対象にしないでください。システムでの未使用イベントの処理を可能にすると、メディアの再生や音量制御などのバックグラウンドの動作が可能になります。" %}

「`MotionEvent`」列または「`KeyEvent`」列に示されているプライマリイベントをアプリが処理しない場合、これらのイベントに**加えて**「セカンダリイベント」列のイベントも生成されます。これらのセカンダリイベントについては、前の「[プライマリとセカンダリの入力イベント](#primary_and_secondary)」を参照してください。

この表における*ゲーム*とは、ゲームカテゴリでAmazonアプリストアに申請され、Amazonアプリストアから端末にインストールされたアプリのことです。

<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
  <thead>
    <tr>
      <th>ゲームコントローラボタン</th>
      <th>MotionEvent</th>
      <th>KeyEvent</th>
      <th>セカンダリイベント</th>
      <th>デフォルトの動作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>なし</td>
      <td>なし</td>
      <td>なし</td>
      <td>GameCircleをサポートしているゲームの場合、GameCircleオーバーレイを起動します。GameCircleをサポートしていないゲームの場合、[Game Paused] ダイアログが表示されます。それ以外のアプリの場合、ホーム画面に戻ります。      </td>
    </tr>
    <tr>
      <td>Back</td>
      <td>なし</td>
      <td><code>KEYCODE_BACK</code></td>
      <td>なし</td>
      <td>前の操作または画面 (アクティビティ) に戻ります。</td>
    </tr>
    <tr>
      <td>Menu</td>
      <td>なし</td>
      <td><code>KEYCODE_MENU</code></td>
      <td>なし</td>
      <td>Androidコンテキストメニュー (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>) が起動されます。</td>
    </tr>
    <tr>
      <td>GameCircle (第 1 世代のFireゲームコントローラのみ)</td>
      <td>なし</td>
      <td><code>なし</code></td>
      <td>なし</td>
      <td>GameCircleをサポートしているゲームの場合、GameCircleオーバーレイを起動します。GameCircleをサポートしていないゲームの場合、[Game Paused] ダイアログが表示されます。それ以外のアプリの場合、ランチャーの [Games] 画面に戻ります。</td>
    </tr>
    <tr>
      <td>A</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_A</code></td>
      <td><code>KEYCODE_DPAD_CENTER</code></td>
      <td>フォーカスが現在置かれているアイテムが選択されます。</td>
    </tr>
    <tr>
      <td>B</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_B</code></td>
      <td><code>KEYCODE_BACK</code></td>
      <td>前の画面 (アクティビティ) に戻ります ([戻る] と同じ)。</td>
    </tr>
    <tr>
      <td>X</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_X</code></td>
      <td>なし</td>
      <td>何も起こりません。</td>
    </tr>
    <tr>
      <td>Y</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_Y</code></td>
      <td>なし</td>
      <td>何も起こりません。</td>
    </tr>
    <tr>
      <td>Left (D-PAD) Right (D-PAD)</td>
      <td><code>AXIS_HAT_X</code> (&gt;0 is right)</td>
      <td>なし</td>
      <td><code>KEYCODE_DPAD_LEFT</code> <code>KEYCODE_DPAD_RIGHT</code></td>
      <td>ユーザーインターフェースのフォーカスが左方向または右方向に移動します。</td>
    </tr>
    <tr>
      <td>Up (D-PAD) Down (D-PAD)</td>
      <td><code>AXIS_HAT_Y</code> (&gt;0 is down)</td>
      <td>なし</td>
      <td><code>KEYCODE_DPAD_UP</code> <code>KEYCODE_DPAD_DOWN</code></td>
      <td>ユーザーインターフェースのフォーカスが上方向または下方向に移動します。</td>
    </tr>
    <tr>
      <td>左側のジョイスティック (左/右)</td>
      <td><code>AXIS_X</code> (正の値は右方向を表します)</td>
      <td>なし</td>
      <td><code>KEYCODE_DPAD_LEFT</code> <code>KEYCODE_DPAD_RIGHT</code>(移動が 0.5 を超える場合)</td>
      <td>ユーザーインターフェースのフォーカスが入力された方向に移動します。</td>
    </tr>
    <tr>
      <td>左側のジョイスティック (上/下)</td>
      <td><code>AXIS_Y</code> (正の値は下方向を表します)</td>
      <td>なし</td>
      <td><code>KEYCODE_DPAD_UP</code> <code>KEYCODE_DPAD_DOWN</code> (移動が 0.5 を超える場合)</td>
      <td>ユーザーインターフェースのフォーカスが入力された方向に移動します。</td>
    </tr>
    <tr>
      <td>左側のジョイスティックの押し下げ</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_THUMBL</code></td>
      <td>なし</td>
      <td>再生/一時停止</td>
    </tr>
    <tr>
      <td>右側のジョイスティック (左/右)</td>
      <td><code>AXIS_Z</code> (正の値は右方向を表します)</td>
      <td>なし</td>
      <td>なし</td>
      <td>何も起こりません。</td>
    </tr>
    <tr>
      <td>右側のジョイスティック (上/下)</td>
      <td><code>AXIS_RZ</code> (正の値は下方向を表します)</td>
      <td>なし</td>
      <td>なし</td>
      <td>何も起こりません。</td>
    </tr>
    <tr>
      <td>右側のジョイスティックの押し下げ</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_THUMBR</code></td>
      <td>なし</td>
      <td>再生/一時停止</td>
    </tr>
    <tr>
      <td>Play/Pause (第 1 世代のFireゲームコントローラのみ)</td>
      <td>なし</td>
      <td><code>KEYCODE_MEDIA_PLAY_PAUSE</code></td>
      <td>なし</td>
      <td>再生/一時停止</td>
    </tr>
    <tr>
      <td>Rewind (第 1 世代のFireゲームコントローラのみ)</td>
      <td>なし</td>
      <td><code>KEYCODE_MEDIA_REWIND</code></td>
      <td>なし</td>
      <td>早戻し</td>
    </tr>
    <tr>
      <td>Fast Forward (第 1 世代のFireゲームコントローラのみ)</td>
      <td>なし</td>
      <td><code>KEYCODE_MEDIA_FAST_FORWARD</code></td>
      <td>なし</td>
      <td>早送り</td>
    </tr>
    <tr>
      <td>左側のトリガー (L2)</td>
      <td><code>AXIS_BRAKE</code></td>
      <td>なし</td>
      <td>なし</td>
      <td>音量 +</td>
    </tr>
    <tr>
      <td>左側のショルダー (L1)</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_L1</code></td>
      <td>なし</td>
      <td>早戻し</td>
    </tr>
    <tr>
      <td>右側のトリガー (R2)</td>
      <td><code>AXIS_GAS</code></td>
      <td>なし</td>
      <td>なし</td>
      <td>音量 -</td>
    </tr>
    <tr>
      <td>右側のショルダー (R1)</td>
      <td>なし</td>
      <td><code>KEYCODE_BUTTON_R1</code></td>
      <td>なし</td>
      <td>早送り</td>
    </tr>
  </tbody>
</table>


{% include links.html %}
