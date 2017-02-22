---
title: よくある質問
permalink: fire-app-builder-questions-and-answers.html
toc: false
github: true
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
github: true
---

Fire App Builderに関する一般的な質問を以下に示します。 

Fire App Builderで作成されたアプリは、スマートフォン端末とタブレットでも動作しますか。それともテレビでしか動作しませんか。
:   現時点では、Fire App Builderで作成されたアプリは、Fire TVと他のAndroid TVプラットフォームでのみ動作します。今後の機能拡張により、Fire App Builderで作成されたアプリは他の端末でも動作するようになる予定です。ツールキットの名前が「Fire TV App Builder」ではなく「Fire App Builder」であるのはこのためです。

ユーザーはFire TVを使用するために、どのような種類のテレビを持っている必要がありますか。
:   HDMIポートが付いたテレビが必要です。

Fire App Builderで作成したアプリは、Amazonのアプリストアだけでなく、Google Playストアにも申請できますか。
:   はい。どちらのストアでもAndroidアプリが表示されるため、Fire App Builderで作成したアプリはAmazonアプリストアとGoogle Playストアの両方で機能します。

Google Adsを実装するにはどうすればよいですか。
:   Google Adsの実装は、Google Adsをサポートしている[VASTコンポーネント][fire-app-builder-vast-ads-component]を使用して行います。

Fire App Builderはライブフィードをサポートしていますか。
:   はい。フィードから、どのようなメディアでも表示することができます。フィード内に有効なメディア (ライブまたは録画済みメディア) があれば、Fire App Builderで作成されたアプリで再生できます。詳細については、「[ライブストリーミングを構成する][fire-app-builder-live-stream-configuration]」を参照してください。

アプリを開発するためには、実際のFire TVデバイスが必要ですか。
:   開発とテストでは実際のデバイスを使用することを強く推奨しますが、[エミュレーターを使用する][fire-app-builder-use-an-android-tv-emulator]こともできます (一部制限あり)。

Fire App Builderとウェブアプリケーションスターターキット (WASK) の違いは何ですか。
:   Fire App Builderでは、ネイティブのJavaアプリを作成するためにAndroidとともにJavaが使用されるほか、IDEとしてAndroid Studioが使用されます。Fire App Builderは、(すべてをプログラミングすることなく) 堅牢なストリーミングメディアアプリを開発したい開発者向けです。WASKはモバイルウェブアプリツールキット (JavaではなくHTML、CSS、JavaScriptを使用) で、開発者向けというよりもコンテンツ製作者向けです。

{: #translator}
Fire App Builderがフィード内のオブジェクトをFire App Builderのコンテンツモデルにマップするときにトランスレーションではなくリフレクションを使用するには、どうすればよいですか。
:   デフォルトではリフレクションが使用されます。リフレクションを選択したい場合は、[カテゴリレシピ][fire-app-builder-set-up-recipes-categories]および[コンテンツレシピ][fire-app-builder-set-up-recipes-content]の`translator`パラメータを削除します。 
    
    さらに、レシピで`matchlist`パラメータを設定する際に、フィード要素名を少し異なる名前にマップする必要があります。`@title`、`@subtitle`、`@id`、`@description`、`@url`、`@cardImageUrl`、`@backgroundImageUrl`、および`@tags`を使用する代わりに、各単語の前に「m」を付けて、その後にキャメルケース形式を使用します。具体的には、`@mTitle`、`@mSubtitle`、`@mId`、`@description`、`@mUrl`、`@mCardImageUrl`、`@mBackgroundImageUrl`、および`@mTags`を使用します。

{% include links.html %}
