---
title: Fire TV Device Specifications
permalink: device-and-platform-specifications.html
toc: false
navtabs: true
sidebar: firetv
product: Fire TV
toc: false
github: true
---

{% if site.target == "hippo" %}
<style> 
ul#profileTabs.nav.nav-tabs li { 
    margin: 5px; 
} 

ul#profileTabs.nav:after, ul#profileTabs.nav:before {
    display: inline-table !important;
    margin-bottom: 40px;
}

ul#profileTabs.nav-tabs>li.active>a, ul#profileTabs.nav-tabs>li.active>a:focus, ul#profileTabs.nav-tabs>li.active>a:hover {
font-weight: bold;
}
    
@media screen and (min-color-index:0) and(-webkit-min-device-pixel-ratio:0) 
{ @media {
ul#profileTabs.nav.nav:after, ul#profileTabs.nav.nav:before {
    display: inline-table !important;
    margin-bottom: 48px;
}
}}

table.grid {
margin-bottom: 30px;
}
</style>

<!--[if IE]>
<style>
    ul#profileTabs.nav:after, ul#profileTabs.nav:before {
        display: inline-table !important;
        margin-bottom: 48px;
    }
</style>
<![endif]-->

<style>
@media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
    ul#profileTabs.nav:after, ul#profileTabs.nav:before {
        display: inline-table !important;
        margin-bottom: 48px;
    }
}
</style>
{% endif %}

This page lists the media, device, and platform specifications for all Amazon Fire TV devices as well as supported technologies.

<ul id="profileTabs" class="nav nav-tabs">
   <li class="active"><a class="noCrossRef" href="#firetvstickgen2" data-toggle="tab">Fire TV Stick (Gen. 2)</a></li>
    <li><a class="noCrossRef" href="#firetvgen2" data-toggle="tab">Fire TV (Gen. 2)</a></li>
    <li><a class="noCrossRef" href="#firetvstickgen1" data-toggle="tab">Fire TV Stick (Gen. 1)</a></li>
    <li><a class="noCrossRef" href="#firetvgen1" data-toggle="tab">Fire TV (Gen. 1)</a></li>
</ul>

  <div class="tab-content">

<div role="tabpanel" class="tab-pane active" id="firetvstickgen2">

{% include_relative specs_firetvstickgen2.md %}
</div>
  
<div role="tabpanel" class="tab-pane" id="firetvgen2">
{% include_relative specs_firetvgen2.md %}
</div>


<div role="tabpanel" class="tab-pane" id="firetvstickgen1">
{% include_relative specs_firetvstickgen1.md %}
</div>

<div role="tabpanel" class="tab-pane" id="firetvgen1">
{% include_relative specs_firetvgen1.md %}
</div>
</div>

## All Fire TV Devices

### Remotes and Game Controllers

{% include_relative specs_remotes.md %}

### Technology Support for Fire TV

Supported technologies are the same for all Amazon Fire TV devices.

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="25%" />
   </colgroup>
  <thead>
    <tr>
      <th>Technology</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Miracast</td>
      <td>Supported (sink)</td>
    </tr>
    <tr>
      <td>DIAL</td>
      <td>Supported. Apps for Fire TV require changes to your app’s Android manifest to be discoverable. See <a href="dial-integration.html">DIAL integration</a>.</td>
    </tr>
    <tr>
      <td>Web Browsing/external links</td>
      <td>No web browser is available. Use Android <a href="http://developer.android.com/reference/android/webkit/WebView.html">WebView</a>.</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/in-app-purchasing">Amazon In-App Purchasing</a></td>
      <td>Supported. Use the latest version of the <a href="https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/installing-and-configuring-app-tester">App Tester</a>.</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/mobile-associates">Amazon Mobile Associates</a></td>
      <td>Not supported</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/engage/gamecircle">Amazon GameCircle</a></td>
      <td>Supported. Use version 2.1 or higher</td>
    </tr>
    <tr>
      <td><a href="http://login.amazon.com/">Login with Amazon</a></td>
      <td>Supported</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/experience/maps">Amazon Maps</a></td>
      <td>Not supported</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/engage/device-messaging">Amazon Device Messaging</a></td>
      <td>Supported for push messages</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/mobile-ads">Amazon Mobile Ads</a></td>
      <td>Not supported</td>
    </tr>
  </tbody>
</table>

### Device Release Dates

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="60%" />
   </colgroup>
  <thead>
    <tr>
      <th>Device</th>
      <th>Release Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Fire TV Stick (Generation 2) </td>
      <td>September 2016 </td>
    </tr>
    <tr>
      <td>Fire TV (Generation 2) </td>
      <td>December 2015</td>
    </tr>
    <tr>
      <td>Fire TV Stick (Generation 1) </td>
      <td> November 2014 </td>
    </tr>
    <tr>
      <td>Fire TV (Generation 1)</td>
      <td>April 2014 </td>
    </tr>
  </tbody>
</table>

{% include links.html %}
