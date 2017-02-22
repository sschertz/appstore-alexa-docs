---
title: Fire TV端末の仕様
permalink: device-and-platform-specifications.html
toc: false
navtabs: true
sidebar: firetv_ja
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

このページでは、すべてのAmazon Fire TV端末を対象に、メディアと端末、プラットフォームの仕様、サポートされるテクノロジーを紹介しています。

<ul id="profileTabs" class="nav nav-tabs">
   <li class="active"><a class="noCrossRef" href="#firetvstickgen2" data-toggle="tab">Fire TV Stick (第 2 世代)</a></li>
    <li><a class="noCrossRef" href="#firetvgen2" data-toggle="tab">Fire TV (Gen. 2)</a></li>
    <li><a class="noCrossRef" href="#firetvstickgen1" data-toggle="tab">Fire TV Stick (第 1 世代)</a></li>
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

## すべてのFire TV端末

### リモコンとゲームコントローラ

{% include_relative specs_remotes.md %}

### Fire TVでサポートされるテクノロジー

サポートされるテクノロジーは、すべてのAmazon Fire TV端末で同じです。

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="25%" />
   </colgroup>
  <thead>
    <tr>
      <th>テクノロジー</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Miracast</td>
      <td>サポートされます (Sink)。</td>
    </tr>
    <tr>
      <td>DIAL</td>
      <td>サポートされます。Fire TVのアプリを検出できるようにするには、Androidマニフェストを変更する必要があります。詳細については、「<a href="dial-integration.html">DIALの統合</a>」を参照してください。</td>
    </tr>
    <tr>
      <td>ウェブサイトの閲覧/外部リンク</td>
      <td>ウェブブラウザは使用できません。Android <a href="http://developer.android.com/reference/android/webkit/WebView.html">WebView</a>を使用します。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/in-app-purchasing">Amazonアプリ内課金</a></td>
      <td>サポートされます。最新バージョンの<a href="https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/installing-and-configuring-app-tester">App Tester</a>を使用します。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/mobile-associates">Amazonモバイルアソシエイト</a></td>
      <td>サポートされません。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/engage/gamecircle">Amazon GameCircle</a></td>
      <td>サポートされます。バージョン 2.1 以降を使用します。</td>
    </tr>
    <tr>
      <td><a href="http://login.amazon.com/">Login with Amazon</a></td>
      <td>サポートされます。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/experience/maps">Amazon Maps</a></td>
      <td>サポートされません。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/engage/device-messaging">Amazon Device Messaging</a></td>
      <td>プッシュメッセージをサポートします。</td>
    </tr>
    <tr>
      <td><a href="https://developer.amazon.com/public/apis/earn/mobile-ads">Amazonモバイル広告</a></td>
      <td>サポートされません。</td>
    </tr>
  </tbody>
</table>

### 端末のリリース日

<table class="grid">
   <colgroup>
      <col width="40%" />
      <col width="60%" />
   </colgroup>
  <thead>
    <tr>
      <th>端末</th>
      <th>リリース日</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Fire TV Stick (第 2 世代) </td>
      <td>2016 年 9 月 </td>
    </tr>
    <tr>
      <td>Fire TV (第 2 世代)  </td>
      <td>2015 年 12 月</td>
    </tr>
    <tr>
      <td>Fire TV Stick (第 1 世代) </td>
      <td> 2014 年 11 月</td>
    </tr>
    <tr>
      <td>Fire TV (第 1 世代)</td>
      <td>2014 年 4 月</td>
    </tr>
  </tbody>
</table>

{% include links.html %}
