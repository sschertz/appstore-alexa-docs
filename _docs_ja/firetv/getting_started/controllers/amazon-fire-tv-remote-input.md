---
title: リモート入力
permalink: amazon-fire-tv-remote-input.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

すべてのボタン、Androidイベント、および動作のガイドラインは、[Voice Search] ([Microphone]) ボタンを除いて、すべてのリモコンで同じです。[Voice Search] ([Microphone]) ボタンは一部のリモコンでしか利用できません。

サポートされるすべてのコントローラにおけるボタンの動作に関する推奨ガイドラインについては、「[コントローラ動作のガイドライン][controller-behavior-guidelines]」を参照してください。Amazon Fireゲームコントローラからの入力を処理する方法の詳細については、「[Amazon Fire TVゲームコントローラ入力][amazon-fire-game-controller-input]」を参照してください。

* TOC
{:toc}

## ボタン

ほとんどのAmazon Fire TVリモコンには次のボタンがあります。一部のFire TVのリモコンには [Microphone] ボタンまたは [Voice Search] ボタンが付属していません。

{% include image.html file="firetv/getting_started/images/remote-callouts" type="png" alt="Remote control" caption="Remote control" %}


## 入力情報を取得する

Android入力機器と同じように、すべてのAmazon Fire TVのリモコンは、ボタンが押されると、[`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)イベントを生成します。コントローラボタン入力は、標準のAndroidイベントリスナーインターフェースとコールバック ([`onClick()`](http://developer.android.com/reference/android/view/View.OnClickListener.html#onClick%28android.view.View%29)、[`onFocusChange()`](http://developer.android.com/reference/android/view/View.OnFocusChangeListener.html#onFocusChange%28android.view.View,%20boolean%29) など) を使用して処理できます。Amazon Fire TV RemoteとVoice Remoteのいずれも、(Android [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)クラスからの) モーションイベントを生成しません。

[`View`](http://developer.android.com/reference/android/view/View.html)で特定のボタンが押されたことを示すイベントをキャプチャするには、[`onKeyDown()`](http://developer.android.com/reference/android/view/View.html#onKeyDown%28int,%20android.view.KeyEvent%29) などの入力イベントハンドラをオーバーライドします。特定のキーをキャプチャするには、[`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)クラスの入力定数に該当するかどうかをテストします。

たとえば、[Left]、[Right]、および [Center] のD-Padボタンをキャプチャするには、次のコードを使用します (ゲームコントローラの [A] ボタンに対しても同様です)。

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event){
boolean handled = false;

switch (keyCode){
case KeyEvent.KEYCODE_DPAD_CENTER:
case KeyEvent.KEYCODE_BUTTON_A:
        // ... handle selections
        handled = true;
        break;
case KeyEvent.KEYCODE_DPAD_LEFT:
        // ... handle left action
        handled = true;
        break;
case KeyEvent.KEYCODE_DPAD_RIGHT:
        // ... handle right action
        handled = true;
        break;
}
return handled || super.onKeyDown(keyCode, event);
}
```

すべての入力イベントに関して、リスナーメソッドは、イベントをキャプチャして処理するために`true`を返すか、他のコントロールがそのイベントを管理できるようにイベントを`super.onKeyDown()` に渡す必要があります。

## 入力イベントリファレンス

次の表に、ボタンとそのAndroid [`KeyEvent`](http://developer.android.com/reference/android/view/KeyEvent.html)定数、各ボタンのデフォルトの動作を示します。Amazon Fire TVのリモコンはいずれも、(Android [`MotionEvent`](http://developer.android.com/reference/android/view/MotionEvent.html)クラスからの) モーションイベントを生成しません。

アプリが特定の入力イベントをキャプチャしないと、デフォルトの動作が発生します。

<table class="grid">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>ボタン</th>
      <th>KeyEvent</th>
      <th>デフォルトの動作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Home</td>
      <td>なし</td>
      <td>[Home] 画面に戻ります。これはシステムイベントで、インターセプトできません。</td>
    </tr>
    <tr>
      <td>Back</td>
      <td><code>KEYCODE_BACK</code></td>
      <td>前の操作または画面 (アクティビティ) に戻ります。</td>
    </tr>
    <tr>
      <td>Menu</td>
      <td><code>KEYCODE_MENU</code></td>
      <td>Androidコンテキストメニュー (<a href="http://developer.android.com/guide/topics/ui/menus.html#options-menu">OptionsMenu</a>) が起動されます。</td>
    </tr>
    <tr>
      <td>Microphone (検索) (Voice Remoteのみ)</td>
      <td>なし</td>
      <td>システムの音声検索が起動されます。これはシステムイベントで、インターセプトできません。</td>
    </tr>
    <tr>
      <td>Select (D-Padの [Center])</td>
      <td><code>KEYCODE_DPAD_CENTER</code></td>
      <td>現在フォーカスが置かれているユーザーインターフェースアイテムが選択されます。</td>
    </tr>
    <tr>
      <td>Up (D-Pad)</td>
      <td><code>KEYCODE_DPAD_UP</code></td>
      <td>ユーザーインターフェース内でフォーカスが上方向に移動します。</td>
    </tr>
    <tr>
      <td>Down (D-Pad)</td>
      <td><code>KEYCODE_DPAD_DOWN</code></td>
      <td>ユーザーインターフェース内でフォーカスが下方向に移動します。</td>
    </tr>
    <tr>
      <td>Left (D-Pad)</td>
      <td><code>KEYCODE_DPAD_LEFT</code></td>
      <td>ユーザーインターフェース内でフォーカスが左方向に移動します。</td>
    </tr>
    <tr>
      <td>Right (D-Pad)</td>
      <td><code>KEYCODE_DPAD_RIGHT</code></td>
      <td>ユーザーインターフェース内でフォーカスが右方向に移動します。</td>
    </tr>
    <tr>
      <td>Play/Pause</td>
      <td><code>KEYCODE_MEDIA_PLAY_PAUSE</code></td>
      <td>メディアが再生または一時停止されます。再生と一時停止は、相互に切り替わります。</td>
    </tr>
    <tr>
      <td>Rewind</td>
      <td><code>KEYCODE_MEDIA_REWIND</code></td>
      <td>再生中のメディアコンテキストが早戻しされます。</td>
    </tr>
    <tr>
      <td>Fast Forward</td>
      <td><code>KEYCODE_MEDIA_FAST_FORWARD</code></td>
      <td>再生中のメディアコンテキストが早送りされます。</td>
    </tr>
  </tbody>
</table>

{% include links.html %}
