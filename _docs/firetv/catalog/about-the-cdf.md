---
title: About the Catalog Data Format (CDF)
permalink: about-the-cdf.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

A Catalog Data Format (CDF) file, or "catalog file", is an XML file that uses the schema defined in the CDF XSD file. The CDF file contains the metadata (title, length, release year, etc.) for your app's media content (movies, TV shows, specials, mini-series, and extras). 

The CDF file is used to integrate that metadata into Amazon Fire TV's universal browse and search, which enables a user to perform a search or browse for content through the Fire TV home screen rather than having to search in or browse through individual apps. See [Getting Started with Fire TV Catalog Integration][getting-started-catalog-integration] for an overview of the entire integration process.

This page is intended as a general overview of the catalog format, including its major elements and how they fit together. We don't cover the use of all possible elements here; for that, see the [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference] or the [XSD schema file](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd) itself.

* TOC
{:toc}

## An Example Catalog File

The following example shows a very simple catalog file that contains only one item (a movie), and uses only required elements. We strongly recommend that you provide more details about each work than this, but this limited example is useful in showing the most basic CDF file structure. Download the [cdf-examples.zip](https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-examples.zip) file for larger and more comprehensive examples.

```xml
<?xml version="1.0" encoding="utf-8" ?>
    <Catalog xmlns="http://www.amazon.com/FireTv/2014-04-11/ingestion" version="FireTv-v1.3">
        <Partner>Everything Ever Made Filmworks</Partner>
        <Works>
            <Movie>
                <ID>MV-12345</ID>
                <Title locale="en-US">Edison Kinetoscopic Record of a Sneeze</Title>
                <Offers>
                    <SubscriptionOffer>
                        <Regions>
                            <Country>US</Country>
                        </Regions>
                    </SubscriptionOffer>
                </Offers>
            </Movie>
        </Works>
    </Catalog>
```

## General CDF File Structure

This section provides an overview of the structure of the CDF schema and its elements. Readers should have a good understanding of XML.

The top-level outline of a catalog file can be represented as:

```xml
<Catalog>
    <Partner>
        <Works>
</Catalog>
```

