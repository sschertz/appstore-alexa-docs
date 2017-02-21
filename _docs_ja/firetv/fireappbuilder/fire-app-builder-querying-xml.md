---
title: XMLでクエリを実行する
permalink: fire-app-builder-querying-xml.html
sidebar: fireappbuilder_ja
product: Fire App Builder
toc: false
---

[レシピのセットアップ][fire-app-builder-set-up-recipes-overview]では、`query`パラメータにXMLクエリ構文を使用しました。XMLクエリではXPath式が使用されます。XPath構文の詳細については、[こちら](http://www.w3schools.com/xsl/xpath_syntax.asp)を参照してください。こちらの[XPath Tester / Evaluator](http://www.freeformatter.com/xpath-tester.html)でXPath式をテストすることもできます。

XPathから結果を取得したら、`matchList`セレクターを使用して、クエリ結果内の特定の要素を選択します。`matchList`セレクターでは、XPath構文ではなく、カスタムのAmazon構文を使用して適切な要素をターゲットに指定します。`matchList`セレクターの目的は、適切なアイテムがFire App BuilderのUIに表示されるよう、フィード内の要素をFire App Builderのコンテンツと関連付けることです。

このページの例は、さまざまなXPathクエリとmatchListセレクターを示しています。それぞれの例では、まずクエリを使用して特定の要素をターゲットにしています。次に、`matchList`パラメータを使用してクエリで返された要素を選択しています。

`matchList`パラメータには、(XPathにあるような) 構文を入力して結果を確認できるエバリュエーターはありません。結果を確認するには、Android Studioでアプリのビルドを調べる必要があります。

* TOC
{:toc}

## 例 1

次のようなXMLドキュメントがあるとします。

```xml
<doc>
     <title>My catagory title</title>
     <p pid="1">My category</p>
     <p>my category</p>
 </doc>
```

`doc/title`のクエリを実行すると、結果は次のようになります。

```xml
Element='<title>My catagory title</title>'
```

`doc/p[2]` のクエリを実行すると、結果は次のようになります。

```xml
Element='<title>My catagory title</title>'
```

`doc/p[@pid="1"]` のクエリを実行すると、結果は次のようになります。

```xml
Element='<p pid="1">My category</p>'
```

## 例 2

より複雑なXMLフィードを見てみましょう。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

  <channel>
    <title>Generic mrss feed</title>
    <link>http://www.developer.amazon.com/</link>
    <description>Generic mrss data</description>
    <item>
      <id>1</id>
      <title>Nullamtus</title>
      <link>http://www.developer.amazon.com/</link>
      <pubdate>Wed, 14 Jan 2015 00:36:00 +0000</pubdate>
      <description>Sed a sagittis urna, a fermentum ligula. In sagittis sagittis libero, ut tincidunt sapien egestas.</description>
      <image>https://raw.githubusercontent.com/amzn/web-app-starter-kit-for-fire-tv/master/src/common/assets/images/l1.jpg</image>
      <category>Lifestyle</category>
    </item>
    <item>
      <id>2</id>
      <title>Ut at augue</title>
      <link>http://www.developer.amazon.com/</link>
      <pubdate>Wed, 14 Jan 2015 00:36:00 +0000</pubdate>
      <description>Phasellus vulputate tellus vitae volutpat viverra. Praesent posuere rutrum erat nec suscipit. Fusce interdum porta porta. Integer vulputate malesuada dictum.</description>
      <image>https://raw.githubusercontent.com/amzn/web-app-starter-kit-for-fire-tv/master/src/common/assets/images/l2.jpg</image>
      <category>Travel</category>
    </item>
    <item>
      <id>3</id>
      <title>Quisque porttitor augue</title>
      <link>http://www.developer.amazon.com/</link>
      <pubdate>Wed, 14 Jan 2015 00:36:00 +0000</pubdate>
      <description>Pellentesque vel metus sem. Aenean porta elementum sagittis.</description>
      <image>https://raw.githubusercontent.com/amzn/web-app-starter-kit-for-fire-tv/master/src/common/assets/images/l3.jpg</image>
      <category>Sports</category>
    </item>
 
  </channel>
</rss>

```

`rss/channel/item/category/text()` のクエリの結果は次のようになります。

```xml
Text='Lifestyle'
Text='Travel'
Text='Sports'
```

## 例 3

フィードをもう 1 つ示します。

```xml
<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"
  xmlns:creativeCommons="http://samplesite.com/mymrssfeed">

  <channel>
    <title>Content Mix for US News-in-Pictures 9x16</title>
    <description>Screenfeed Content Server</description>
    <lastBuildDate>Mon, 08 Dec 2014 22:55:16 GMT</lastBuildDate>
<ttl>5</ttl>
 
    <item>
      <title>Taylor Swift</title>
      <guid isPermaLink="false">1</guid>
      <pubDate>Mon, 08 Dec 2014 22:55:16 GMT</pubDate>
      <category>News</category>
      <media:content url="http://samples.screenfeed.com/1.jpg" type="image/jpeg">
        <media:title type="plain">1080x1920 - English - with caption</media:title>
        <media:credit>Â© 2014 Thomson Reuters</media:credit>
        <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9xnRIN9CUGiTWNQBBrjOw-1080x1920h-1.jpg" />
      </media:content>
    </item>
    
    <item>
      <title>Melanie Martinez</title>
      <guid isPermaLink="false">1</guid>
      <pubDate>Mon, 1 Dec 2014 12:35:56 GMT</pubDate>
      <category>Trending</category>
      <media:content url="http://samples.screenfeed.com/2.jpg" type="image/jpeg">
        <media:title type="plain">1080x1920 - English - with caption</media:title>
        <media:credit>Â© 2014 Thomson Reuters</media:credit>
        <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9x4985398UGiTWNQBBrjOw-1080x1920h-2.jpg" />
      </media:content>
    </item>
  </channel>
</rss>
```
 
`rss/channel/item/category/text()` のクエリの結果は次のようになります。

```xml
Text='News'
Text='Trending'
```

`/rss/channel/item`のクエリの結果は次のようになります。


```xml
Element='<item>
  <title>Taylor Swift</title>
  <guid isPermaLink="false">1</guid>
  <pubDate>Mon, 08 Dec 2014 22:55:16 GMT</pubDate>
  <category>News</category>
  <media:content xmlns:media="http://search.yahoo.com/mrss/" url="http://samples.screenfeed.com/1.jpg" type="image/jpeg">
    <media:title type="plain">1080x1920 - English - with caption</media:title>
    <media:credit>Â© 2014 Thomson Reuters</media:credit>
    <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9xnRIN9CUGiTWNQBBrjOw-1080x1920h-1.jpg" />
  </media:content>
</item>'
Element='<item>
  <title>Melanie Martinez</title>
  <guid isPermaLink="false">1</guid>
  <pubDate>Mon, 1 Dec 2014 12:35:56 GMT</pubDate>
  <category>Trending</category>
  <media:content xmlns:media="http://search.yahoo.com/mrss/" url="http://samples.screenfeed.com/2.jpg" type="image/jpeg">
    <media:title type="plain">1080x1920 - English - with caption</media:title>
    <media:credit>Â© 2014 Thomson Reuters</media:credit>
    <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9x4985398UGiTWNQBBrjOw-1080x1920h-2.jpg" />
  </media:content>
</item>'
```
 
{% include links.html %}