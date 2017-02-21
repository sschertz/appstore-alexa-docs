---
title: Display and Layout
permalink: display-and-layout.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Implementing an Android app that renders well on a television (or that behaves properly on both a tablet and a television) requires some attention to user interface layout.

This document provides technical information for building your UI layouts on the Amazon Fire TV platform. See [Design and User Experience Guidelines][design-and-user-experience-guidelines] for general guidelines on TV design.

* TOC
{:toc}

## Screen Size and Resolution

Many Android devices such as Fire tablets have a fixed physical size and a single resolution. This is not the case with Amazon Fire TV devices, to which you can connect a 720p or 1080p screen of any size.

The mechanism in Android to specify an activity layout in absolute coordinates independently of the video output resolution is to use density independent units (dp). Android scales the graphic resources so that the size remains constant independently of the screen resolution.

The following table shows the pixel size, density and display resolution for various video outputs connected to an Amazon Fire TV device.

<table class="grid">
   <colgroup>
      <col width="14%" />
      <col width="14%" />
      <col width="14%" />
      <col width="14%" />
      <col width="14%" />
      <col width="14%" />
      <col width="16%" />
   </colgroup>
  <thead>
    <tr>
      <th>TV setting</th>
      <th>Output resolution (pixels)</th>
      <th>Render surface (pixels)</th>
      <th>Density identifier</th>
      <th>screen density (dp)</th>
      <th>Display resolution (dp)</th>
      <th>Screen size identifier</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1080p</td>
      <td>1920 x 1080</td>
      <td>1920 x 1080</td>
      <td><code>xhdpi</code></td>
      <td>320</td>
      <td>960x540</td>
      <td><code>large</code></td>
    </tr>
    <tr>
      <td>720p</td>
      <td>1280 x 720</td>
      <td>1920 x 1080</td>
      <td><code>xhdpi</code></td>
      <td>320</td>
      <td>960x540</td>
      <td><code>large</code></td>
    </tr>
    <tr>
      <td>480p</td>
      <td>640 x 480</td>
      <td>1920 x 1080</td>
      <td><code>xhdpi</code></td>
      <td>320</td>
      <td>960x540</td>
      <td><code>large</code></td>
    </tr>
  </tbody>
</table>


## Orientation

The orientation of the Amazon Fire TV device never changes, and requests for the rotation or orientation on the device return these results:

<table class="grid">
   <colgroup>
      <col width="30%" />
      <col width="70%" />
   </colgroup>
  <thead>
    <tr>
      <th>Method</th>
      <th>Result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="http://developer.android.com/reference/android/view/Display.html#getRotation%28%29"><code>Display.getRotation()</code></a></td>
      <td>0 (<code>ROTATION_0</code>)</td>
    </tr>
    <tr>
      <td><a href="http://developer.android.com/reference/android/view/Display.html#getOrientation%28%29"><code>Display.getOrientation()</code></a> (deprecated)</td>
      <td>0 (<code>ORIENTATION_UNDEFINED</code>)</td>
    </tr>
  </tbody>
</table>

## Resource Configurations

If you design your app to run on platforms other than Amazon Fire TV, such as tablets, you can create different layouts and drawables for each platform, and store them in subdirectories of `res/` named for various platform and device configurations. For more information on using these resource configurations, see the Android best practices guide for [Supporting Multiple Screens](http://developer.android.com/guide/practices/screens_support.html).

The following table describes the resource configurations available for the Amazon Fire TV platform.

<table class="grid">
   <colgroup>
      <col width="30%" />
      <col width="70%" />
   </colgroup>
  <thead>
    <tr>
      <th>Configuration</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Smallest width</td>
      <td><code>sw540dp</code></td>
    </tr>
    <tr>
      <td>Available Width</td>
      <td><code>w960dp</code></td>
    </tr>
    <tr>
      <td>Available Height</td>
      <td><code>h540dp</code></td>
    </tr>
    <tr>
      <td>Screen Size</td>
      <td><code>large</code></td>
    </tr>
    <tr>
      <td>Screen aspect</td>
      <td><code>long</code></td>
    </tr>
    <tr>
      <td>Screen orientation</td>
      <td><code>land</code> (TV apps are always landscape)</td>
    </tr>
    <tr>
      <td>UI mode</td>
      <td><code>television</code></td>
    </tr>
    <tr>
      <td>Night mode</td>
      <td><code>notnight</code></td>
    </tr>
    <tr>
      <td>Screen pixel density</td>
      <td><code>xhdpi</code></td>
    </tr>
    <tr>
      <td>Touchscreen type</td>
      <td><code>notouch</code></td>
    </tr>
    <tr>
      <td>Keyboard availability</td>
      <td><code>keyssoft</code></td>
    </tr>
    <tr>
      <td>Primary text input method</td>
      <td><code>nokeys</code></td>
    </tr>
    <tr>
      <td>Navigation key availability</td>
      <td><code>navexposed</code></td>
    </tr>
    <tr>
      <td>Primary non-touch navigation method</td>
      <td><code>dpad</code></td>
    </tr>
    <tr>
      <td>Platform version</td>
      <td><code>v17</code></td>
    </tr>
  </tbody>
</table>

{% include links.html %}
