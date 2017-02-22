---
title: コントローラの識別
permalink: identifying-controllers.html
sidebar: firetv_ja
product: Fire TV
toc: false
github: true
---

Amazon Fire TVプラットフォームでは、ユーザーは最大で 7 つのBluetoothコントローラに同時に接続できます。アプリまたはゲームが複数のユーザーやプレーヤーからの入力をサポートしている場合、接続されたコントローラを識別し、それらのコントローラの機能を特定し、それぞれのコントローラからのユーザー入力を区別できる必要があります。

Fire TVプラットフォームでは、Androidの標準機能である[`InputDevice`][1]クラスが使用されます。`InputDevice`クラスを使用することで、接続されたすべての入力端末 (コントローラを含む) のリストを入手し、入力端末の機能を問い合わせることができます。

また、任意のキーイベントやモーションイベントから入力端末オブジェクトを入手できることから、異なるコントローラやユーザーからの入力が発生した場合に、それらを処理することができます。

* TOC
{:toc}

## 入力端末と端末IDの取得

Amazon Fire TVに接続された入力端末は、Android [`InputDevice`][1]クラスによって表されます。接続された入力端末は、システムのブート時、および新たに端末が追加された場合に、端末IDを割り当てられます。IDのリストにある入力端末は、ゲームコントローラなどの実際のコントローラである場合と、オンスクリーンキーボードなど、他の入力形式を表している場合があります。端末IDは無作為に割り当てられるもので、これによって特定のコントローラやコントローラ種類を一意に識別することはできません。

利用可能なすべての端末IDのリストを取得することで、アプリやゲームが使用できるコントローラの番号と種類を確認できます。また、特定のキーイベントまたはモーションイベントから端末IDまたは入力端末オブジェクトを取得して、どのコントローラがそのイベントで使用されていたかを確認することもできます。 

### すべての入力端末IDの取得

以下のように、静的な[`InputDevice.getDeviceIds()`][3] メソッドを使って、利用可能なすべての入力端末IDの配列を取得し、[`InputDevice.getDevice()`][4] を使って、それらのIDを実際の入力端末オブジェクトと関連付けることができます。

```java
int[] ids = InputDevice.getDeviceIds();

for (int i=0;i < ids.length; i++) {
// get an InputDevice object based on the device ID
InputDevice device = InputDevice.getDevice(ids[i]);

// ...
}
```

`getDeviceIds()` メソッドで取得した端末IDのリストには、システムに実際に接続された入力端末のみが含まれることに留意してください。Bluetoothコントローラによっては、しばらくの間使用されていないと、電気を節約するために切断される (スリープ状態になる) 場合があります。また接続範囲外になると、切断される場合もあります。

スリープ状態のコントローラ、または他の状態で使用不可能なコントローラは、接続されているとみなされないため、端末IDのリストには列挙されません。[`InputDeviceListener`][5]インターフェースを実装することで、コントローラの接続イベントや切断イベントをリッスンできます。

### キーイベントまたはモーションイベントからの入力端末IDの取得

以下のように、[`InputEvent.getDeviceId()`][6] メソッドを使って、イベントハンドラ内でキーイベントまたはモーションイベントを起動した端末のIDを取得できます。

```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
int id = event.getDeviceId();
InputDevice device = InputDevice.getDevice(id);
}
```

または、以下のように入力端末オブジェクトそのものを取得します。

```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
InputDevice device = event.getDevice();
}
```

## コントローラ機能の特定

Androidの入力端末IDは無作為に割り当てられるもので、これによってコントローラやコントローラの種類を一意に識別することはできません。入力端末の種類を特定するには、その端末の機能を問い合わせ、コントローラ (リモコン、ゲームコントローラ、またはセカンドスクリーンアプリ) からの入力であるか、または他の端末からの入力であるかを判断します。

