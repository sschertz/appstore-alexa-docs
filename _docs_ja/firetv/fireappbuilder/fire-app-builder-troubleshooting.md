---
title: トラブルシューティング
permalink: fire-app-builder-troubleshooting.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

こちらを読む前に、発生したバグが既知のものかどうかを「[既知の問題][fire-app-builder-release-notes#known_issues]」セクションで確認してください。 

問題が解決されない場合は、サポートフォーラムの[Fire TV and Fire TV Stick](https://forums.developer.amazon.com/spaces/43/Fire+TV+and+Fire+TV+Stick.html)カテゴリを参照してください。

## ビルドエラー

**問題**: プロジェクトのビルドを試みたが、次のメッセージが表示される。

```
Error: Content is not allowed in prolog
```

このエラーはWindowsに関係するものです。Githubのリポジトリをクローンしたときに、gitのシンボリックリンクが`true`に設定されていませんでした。そのため、一部のXMLファイルで使用されているシンボリックリンクが正しいコンテンツとともにコピーされませんでした。

**解決策**: 次のコマンドで、シンボリックリンクを使用するようgitを設定します。

```
git config –global core.symlinks true 
```

続いて、リポジトリを再度クローンし、プロジェクトをもう一度ビルドします。**Utils > src > main > res > values > strings.xml > strings.xml (en-rUS)** にあるstrings.xmlファイルを調べて、シンボリックリンクが正常に機能していることを確認できます。通常のコンテンツが表示される場合は、シンボリックリンクが正常に機能しています。一方、短い参照しか表示されない場合は、シンボリックリンクが機能していません。

{% include links.html %}
