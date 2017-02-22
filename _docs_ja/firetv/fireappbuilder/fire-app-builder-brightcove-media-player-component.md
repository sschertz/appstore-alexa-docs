---
タイトル: Brightcove Media Playerコンポーネント
permalink: fire-app-builder-brightcove-media-player-component.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire App Builderには、デフォルトでAmazon Media Playerが構成されています。ただし、必要な場合は、代わりにBrightCove MediaPlayerを使用することもできます。この 2 つのメディアプレーヤーに機能的な違いはありません。メディアやアプリのインフラストラクチャがBrightCoveと密接に連携している場合は、Amazon Media Playerの代わりにBrightCove MediaPlayerをロードできます。

[Content Renderer] 画面の [Watch Now] ボタンをクリックすると、BrightCove Media Playerでコンテンツが再生されます。

## BrightCove Media Playerを構成する

BrightCove Media Playerコンポーネントをカスタマイズするオプションは用意されていません。このコンポーネントのロードは、「[アプリにコンポーネントをロードする][fire-app-builder-load-a-component]」に記載された一般的な手順に従って実行します。

多くの場合、アプリにはすでにAmazon Media Playerコンポーネント (AMZNMediaPlayerComponen) がロードされています。BrightCove Media Playerをロードするには、このコンポーネントをアプリから削除する必要があります。詳細については、「[コンポーネントを削除する][fire-app-builder-load-a-component#removeacomponent]」を参照してください。

{% include_relative componentnote_mediaplayer.html %}

{% include links.html %}