[Catalog][catalog-data-format-schema-reference#Catalog] (**required**) is the root element of all catalog files.

[Partner][catalog-data-format-schema-reference#Partner] (**required**) identifies you, the content provider.

[Works][catalog-data-format-schema-reference#Works] (**required**) contains the bulk of the file: all of the movies and TV shows in your library, as much information as you choose to provide about each entry, and the offers that enable the user to view them.

The Works element can contain any number of child elements, each representing an individual work such as a movie or a TV show. If the Works element contains no child elements, we infer that all of your content is no longer available and we will remove it from our index.

The available work type elements are:

| [Movie][catalog-data-format-schema-reference#Movie] | A theatrical or made-for-TV movie. |
| [TvSpecial][catalog-data-format-schema-reference#TvSpecial] | A standalone TV show, which can be a special event or a show associated with a series but not part of its normal sequence of episodes |
| [TvShow][catalog-data-format-schema-reference#TvShow] | A sequential TV presentation, normally presented in episodes grouped by seasons |
| [TvSeason][catalog-data-format-schema-reference#TvSeason] | A single season of a TV show
| [TvEpisode][catalog-data-format-schema-reference#TvEpisode] | A single episode in a single season of a TV show |
| [MiniSeries][catalog-data-format-schema-reference#MiniSeries] | A TV series consisting of a small number of sequential episodes |
| [MiniSeriesEpisode][catalog-data-format-schema-reference#MiniSeriesEpisode] | A single episode in a mini-series |
| [Extra][catalog-data-format-schema-reference#Extra] | A clip or trailer, often related to another work |

Movies are stand-alone elements. TV shows, seasons, and episodes are separate elements that are tied together by IDs, as are mini-series and mini-series episodes. TV specials and extras can be stand-alone elements, but can also be tied to other works. See [Tying Shows, Seasons, and Episodes Together](#tying) for details.

## Common Elements for All Works

All of the work types listed above are built on a core of common elements, extended in each case with only 1-5 type-specific elements. Only a small subset of the common elements are required. The vast majority of elements available to you in the CDF schema are optional, but providing additional metadata both helps the user to find your content more easily and helps us [match][understanding-fire-tv-catalog-integration#matching] your content to that of other content providers for a better user search and browse experience.

The outline of a work element can be represented as:

```xml
<_WorkType_> (such as <Movie> or <TvSeason>)
    <ID>
        <Title>
            <Offers>
</_WorkType_>
```

[ID][catalog-data-format-schema-reference#ID] (**required**) is an identifier of your choosing for the work. Each work's ID must be unique within your catalog and it should never change as long as you offer that work. The ID element is also used in associating work elements, for instance to specify a TV episode as part of a TV show and season.

[Title][catalog-data-format-schema-reference#Title] (**required**) is, of course, the work's title. You can provide the title in multiple languages.

[Offers][catalog-data-format-schema-reference#Offers] (**required**) contains the methods through which the customer can view the work: for free, by rental or purchase, or through subscription. Offers can be limited by time or by region. The Offers element must contain at least one offer, but can contain as many as necessary. There are four offer types:

| [FreeOffer][catalog-data-format-schema-reference#FreeOffer] | The work is free to view at any time |
| [SubscriptionOffer][catalog-data-format-schema-reference#SubscriptionOffer] | The work requires a subscription to your service to view |
| [PurchaseOffer][catalog-data-format-schema-reference#PurchaseOffer] | The work requires a one-time payment, after which it can be viewed at any time |
| [RentalOffer][catalog-data-format-schema-reference#RentalOffer] | The work requires a one-time payment and can be viewed for only a limited time after that |

The outline of an offer, including optional elements, can be represented as:

```xml
<_OfferType_> (such as <FreeOffer> or <PurchaseOffer>)
    <Regions>
        <WindowStart>
            <WindowEnd>
                <LaunchDetails>
                    <Price> (rental or purchase only)
                        <Duration> (rental only)
</_OfferType_>
```

[Regions][catalog-data-format-schema-reference#Regions] (**required**) specifies the countries in which this offer is valid. Regions is **required** and must contain at least one [Country][catalog-data-format-schema-reference#Country].

[WindowStart][catalog-data-format-schema-reference#WindowStart] and [WindowEnd][catalog-data-format-schema-reference#WindowEnd] (both **optional**) can be used together or separately to specify the time when this offer is valid. Before WindowStart and after WindowsEnd, the offer is not shown to the user.

[LaunchDetails][catalog-data-format-schema-reference#LaunchDetails] (**optional**) enables you to specify options for visual quality, audio language, and subtitles under this offer. It also allows you to define a special ID that can be used to directly launch a work with a predetermined configuration of those options.

[Price][catalog-data-format-schema-reference#Price] is the cost to rent or buy the work, and is **required** for RentalOffer and PurchaseOffer. It can be specified in one of four currencies: US dollar, British pound, Japanese yen, and euro. [Duration][catalog-data-format-schema-reference#Duration] is the number of hours that a rental lasts, and is **required** for RentalOffer.

If the availability of a given work changes, you must submit an updated catalog file with the new offer information.

The detail page for a movie, extra, or TV show displays all of the available offers and providers for that item. We display viewing options in this order (subject to change):

*   Free offers
*   Subscription content
*   Fee-based content (purchase or rental)

### Common Optional Elements for All Works

So far, we've discussed the common required elements and a handful of common optional elements. Using those alone, you have the knowledge to construct a valid CDF file. However, those elements account for only about one-third of the total available to you. All the rest of the common elements are optional and are used to provide more information about the work. For example, there are elements for genre, certification, cast and crew, plot description, studio, images, and customer rating. For a full list, see the [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference].

### A Note About Strings

Some string data in the CDF schema, such as a work's title and description, are defined as the custom LocalizedString type. Localized strings allow you to provide the same content in different languages, to be used according to the user's device language setting. These strings have the required attribute _locale_ (of standard type xsd:language). Here is an example:

```xml
<Title locale="en-US">Edison Kinetoscopic Record of a Sneeze</Title>
```

Localized strings also have an optional attribute _pronunciation_ (of standard type xsd:string). This attribute is provided for Japanese catalog entries that specify string text in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana.

## Elements Specific to the Work Type

In addition to the common elements, each work type has from one to five elements specific to that work type alone. In general, these elements have two uses: (1) to specify an original release or air date and (2) to tie works together, such as a TV episode to a TV show.

Release dates are **optional** but recommended, and are specified by either a [ReleaseDate][catalog-data-format-schema-reference#ReleaseDate] (for movies, TV shows, or mini-series) or [OriginalAirDate][catalog-data-format-schema-reference#OriginalAirdate] (for TV and mini-series episodes, and TV specials).

The elements that bundle shows, seasons, and episodes are **required**, and are discussed in more detail in the next section.

Refer to the [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference] for particulars on the type-specific elements.

## Tying Shows, Seasons, and Episodes Together {#tying}

TV episodes are aired during one season of a particular TV show. A mini-series is made up of individual episodes. An extra can provide a preview or behind-the-scenes information about a movie. A TV special might be related to a TV show, but be outside of that show's normal sequence. The CDF provides elements that enable you to make those connections. When a Fire TV user browses to a TV show and sees the show organized by seasons with each episode shown sequentially in its season, that is the result of these elements.

In general, you can associate one work with another by either ID or title. An ID must match the ID of another work in your catalog. If that work is not in your catalog, you can use a title instead. The title isn't required to match anything in your catalog and is only used to group works. 

The following table shows these elements for each work type:

| Work Type | Link Elements |
|------|-------|
| TvEpisode | [ShowID][catalog-data-format-schema-reference#ShowID] or [ShowTitle][catalog-data-format-schema-reference#Showtitle] <br/> [SeasonID][catalog-data-format-schema-reference#SeasonID] or [SeasonInShow][catalog-data-format-schema-reference#SeasonInShow] <br/> **Note**: TvEpisode also has a [SeasonTitle][catalog-data-format-schema-reference#SeasonTitle] element, but it is not used for grouping. |
| TvSeason | [ShowID][catalog-data-format-schema-reference#ShowID] or [ShowTitle][catalog-data-format-schema-reference#ShowTitle] |
| TvShow | None |
| TvSpecial | [ShowID][catalog-data-format-schema-reference#ShowID] or [ShowTitle][catalog-data-format-schema-reference#ShowTitle] |
| MiniSeries | None |
| MiniSeriesEpisode | [MiniSeriesID][catalog-data-format-schema-reference#MiniseriesID] or [MiniSeriesTitle][catalog-data-format-schema-reference#MiniSeriesTitle] |
| Extra | [RelatesToID][catalog-data-format-schema-reference#RelatesToID] or [RelatesToExternalID][catalog-data-format-schema-reference#RelatesToExternalID] |
| Movie | None |

Rather than a title, TvEpisode uses a number to specify a season not in your catalog and Extra uses a link to an external ID scheme, such as IMDb. Also, while you can choose which to use, one in each element link pair is **required** for TvEpisode, TvSeason, and MiniSeriesEpisode. The link value is **optional** for TvSpecial and Extra, which can be standalone works.

The following illustration shows how the various elements and values are matched for a TV show when using IDs.

{% include image.html caption="Diagram showing the relations between the IDs of TV shows, seasons, episodes, and specials" file="firetv/catalog/images/catalog_RelatingShowsSeasonsSpecialsAndEpisodes" type="png" %}

For instance, if a TvShow had an ID of "TV-123456", the ShowID values in TvEpisode and TvSeason would also equal "TV-123456". Download the [Catalog Data Format Examples](https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-examples.zip) to see fully implemented catalogs illustrating these concepts.

## Requirements for Box Art Images (ImageUrl)

The [ImageUrl][catalog-data-format-schema-reference#ImageUrl] element, one of the optional elements common to all works, provides the URL of an image that represents the work, sometimes called the "box art." If you don't include ImageUrl, we attempt to use art available from other sources such as IMDb, if we can make a [match][understanding-fire-tv-catalog-integration#matching], or we might use a generic placeholder image.

{% include note.html content="Your catalog must provide valid images for at least 50% of its entries or the integration process will fail and the entire catalog will be rejected." %}

### Image requirements

| Type | JPG (preferred) or PNG | Other image types will not be used |
| Aspect Ratio | Between 1:3 and 3:1 (1:2 and 2:1 preferred) | Images with aspect ratios less than 1:3 or greater than 3:1 will not be used <br/>Images with aspect ratios between 1:2 and 1:3 are cropped to 1:2 <br/> Images with aspect ratios between 2:1 and 3:1 are cropped to 2:1 |
| Size | Height greater than 240 px (480 px preferred) | Images less than 480 px in height generate a warning in the [ingestion report][receiving-and-understanding-the-catalog-ingestion-report], but those between 240 px and 480 px are accepted without counting toward the total number of invalid images. Images less than 240 px in height will not be used. For optimal quality, we prefer large images (no image size is too big) that we can scale as needed. <br/><br/>**Note**: If we crop your image because of its aspect ratio, the cropped version must still meet this height requirement regardless of its original dimensions. |


At a minimum, we recommend that your images meet the specifications in the following table. Note that this refers to the box art image, not the work itself.

| Media | Aspect Ratio | Minimum Size |
|----|-----|-----|
| Movies | 3:4 | 360 px by 480 px |
| TV episodes, seasons, shows, and specials <br/>Mini-series and mini-series episodes | 16:10 | 768 px by 480 px |
| TV episodes, seasons, shows, and specials <br/>Mini-series and mini-series episodes | 16:9 | 853 px by 480 px |

## Validating the CDF File Against the Schema {#validating}

Before you upload your catalog file, please ensure that it is well formed XML and validates against the schema. An incorrectly formatted or invalid catalog file cannot be accepted by our service. For example, on Linux systems, you can use the following `xmllint` command to validate your file. This command assumes that your catalog file (file.xml) is in the same folder as the schema file (schema.xsd), and you are in this folder when you issue the command. You can download the current schema file [here](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd).

```xml
xmllint --noout --schema schema.xsd file.xml
```

Most Integrated Development Environment (IDEs) such as Eclipse or Intellij can also validate your catalog against the schema. For more information, see:

*   [Eclipse XML Validation][3]
*   [Intellij XML Validation][4]
*   [Visual Studio XML Validation][5]
*   [Notepad++ XML Tools Plug-In][6]

[1]: http://www.amazon.com/gp/html-forms-controller/aftsdk-cdf-request
[2]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-13.xsd
[3]: http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.wst.xmleditor.doc.user%2Ftopics%2Ftwxvalid.html
[4]: https://www.jetbrains.com/idea/help/validating-web-content-files.html](https://www.jetbrains.com/idea/help/validating-web-content-files.html
[5]: https://msdn.microsoft.com/en-us/library/ms255815.aspx](https://msdn.microsoft.com/en-us/library/ms255815.aspx
[6]: https://notepad-plus-plus.org

{% include links.html %}
