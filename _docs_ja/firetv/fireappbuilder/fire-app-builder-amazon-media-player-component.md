---
title: Amazon Media Playerコンポーネント
permalink: fire-app-builder-amazon-media-player-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Amazon Media Playerは、GoogleがAndroid向けに開発したオープンソースの[ExoPlayer](http://google.github.io/ExoPlayer/)を基盤としています。ExoPlayerをベースに構築されたAmazon Media Playerは、Fire TV端末と互換性のある堅牢で安定性に優れたメディアプレーヤーです。

Amazon Media PlayerにはExoPlayerの機能性が忠実に反映されています。たとえば、SmoothStreaming、DASH、HLS、Dolby、キャプションなどをサポートしています。Amazon Media Playerがサポートしている機能の詳細については、ExoPlayerのドキュメントをご覧ください。Android SDKの最小要件は、APIレベル 19 (KitKat) です。

Amazon Media Playerが開発された目的は、主要な再生シナリオ向けに複雑なExoPlayer APIを取り除いた上位レベルのプレーヤーAPIを提供することにあります。Amazon Media Playerが提供するAPIセットは一部を除いて固定されており、ExoPlayerのAPIにバージョンアップグレードによる変更が加えられた場合でも、開発者はこのAPIセットを続けて使用できます。

## Amazon Media Playerコンポーネントを構成する

Amazon Media Playerコンポーネントをカスタマイズするためのオプションはありません。「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」に記載された一般的な手順に従ってコンポーネントをロードする以外に、操作は必要ありません。

ただし、別のメディアプレーヤーコンポーネントがすでにロードされている場合は、それを削除する必要があります。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。

{% include_relative componentnote_mediaplayer.html %}

{% include links.html %}
