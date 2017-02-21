---
title: Integrating Your Catalog with Fire TV
permalink: integrating-your-catalog-with-fire-tv.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

Integrating your media catalog with Amazon Fire TV allows your content to be discovered and launched from the Fire TV home screen. Catalog Integration has two major components:

*   A catalog file that specifies the movies and TV shows that you offer through your app.
*   Integration between your app and the Fire TV Home Screen Launcher, which enables users to play your content directly from search and browse results.

Catalog Integration allows your content to be included in the results of a search performed from the Fire TV home screen. It also allows a user to find your content while browsing Fire TV outside of your app, such as when browsing by genre (also known as "universal browse and search"). Regardless of where in the Fire TV UI users find your content, they can play it directly without needing to open your app first.

{% include note.html content="The information on these pages provides an introduction to Amazon's catalog integration process. To begin your catalog integration with our platform, please [contact us](http://www.amazon.com/gp/html-forms-controller/aftsdk-cdf-request) for further information. If your content qualifies for integration into the catalog, it takes two weeks to set up our systems to provide you with a sandbox to test the integration." %}

* TOC
{:toc}

## Resources in This Section

These topics will teach you about Fire TV catalog and launcher integration. If you're new to this, reading these topics in the given order is recommended.

*   [Understanding Fire TV Catalog Integration][understanding-fire-tv-catalog-integration]: Introduction to Catalog Integration concepts.
*   [Getting Started with Fire TV Catalog Integration][getting-started-catalog-integration]: A quick-start guide to Fire TV catalog integration.
*   [About the Catalog Data Format (CDF)][about-the-cdf]: An overview of the Catalog Data Format (CDF) catalog file that contains your content's metadata.
*   [Setting Up Your AWS Account for Fire TV Catalog Integration][setting-up-your-aws-account-for-fire-tv-catalog-integration]: One-time setup instructions for AWS.
*   [Uploading Your Catalog to Amazon][uploading-your-catalog]: Learn the process for configuring AWS and uploading your CDF file to our service.
*   [Receiving and Understanding the Fire TV Catalog Ingestion Report][receiving-and-understanding-the-catalog-ingestion-report]: Learn about the reports on your catalog's ingestion results.
*   [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]: How to modify your app to integrate subscription and playback information into the Amazon Fire TV launcher.
*   [Test Cases for Verifying Fire TV Deep Links][test-cases-for-verifying-deep-links-from-your-fire-tv-catalog]: Recommended test cases to run for testing launcher integration.
*   [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb]: Using the Android Debug Bridge to test your app's integration with the launcher.
*   [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app]: We provide a simple app with which you can test your app's integration with the launcher.
*   [Fire TV Catalog Integration Frequently Asked Questions][fire-tv-catalog-integration-faqs]: Frequently asked questions about Catalog Integration.

Read these topics as needed:

*   [Catalog Data Format (CDF) Ingestion Report Messages][catalog-data-format-ingestion-report-messages]: Interpret the messages contained in the ingestion report and troubleshoot your CDF file.
*   [Migrating a Catalog Data Format (CDF) File to the Latest Version][migrating-a-cdf-file-to-the-latest-version]: Step-by-Step instructions to adapt your catalog file to the latest CDF schema version.

These documents can be used as a reference once you have a grasp of the overall catalog and launcher integration concepts.

*   [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference]: Overall reference documentation for the Catalog Data Format XML schema definition (XSD).
*   [Catalog Data Format (CDF) Schema Definition](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd): The XSD file that defines the current CDF.
*   [Catalog Data Format (CDF) Examples](https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-examples.zip): Download a zip file that contains examples of CDF XML examples of individual media types as well as a full catalog, and examples showing how to tie shows, seasons, specials, extras, and episodes together.

## Changes to the Catalog Data Format (CDF) XML Schema

From time to time, the CDF schema, on which your catalog file is based, is updated to add new features. In rare cases, existing elements might be removed or altered. Check here for a summary of the latest changes. See [Migrating a Catalog Data Format (CDF) File to the Latest Version][migrating-a-cdf-file-to-the-latest-version]) for more detailed information and instructions on how to use new elements.

### What's New (10/2015): CDF v1.3

*   New tags `<MiniSeries>` and `<MiniSeriesEpisode>` for use with episode-like content that does not belong to the `<TVEpisode>` type. Amazon considers episodes that belong to both a `<TvSeason>` and `<TvShow>` as valid `<TVEpisode>` types. `<MiniSeriesEpisode>` is used for sequential, episodic content that doesn’t belong to a `<TvSeason>`. `<MiniSeriesEpisode>` entries need to have an order and you must provide sequence numbers for those episodes.
*   New tag `<TVSpecial>` for non-sequential or one-off episodes that might or might not belong to a `<TvShow>`. Each `<TVSpecial>` entry requires an `<OriginalAirDate>`, which Amazon uses to order the `<TVSpecial>` in relation to other entries. A `<TVSpecial>` can be linked to a `<TVShow>` without also having to link to a `<TVSeason>`.
*   New tag `<Extra>` to include trailers and clips. Trailers and clips can be associated with other content.
*   The `<ReleaseInfo>` element, containing the `<ReleaseDate>` and `<ReleaseCountry>` elements, is now deprecated. The `<ReleaseCountry>` information is no longer used. The `<ReleaseDate>` information is now specific to each work type. `<Movie>`, `<TvShow>`, and `<MiniSeries>` have a new `<ReleaseDate>` element. `<TvEpisode>`, `<TvSpecial>`, and `<MiniSeriesEpisode>` use `<OriginalAirDate>`. `<Extra>` does not have release date information.

### What's New (5/27/2015) CDF v1.2

*   New tag `<JP_Require18PlusAgeConfirmation>` to indicate adult content in the Japanese marketplace.
*   New tag `<Count>` in `RatingType` to indicate the number of users who have contributed a rating.
*   The `<Role>` tag in `ActorType` is now optional.
*   The `<Quality>` element in each Offer type is now deprecated. Use the `<Quality>` element in `<LaunchDetails>` instead.
*   New tag `<LaunchDetails>` provides new content metadata that can be used by the Fire TV launcher, including `<Quality>`, `<AudioLanguage>` (for dubbed options), `<Subtitle>` (for alternate language subtitles), and `<LaunchId>` for a custom launch ID that includes these metadata.


## Older Resources

* [Catalog Data Format (CDF) Reference][1] (version 1.2): Overall reference documentation for the Catalog Data Format XML schema definition (XSD).
* [Catalog Data Format (CDF) Schema Definition][2] (version 1.2): The XSD file that defines the CDF format.
* [Catalog Data Format (CDF) Reference (version 1.1)][3]: Overall reference documentation for the Catalog Data Format XML schema definition (XSD).
* [Catalog Data Format (CDF) Schema Definition (version 1.1)][4]: The XSD file that defines the CDF format.
* [Catalog Data Format (CDF) Reference (version 1.0)][5]: Overall reference documentation for the Catalog Data Format XML schema definition (XSD).
* [Catalog Data Format (CDF) Schema Definition][6] (version 1.0): The XSD file that defines the CDF format.

[1]: https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-xsd-ref-12.html
[2]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-12.xsd
[3]: https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-xsd-ref-11.html
[4]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-11.xsd
[5]: https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-xsd-ref-10.html
[6]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-10.xsd


{% include links.html %}