[`InputDevice.getSources()`][7] メソッドを使用して、入力端末の機能を取得します。このメソッドは、端末の機能を表す整数ビットマップを返します。[`InputDevice`][1]クラスで定義された定数を使用して、そのビットマップと、アプリに実装したい具体的な機能を比較します。

```java
// get an InputDevice object based on the device ID
InputDevice device = InputDevice.getDevice(id);

// getSources() gives you an integer bitmap that defines the features of the device;
// compare with constants from InputDevice to find controller features
if ((device.getSources() & InputDevice.SOURCE_CLASS_JOYSTICK != 0 {
// this controller has a joystick
}
```

以下のように、[`InputEvent.getSource()`][8] メソッドを使って、キーイベントやモーションイベントから入力端末機能のビットマップを直接取得することもできます。

```java
public boolean onGenericMotionEvent(MotionEvent event) {
// did this event come from a joystick?
if ((event.getSource() & InputDevice.SOURCE_CLASS_JOYSTICK) != 0) {
// ... handle input events from controllers with joysticks }
// ...
}
```

### 端末がゲームコントローラかどうかの判断

[`InputDevice.SOURCE_GAMEPAD`][9]と[`InputDevice.SOURCE_JOYSTICK`][10]の両方の定数をテストして、ゲームコントローラ (Amazonゲームコントローラと他のゲームコントローラの両方) を特定することができます。このコードでは、Amazonゲームコントローラと他社製のBluetoothコントローラが区別されないことに留意してください。

```java
int hasFlags = InputDevice.SOURCE_GAMEPAD | InputDevice.SOURCE_JOYSTICK;
boolean isGamepad = inputDevice.getSources() & hasFlags == hasFlags;
```

### 端末がリモコンかどうかの判断

[`InputDevice.SOURCE_DPAD`][11]定数を使用して、Amazon Fire TV RemoteまたはAmazon Fire TV Voice Remoteを特定できます。ただし、一部のキーボードはD-Padを搭載していると認識される場合もあるため、キーボードの種類もテストする必要があります (以下のように、[`InputDevice.KEYBOARD_TYPE_NON_ALPHABETIC`][12]を使用)。

```java
int hasFlags =  InputDevice.SOURCE_DPAD;
bool isRemote = (inputDevice.getSources() & hasFlags == hasFlags)
&& inputDevice.getKeyboardType() == InputDevice.KEYBOARD_TYPE_NON_ALPHABETIC;
```

### 端末がセカンドスクリーンアプリかどうかの判断

[`InputDevice.SOURCE_MOUSE`][13]と[`InputDevice.SOURCE_TOUCHPAD`][14]の両方の定数を使用して、Amazonセカンドスクリーンアプリを特定できます。

```java
int hasFlags =  InputDevice.SOURCE_MOUSE | InputDevice.SOURCE_TOUCHPAD;
bool isRemote = inputDevice.getSources() & hasFlags == hasFlags;
```

[1]: http://developer.android.com/reference/android/view/InputDevice.html
[3]: http://developer.android.com/reference/android/view/InputDevice.html#getDeviceIds%28%29
[4]: http://developer.android.com/reference/android/view/InputDevice.html#getDevice%28int%29
[5]: http://developer.android.com/reference/android/hardware/input/InputManager.InputDeviceListener.html
[6]: http://developer.android.com/reference/android/view/InputEvent.html#getDeviceId%28%29
[7]: http://developer.android.com/reference/android/view/InputDevice.html#getSources%28%29
[8]: http://developer.android.com/reference/android/view/InputEvent.html#getSource()
[9]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_GAMEPAD
[10]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_JOYSTICK
[11]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_DPAD
[12]: http://developer.android.com/reference/android/view/InputDevice.html#KEYBOARD_TYPE_ALPHABETIC
[13]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_MOUSE
[14]: http://developer.android.com/reference/android/view/InputDevice.html#SOURCE_TOUCHPAD

{% include links.html %}
