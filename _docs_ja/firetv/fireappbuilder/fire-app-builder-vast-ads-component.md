---
title: VAST広告コンポーネント
permalink: fire-app-builder-vast-ads-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

<style>
table.small {
max-width: 300px;
}
</style>


Fire App Builderは、VAST (Video Ad Serving Template) 広告テンプレートをサポートしています。VASTはさまざまなビデオ広告主をサポートしている標準的なプロトコルです。Interactive Advertising Bureau (IAB) では、VASTが次のように説明されています。

>VASTは、ビデオプレーヤーに広告を配信する広告タグを作成するためのビデオ広告配信テンプレートです。VASTは、XMLスキーマを使用して、広告に関する重要なメタデータを広告サーバーからビデオプレーヤーに転送します。2008 年に初めて公開されて以来、VASTはデジタルビデオ市場の成長に重要な役割を果たしてきました。「[Digital Video Ad Serving Template (VAST) 4.0](http://www.iab.com/guidelines/digital-video-ad-serving-template-vast-4-0/)」

アプリでDoubleClick広告を表示させたくない場合は、VAST広告コンポーネントを使用して非表示にできます。ただし、Fire App BuilderでサポートされるVASTの機能は一部だけです。

* TOC
{:toc}

## サポートされていないVAST機能

Fire App BuilderのVAST統合機能では、VASTの仕様全体がサポートされるわけではありません。また、Fire App BuilderのVAST統合機能は、IABによる正式な認定は受けていません。以下のVAST機能は、Fire App Builderではサポートされていません。

*  「リニアビデオ広告」以外の広告タイプ
*  広告内のインタラクション
*  広告内でのクリック、停止、再開、または閉じるアクション
*  302 リダイレクト

{% include note.html content="リニア広告とは、ビデオの再生前、再生中、または再生後に表示できる広告です。しかし、Fire App BuilderのVastAdsComponentでは、現時点ではビデオの再生前にしか表示できません (プレロール広告)。" %}

## トラッキングイベント

リニアビデオ広告の再生中、コンポーネントによって以下に関するイベントがトリガーされます。

*  インプレッション
*  エラー
*  開始 (25%、中間、75%、および完了)

コンポーネントによる各イベントのサポート状況を以下の表に示します。

<table class="grid">
  <thead>
    <tr>
      <th>トラッキングイベント</th>
      <th>サポート</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="highlighter-rouge">creativeView</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">start</code></td>
      <td>あり</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">midpoint</code></td>
      <td>あり</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">firstQuartile</code></td>
      <td>あり</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">thirdQuartile</code></td>
      <td>あり</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">complete</code></td>
      <td>あり</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">mute</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">unmute</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">pause</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">rewind</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">resume</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">fullscreen</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">expand</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">collapse</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">acceptInvitation</code></td>
      <td>なし</td>
    </tr>
    <tr>
      <td><code class="highlighter-rouge">close</code></td>
      <td>なし</td>
    </tr>
  </tbody>
</table>

## ユーザーエクスペリエンス

ユーザーが [Watch Now] ボタンをクリックすると、VAST広告テンプレートによって広告が短時間表示されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_vastadsdisplay" type="png" caption="VAST広告テンプレートでは、ユーザーによって選択されたメディアが再生を開始する前に、ビデオを表示します。このスクリーンショットは広告の代替画像です。VAST広告を設定すると、実際の広告に置き換えられます。" %}

ビデオ広告が終わると、ユーザーが選択したメディアの再生が開始されます。

## VAST広告を構成する

VAST広告を構成するには:

1.  VASTコンポーネントをアプリにロードします。アプリにコンポーネントをロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。
2.  アプリにロードされている他の広告コンポーネント (FreeWheelAdsCompnentやPassThroughAdsComponentなど) があれば削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。
    
    {% include_relative componentnote_ads.html %}
    
2.  **VastAdsComponent > res > values**に移動し、**strings.xml**を開きます。`vast_preroll_tag`文字列をコピーし、アプリの**custom.xml**ファイル (res > valuesにあります) にコピーします。
    
    ```xml
    <string name="vast_preroll_tag">"https://pubads.g.doubleclick.net/gampad/ads?sz=640x480&amp;iu=/124319096/external/single_ad_samples&amp;ciu_szs=300x250&amp;impl=s&amp;gdfp_req=1&amp;env=vp&amp;output=vast&amp;unviewed_position_start=1&amp;cust_params=deployment%3Ddevsite%26sample_ct%3Dlinear&amp;correlator="</string>
    ```
    
3.  独自のVAST広告タグを使用して`vast_preroll_tag`文字列の値をカスタマイズします。

    使用するVASTタグ内では、以下の文字をエンコードする必要があることに注意してください。

    <table class="small">
      <thead>
        <tr>
          <th>文字</th>
          <th>文字コード</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code class="highlighter-rouge">"</code></td>
          <td><code class="highlighter-rouge">&amp;quot;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">'</code></td>
          <td><code class="highlighter-rouge">&amp;apos;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&lt;</code></td>
          <td><code class="highlighter-rouge">&amp;lt;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&gt;</code></td>
          <td><code class="highlighter-rouge">&amp;gt;</code></td>
        </tr>
        <tr>
          <td><code class="highlighter-rouge">&amp;</code></td>
          <td><code class="highlighter-rouge">&amp;amp;</code></td>
        </tr>
      </tbody>
    </table>

{% include links.html %}

