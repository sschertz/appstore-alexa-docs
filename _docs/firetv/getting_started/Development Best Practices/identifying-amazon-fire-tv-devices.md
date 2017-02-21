---
title: Identifying Amazon Fire TV Devices
permalink: identifying-amazon-fire-tv-devices.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

You can identify Amazon Fire TV devices by looking for `amazon.hardware.fire_tv` as a feature.

The following table provides a list of features for different Fire TV devices:

<style>
td.center {
text-align: center;
 }
</style>
 
<table class="grid">
<colgroup>
  <col width="40%" />
  <col width="15%" />
  <col width="15%" />
  <col width="15%" />
  <col width="15%" />
</colgroup>
<thead>
<tr>
  <th>Feature</th>
  <th>Fire TV Stick <br/>(Gen 2)</th>
  <th>Fire TV <br/>(Gen 2)</th>
  <th>Fire TV Stick <br/>(Gen 1)</th>
  <th>Fire TV <br/>(Gen 1)</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>amazon.hardware.fire_tv</code></td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
</tr>
<tr>
  <td><code>amazon.hardware.low_power</code></td>
  <td class="center"> ✓ </td>
  <td class="center"> </td>
  <td class="center"> ✓ </td>
   <td class="center"> </td>
</tr>
<tr>
  <td><code>amazon.hardware.uhd_capable</code></td>
  <td class="center"></td>
  <td class="center"> ✓ </td>
  <td class="center"> </td>
  <td class="center"> </td>
   
</tr>
<tr>
  <td><code>amazon.software.drm_teardown</code></td>
  <td class="center"></td>
  <td class="center"></td>
  <td class="center"></td>
   <td class="center"> ✓ </td>
</tr>
<tr>
<td><code>android.hardware.type.television</code></td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
  <td class="center"> ✓ </td>
</tr>
</tbody>
</table>

In all cases, Fire TV devices can be identified through the feature `amazon.hardware.fire_tv`.

You get the feature by calling the [`getPackageManager()`][1] method on the [`Context`][2] object and checking whether [`hasSystemFeature()`][3] returns `com.hardware.amazon.fire_tv`. The following code shows a sample:

```java
final String AMAZON_FEATURE_FIRE_TV = "amazon.hardware.fire_tv";

if (getPackageManager().hasSystemFeature(AMAZON_FEATURE_FIRE_TV)) {
 Log.v(TAG, "Yes, this is a Fire TV device.");
 } else {
  Log.v(TAG, "No, this is not a Fire TV device.");
 }
```

## Reasons for Checking the Fire TV Device

You might want to check for the Fire TV device in your code for a number of reasons:

*  To identify Amazon Fire TV for choosing strings or making special offers (like free Plex service).
*  To determine whether the app needs to tear down the DRM and HW decoding pipeline in their `onPause()` method (which is needed for Fire TV &ndash; 1st Generation and Fire TV Stick due to poor handling of multiple DRM contexts).
*  To check whether the TV platform requires a D-pad controller to drive the UI.
*  To determine whether the app should play 4k Ultra HD.

## Checking for the Model

Previously, you could identify Fire TV devices by combining the `android.os.Build.MODEL` with the `Build.MANUFACTURER`. As more Amazon-powered devices come to market, this approach will no longer work. 

If you absolutely have to switch behavior based on a specific Amazon product model, you may use the `MODEL` name.  Be aware that such code will not likely work on future devices. As such, include a sensible fallback approach for future Fire TV devices based on the `com.hardware.amazon.fire_tv` feature.

The [`android.os.Build.MODEL`][4] value for Fire TV devices is as follows:

<table class="grid">
<colgroup>
  <col width="20%" />
  <col width="20%" />
  <col width="20%" />
  <col width="20%" />
</colgroup>
<thead>
<tr>
  <th>Fire TV Stick (Gen 2)</th>
  <th>Fire TV (Gen 2)</th>
  <th>Fire TV Stick (Gen 1)</th>
  <th>Fire TV (Gen 1)</th>
</tr>
</thead>
<tbody>
<tr>
  <td><code>AFTT</code></td>
  <td><code>AFTS</code></td>
  <td><code>AFTM</code></td>
  <td><code>AFTB</code></td>
</tr>
</tbody>
</table>

Here's a code sample that looks for the model but falls back on the feature:

```java
final String AMAZON_FEATURE_FIRE_TV = "amazon.hardware.fire_tv";
String AMAZON_MODEL = Build.MODEL;

if (AMAZON_MODEL.matches("AFTS")) {
 Log.v(TAG, "Yes, this is a Fire TV Gen 2 device.");
} else if (getPackageManager().hasSystemFeature(AMAZON_FEATURE_FIRE_TV)) {
 Log.v(TAG, "Yes, this is a Fire TV device.");
} else {
 Log.v(TAG, "No, this is not a Fire TV device");
}
```

[1]: https://developer.android.com/reference/android/content/Context.html#getPackageManager()
[2]: https://developer.android.com/reference/android/content/Context.html
[3]: https://developer.android.com/reference/android/content/pm/PackageManager.html#hasSystemFeature(java.lang.String)
[4]: https://developer.android.com/reference/android/os/Build.html#MODEL