---
title:  Freewheel広告コンポーネント
permalink: fire-app-builder-freewheel-ads-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire App BuilderでビルドするアプリにFreeWheelのビデオ広告を実装できます。Freewheelの詳細については、[Freewheel.tv](http://freewheel.tv/)を参照してください。Fire App Builderでは、プレロール広告とミッドロール広告の両方がサポートされています。

* TOC
{:toc}

## ユーザーエクスペリエンス

[Content Renderer] 画面でメディアの再生が開始される前に、Freewheelのビデオ広告が再生されます。

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_freewheeladdisplay" type="png" caption="FreeWheelの広告が表示されます (このスクリーンショットでは、フィラー広告トラックが表示されています)。" %}

ビデオ広告が終了すると、ユーザーが選択したメディアの再生が開始されます。

## Freewheelを構成する

1.  Freewheelコンポーネントをアプリにロードします。コンポーネントをアプリにロードする方法の詳細については、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」を参照してください。

2.  アプリにロードされている他の広告コンポーネント (VastAdsComponentやPassThroughAdsComponentなど) を削除します。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。    
    
    {% include_relative componentnote_ads.html %}
    
2.  **FreeWheelAdsComponent** > **java** > **com.amazon.ads.android.freewheel**の順に移動し、**FreeWheelAds.java**ファイルを開きます。

    {% include note.html content="他のコンポーネントとは異なり、FreeWheelAdsComponentでカスタマイズする必要がある値はXMLファイルに抽出されません (抽出は今後のリリースで可能になる予定です)。"%}

3.  次の 4 つの文字列の値をカスタマイズします。

    ```xml
    /**
     * FreeWheel server url.
    */
    private String mAdUrl = "http://demo.v.fwmrm.net/";

    /**
     * FreeWheel network id.
    */
    private int mNetworkId = 90750;

    /**
     * FreeWheel profile.
    */
    private String mProfile = "3pqa_android";

    /**
     * FreeWheel site section.
    */
    private String mSiteSectionId = "3pqa_section_nocbp";
    ```

   FreeWheelアカウントでこれらの値を取得します。

{% include links.html %}
