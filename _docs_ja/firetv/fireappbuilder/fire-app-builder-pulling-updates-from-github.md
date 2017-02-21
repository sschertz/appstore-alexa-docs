---
title: Githubから更新データをプルする
permalink: fire-app-builder-pulling-updates-from-github.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

Githubでプロジェクトをwatchおよびstarすることで、Fire App Builderへの最新のコミットに関する更新情報を随時受け取ることができます。また、ドキュメントの「[リリースノート][fire-app-builder-release-notes]」ページを定期的に確認することもできます。

{% include note.html content="プロジェクトのクローンとビルドを初めて行う場合は、このトピックではなく「[Fire App Builderをダウンロードして、アプリをビルドする][fire-app-builder-download-and-build]」を参照してください(このトピックでは、既存のプロジェクトへの更新の適用方法について取り上げています)。" %}

* TOC
{:toc}

## 更新データを取得する

新しいバージョンのFire App BuilderがリリースされてGithubにプッシュされたら、Githubワークフローで広く使用されている`git pull`を使用して、新しいバージョンの更新データを取得し、そのコードをプロジェクトに組み込むことができます。 

まず、自分のfire-app-builderディレクトリ (または自分で名前を付けたディレクトリ) に移動します。次に、最新の変更点を自分のリポジトリにプルします。

```
cd fire-app-builder
git pull
```

[`git pull`](https://git-scm.com/docs/git-pull)コマンドを実行すると、オリジナルのリポジトリから最新の更新データがダウンロードされ、コードへのマージが試みられます。 

## マージの競合を解決する

オリジナルリポジトリのファイルに加えられた変更とローカルコピーに加えた変更が競合する場合、ローカルコピーが自動的に更新データで上書きされることはありません。代わりに、影響のあるファイルについてのマージの競合が表示され、そのファイルがトラッキングから除外されます。この場合、マージの競合を解決し、除外されたファイルをgitトラッキングに戻し、更新をコミットする必要があります。

`git pull`の実行後にマージの競合が表示された場合は、マージの競合が生じているファイルを編集します。競合の発生場所はキャレット (>>>>>>) および (<<<<<<) で示されています。ファイルを手動で編集してキャレットを削除します。次に、`git add <filename>` を実行してファイルをgitトラッキングに戻します。この <filename> は戻すファイルの名前です。 

マージの競合を解決する方法の詳細については、「[Resolving a merge conflict from the command line](https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/)」を参照してください。

## Gitのリソース

gitについて学習するうえで役に立つリソースの一部を以下に挙げます。

* [gitのドキュメント](https://git-scm.com/doc)
* [git - the simple guide](http://rogerdudler.github.io/git-guide/)
* [Atlassianによるgitのチュートリアル](https://www.atlassian.com/git/tutorials/)

{% include links.html %}