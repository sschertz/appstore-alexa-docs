---
title: Querying XML
permalink: fire-app-builder-querying-xml.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

When you [set up your recipes][fire-app-builder-set-up-recipes-overview], you used XML query syntax for the `query` parameter. XML queries use XPath expressions. You can learn more about the [XPath syntax here](http://www.w3schools.com/xsl/xpath_syntax.asp). You can also test out your XPath expressions using this [XPath Tester / Evaluator](http://www.freeformatter.com/xpath-tester.html).

Once you get the result from XPath, you use `matchList` selectors to select specific elements in the query result. The `matchList` selectors don't use XPath syntax but rather a custom Amazon syntax to target the right elements. The purpose of the `matchList` selector is to correlate an element in your feed with the Fire App Builder content model so that the right item can be displayed in the Fire App Builder UI.

The examples on this page demonstrate a variety of both XPath queries and matchList selectors. In each example, first a query is used to target specific elements from your feed. Then a `matchList` parameter is used to select the elements from the query's returns.

With the `matchList` parameter, there's not an evaluator (as there is with XPath) where you can plug in the syntax and see the result. The only way is to look at how your app builds in Android Studio.

* TOC
{:toc}

## Example 1

Suppose your XML document looks like this:

```xml
<doc>
     <title>My catagory title</title>
     <p pid="1">My category</p>
     <p>my category</p>
 </doc>
```

A query for `doc/title` would result in the following:

```xml
Element='<title>My catagory title</title>'
```

A query for `doc/p[2]` would result in the following:

```xml
Element='<title>My catagory title</title>'
```

A query for `doc/p[@pid="1"]` would result in the following:

```xml
Element='<p pid="1">My category</p>'
```

## Example 2

Now let's look at a more complex XML feed:

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

A query for `rss/channel/item/category/text()` would result in the following:

```xml
Text='Lifestyle'
Text='Travel'
Text='Sports'
```

## Example 3

Here's one more feed:

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
 
A query for `rss/channel/item/category/text()` would result in the following:

```xml
Text='News'
Text='Trending'
```

A query for `/rss/channel/item` would result in the following:


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