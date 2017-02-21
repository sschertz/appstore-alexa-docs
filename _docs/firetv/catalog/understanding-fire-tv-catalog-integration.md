---
title: Understanding Fire TV Catalog Integration
permalink: understanding-fire-tv-catalog-integration.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

Integrating your media catalog with Amazon Fire TV allows your content to be discovered and launched from the Fire TV home screen. Catalog Integration has two major components:

*   A catalog file that specifies the movies and TV shows that you offer through your app.
*   Integration between your app and the Fire TV Home Screen Launcher, which enables users to play your content directly from search and browse results.

Catalog Integration allows your content to be included in the results of a search performed from the Fire TV home screen. It also allows a user to find your content while browsing Fire TV outside of your app, such as when browsing by genre (also known as "universal browse and search"). Regardless of where in the Fire TV UI users find your content, they can play it directly without needing to open your app first.

{% include note.html content="To begin your catalog integration with our platform, please [contact Amazon][1] for further information." %}

* TOC
{:toc}

## Benefits of Integrating Your Catalog with Fire TV

Catalog Integration allows users to seamlessly view, browse, and search for your content alongside content from Amazon and other streaming media providers within Fire TV. It provides these benefits:

*   [Includes your content in universal browse and search results across content providers](#ubas)
*   [Drives new customers to download and use your app](#acquisition)
*   [Retains existing customers through prioritized search results and ease of use](#rec)
*   [Qualifies content to be promoted through "Featured Movies and TV" on the Fire TV home screen](#featured)

### Universal Browse and Search Results Across Content Providers {#ubas}

Catalog Integration allows a consolidated customer search and browse experience, regardless of which apps provide the content. For example, consider a Fire TV customer who wants to watch the fictional movie "Argoneum 2" and searches for "Argoneum" from the Fire TV home screen. Fire TV would return the following search results:

{% include image.html file="firetv/catalog/images/catalog_CI_Argoneum2" type="png" %}

The search results show all items found across all content providers with an integrated catalog, as well as consolidating multiple results for a work into a single result (see [Content Matching](#matching) below).

If a customer ran the same search, but you had not submitted your catalog for integration while others had, the search results would include only offerings from Amazon and those providers with integrated catalogs. Customers would have to explicitly launch your app and search within it to find that you also offered that movie.

### Drive New Customer Acquisition {#acquisition}

Catalog integration can drive new customers to download and use your app. When your catalog is integrated, your content is shown in Fire TV home screen search results even if a user has not downloaded your app or is not yet a subscriber to your service. If you offer a free trial subscription to new users and allow users to sign up through a Fire TV device, your version of the content can be presented in the highlighted **Watch Now with..** box, depending on other available offers. (Note that these prioritization rules are subject to change without notice.)

For example, the fictional TV series "Office Factor" is available to subscribers of the fictional streaming service Enyo Xtra. When a user who is not already an Enyo Xtra subscriber searches for "Office Factor", Fire TV displays both the series information and a prompt to start a free trial with the Enyo Xtra service.

{% include image.html file="firetv/catalog/images/catalog_CI_OfficeFactor_trial" type="png" %}

When the user clicks the "Free Trial" button, Fire TV displays a screen that prompts the user to download the Enyo Xtra app to begin the free trial. 

{% include image.html file="firetv/catalog/images/catalog_EnyoXtra_trial" type="png" %}

### Retain Existing Customers {#rec}

The detail page for a movie or TV show uses integrated catalog data to display all the available providers and options for that item, generally prioritizing them by offer type: free, subscription, rental, and purchase. This prioritization order means that your current subscribers will often see your app as the first option. Seeing your app listed as a preferred viewing option reminds customers of the value they receive from subscribing to your service. (Note that these prioritization rules are subject to change without notice.)

In the following example, a subscriber to the fictional StreamTime service searches for the fictional movie "Repeat Offender". The search results show that the movie can be either streamed through StreamTime or a digital copy can be purchased from Amazon. Due to the prioritization order, the StreamTime option is given top priority.

{% include image.html file="firetv/catalog/images/catalog_RepeatOffender" type="png" %}

### Inclusion in Featured Movies and TV {#featured}

Fire TV's home screen includes a category titled "Featured Movies and TV", with a rotating selection of works. Only integrated catalog content is eligible to be included in these promoted titles.

## Components of Catalog Integration

Fire TV catalog integration has two major components, both of which are required:

*   **Catalog Data Format (CDF) file**: The CDF file is an XML file that uses the schema defined by the [CDF XSD file][2]. The CDF file contains the catalog of movies and TV shows that you offer through your Fire TV app. Each work's entry in your catalog includes metadata that describes the work, such as title, release date, cast and crew, and length. The metadata also includes the methods of viewing the work: for free, only by subscribers, or through rental or purchase. You will upload this catalog to Amazon. To learn more about the CDF file and its schema, see [About the Catalog Data Format (CDF)][about-the-cdf] and the [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference].
*   **Amazon home screen launcher**: You will configure your app to integrate with the media launcher used by Fire TV's home screen. This allows a customer to launch content from your catalog without having to launch your app first.

## Content Matching {#matching}

When a content item, such as a movie, is included in the integrated catalogs of multiple content providers, Amazon uses the item's metadata (such as title or year of release) to identify and match them as the same actual piece of content. As a result, Fire TV's universal browse and search can present a single entry for the item instead of separate entries for each provider.

The following image shows the results of a search for the fictional movie "Argoneum", offered by three different providers, without content matching. The user would have to select each result in turn to determine the provider and whether the movie is available for free, rental, purchase, or through subscription from that provider.

{% include image.html caption="Search results with 3 instances of the movie and 2 instances of a secondary result" file="catalog/catalog_ArgoneumWithoutMatching" type="png" %}

With content matching, the user is presented with a cleaner set of results:

{% include image.html caption="Search resuts with 1 instance of the movie and 1 instance of a secondary result" file="catalog/catalog_ArgoneumWithMatching" type="png" %}

When the user selects the single "Argoneum" entry, they are then shown the three providers and the viewing options for each, all in one place.

The chance of Amazon successfully matching one piece of content to another depends on the amount of information that you can provide. If you only include the item's required elements in your CDF file, the title might be the only data available to Amazon for matching. If you include a release year, the content's runtime, an episode number in a season or mini-series, or any other unambiguous metadata value, you will increase the chance of Amazon achieving a high-confidence match when a user searches or browses for that content.

## Guidelines and Expectations for Fire TV Catalog Integration

As a content provider participating in Fire TV catalog integration, you should ensure that your processes related to catalog updates and uploads meet the following guidelines, and set your expectations accordingly with regards to various wait times after uploading a new catalog:

### Guidelines for Uploading Catalogs

*   Amazon expects your catalog to be uploaded at least once per week, regardless of whether the catalog has changed. Your upload process should be scripted or otherwise automated so that the current catalog is uploaded on at least a weekly interval.
*   If your most recent successfully ingested catalog file is more than three weeks old, Amazon will disable catalog integration for your app. (In other words, if your catalog is stale for more than three weeks, or fails catalog ingestion for more than three weeks, Amazon disables the catalog integration.)

### Wait Times for Content Availability after Upload

*   Amazon will validate and ingest newly uploaded catalog files every four hours. After uploading a new catalog, you might have to wait up to four hours to see if the ingestion was successful.
*   All content included in a valid catalog file will be available to the user within four hours after catalog upload. However, on-device caches for shows and seasons can persist for up to ten hours, so some content could have up to a 14-hour delay in availability after catalog upload. This means that if a show is cached on a device and has a new season uploaded, the new content might not actually be visible to users for up to 14 hours.
*   Content will be able to be matched to content from other providers in search or browse within 4 hours of catalog upload.

### User Experience (UX) Guidelines for Catalog Integration

*   When a user selects content from your app via Fire TV's universal search or browse results, begin playback of that content immediately; do not route the customer to your app or the work's detail screen before starting playback.
*   As part of launcher integration (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]), make sure that you publish your users' subscription status each time that your app opens so that the "Watch Now" option appears for current subscribers when your content is found from outside of your app. 
*   In the event that your current available catalog becomes out of sync with your content that is actually available to users, implement error handling code to gracefully handle the use case where a user selects a content item that is not actually available for playback. If you do not implement error handling code for this scenario, the user will be taken to a black screen with no clear way to navigate back to Fire TV or your app or catalog, providing a confusing and negative user experience.

To learn more about the catalog integration set-up process, including setting up your AWS account and working with the S3 tools, see [Getting Started with Fire TV Catalog Integration][getting-started-catalog-integration]. See [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration] for the code changes needed to integrate with the launcher.

[1]: http://www.amazon.com/gp/html-forms-controller/aftsdk-cdf-request
[2]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-13.xsd

{% include links.html %}
