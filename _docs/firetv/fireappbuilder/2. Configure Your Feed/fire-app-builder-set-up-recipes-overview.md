---
title: Recipe Configuration Overview
permalink: fire-app-builder-set-up-recipes-overview.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

After you [load the media feed][fire-app-builder-load-media-feed], you need to configure two recipes that Fire App Builder will use to pull the media categories and content from your feed. "Recipes" are simple files that contain settings (in the form of keys and values) that Fire App Builder uses when building your app. The two recipes you must configure are as follows:

*  **[Categories recipe][fire-app-builder-set-up-recipes-categories]**: Provides different containers to group your media. 
*  **[Contents recipe][fire-app-builder-set-up-recipes-categories]**:  Maps the properties from your media feed, such as the titles, descriptions, video URLs, and images, to Fire App Builder's content model.

If you look at the Home screen with the default Lightcast feed, you'll see that the media is organized in different rows. On the default home screen, you can see that media is grouped under "Jamaica Attractions" and "The Country Jamaica," among other groups.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" alt="Alternative Home screen" caption="Categories providing groupings for media." %}

Since every media feed usually has a different structure (for example, your feed might use the term "channels" instead of "categories," "img" instead of "image," and so on, as well as different hierarchies and structures) you will need to write some query syntax to target the categories and content in your feed. 

If your feed is JSON, you will use [Jayway JsonPath syntax](https://github.com/jayway/JsonPath) to write the queries. If your feed is XML, you will use [XPath syntax here](http://www.w3schools.com/xsl/xpath_syntax.asp) to write the queries.

After you query for the categories and content from your query, you then apply a selector to get the category or content text from the results. This selector, called `matchList`, doesn't use Jayway JsonPath or XPath expressions but rather is custom Fire App Builder syntax. 

## Next Steps

See [Set Up the Category Recipe][fire-app-builder-set-up-recipes-categories] to start identifying categories in your feed. Fire App Builder will group your media based on these categories.

{% include links.html %}