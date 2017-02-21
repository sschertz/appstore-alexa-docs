---
title: Fire TV Catalog Data Format (CDF) Schema
permalink: catalog-data-format-schema-reference.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

This topic provides a dictionary of the elements available in the Catalog Data Format (CDF) schema, used to construct a catalog of your media content for upload to Amazon Fire TV.

For an overview of the structure of a CDF catalog file and how these elements interact, see [About the Catalog Data Format (CDF)][about-the-cdf] The catalog examples in the downloadable [cdf-examples.zip](https://s3.amazonaws.com/com.amazon.aftb.cdf/cdf-examples.zip) file also can be very useful in understanding how a catalog file is put together. Download [catalog.xsd](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd) to examine the XSD directly.

To use this information, readers should have a good understanding of XML. The following sections list the CDF schema element definitions.

* TOC
{:toc}

## AdultProduct (deprecated) {#AdultProduct}

**Deprecated, do not use**. Use a [ContentRating](#ContentRating) to convey this information instead. In Japan, you can also use [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation) (CDF v1.2 and later).

Identifies a work as content for adult audiences only.

**Optional:**

| Added | CDF version 1.0 |
| Deprecated | CDF version 1.1 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | true <br/> false |

**Example**:

```xml
<Movie>false</Movie>
  ...
  <AdultProduct>false</AdultProduct>
  ...
</Movie>false</Movie>
```

* * *

## AudioLanguage {#AudioLanguage}

An audio option for the work when that work has been dubbed into additional languages. You can include as many AudioLanguage elements as needed to specify the work's available alternatives.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [LaunchDetails](#LaunchDetails) |
| Child Elements | None |
| Attributes | None |
| Accepted values | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR |

**Example:**

```xml
<LaunchDetails>
  <Quality>SD</Quality>
  <Quality>HD</Quality>
  <AudioLanguage>en-US</AudioLanguage>
  <AudioLanguage>es-MX</AudioLanguage>
  <Subtitle>en-US</Subtitle>
  <Subtitle>es-MX</Subtitle>
  <LaunchId>MV123456_HD_es-MX_en</LaunchId>
</LaunchDetails>
```

* * *

## CastMember {#CastMember}

Provides information about a person in the work's cast, such as an actor, host, narrator, or voice talent. When present, the optional [Credits](#Credits) element must include at least one entry, either a CastMember or a [CrewMember](#CrewMember). You can include as many CastMember elements as needed.

**Required if no CrewMember element is present, otherwise optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Credits](#Credits) |
| Child Elements | [Name](#Name) (required), [ExternalID](#ExternalID) (optional), [Role](#Role) (optional) |
| Attributes | None |

**Example:**

```xml
<Credits>
  <CastMember>
    <Name locale="en-US">Alan Smithee</Name>
    <ExternalID scheme="imdb">tt0000000</ExternalID>
    <Role locale="en-US">Self</Role>
  </CastMember>
</Credits>
```

* * *

## Catalog {#Catalog}

The root element of a CDF file. Each catalog file must contain a single Catalog element which contains the rest of the file.

**Required:**

| Added | CDF version 1.0 | | |
| Parent Elements | None | | |
| Child Elements | [Partner](#partner) (required), [Works](#works) (required) | | |
| Attributes | | | |
| | Attribute | Accepted Values | Description |
| | xmlns | http://www.amazon.com/ FireTv/2014-04-11/ingestion | **Required**. The XML namespace. |
| | SchemaVersion | FireTv-v1.2 <br/> FireTv-v1.3 | **Optional**. Added in CDF v1.2\. The version of the schema this catalog uses. Refer to the schema "id" to figure out which schema version you are using. Although this attribute is optional for compatibility reasons, we recommend that you provide the version. |

**Example**:

```xml
<xml version="1.0" encoding="utf-8" ?>
<Catalog xmlns="http://www.amazon.com/FireTv/2014-04-11/ingestion" version="FireTv-v1.3">
  <Partner>Everything Ever Made Filmworks</Partner>
  <Works>
    ...
  </Works>
<Catalog>
```

* * *

## Certification {#Certification}

The certification or rating given to the work under a specified certification [System](#System). Only one Certification element is allowed for each ContentRating.

**Required in a ContentRating:**

| Added | CDF version 1.0 |
| Parent Elements | [ContentRating](#ContentRating) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<ContentRating>
  <System>MPAA</System>
  <Certification>PG-13</Certification>
</ContentRating>
```

* * *


## Color {#Color}

Specifies whether the movie is primarily in color or in black-and-white.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | color <br/>black_and_white |

**Example:**

```xml
<Movie>
  ...
  <Color>black_and_white</Color>
  ...
</Movie>
```

* * *

## ContentRating {#ContentRating}

Contains elements that specify a rating system or organization and the rating they gave the work. When present, the optional [ContentRatings](#ContentRatings) must contain at least one ContentRating. You can have as many ContentRating elements as you need, one for each system/rating pair.


**Required if the optional ContentRatings element is present:**

| Added | CDF version 1.0 |
| Parent Elements | [ContentRatings](#ContentRatings) |
| Child Elements | [System](#System) (required), [Certification](#Certification) (required) |
| Attributes | None |

**Example:**

```xml
<ContentRatings>
  <ContentRating>
    <System>MPAA</System>
    <Certification>PG-13</Certification>
  </ContentRating>
</ContentRatings>
```

* * *

## ContentRatings {#ContentRatings}

Contains one or more official ratings for the work, as determined by a specified certifying agency. Only one ContentRatings element is allowed per work.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [ContentRating](#ContentRating) (at least one required) |
| Attributes | None |

**Example:**

```xml
<ContentRatings>
  <ContentRating>
    <System>MPAA</System>
    <Certification>PG-13</Certification>
  </ContentRating>
  <ContentRating>
    <System>Eirin</System>
    <Certification>R15+</Certification>
  </ContentRating>
</ContentRatings>
```

* * *

## Copyright {#Copyright}

A statement of the work's copyright.

**Optional:**

| Added | CDF version 1.0 | |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | | |
| | Attribute | Accepted Values | Description |
| locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<Copyright locale="en-US">© 2014 Amazon Studios</Copyright>
```

* * *

## Count {#Count}

The number of users that have contributed to a customer rating [Score](#Score). Only one Count is allowed per [CustomerRating](#CustomerRating).

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [CustomerRating](#CustomerRating) |
| Child Elements | None |
| Attributes | None |
| Accepted values | Any non-negative whole number. |

**Example:**

```xml
<CustomerRating>
  <Score>8.2</Score>
  <MaxValue>10</MaxValue>
  <Count>512</Count>
</CustomerRating>
```

* * *

## Country {#Country}

A country in which a particular offer (subscription, free, purchase, or rental) is available. Each offer can contain as many Country elements as needed.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Regions](#Regions) |
| Child Elements | None |
| Attributes | None |
| Accepted values | The following subset of [ISO 3166-1 country codes](http://www.iso.org/iso/country_codes.htm): AF AX AL DZ AS AD AO AI AQ AG AR AM AW AU AT AZ BS BH BD BB BY BE BZ BJ BM BT BO BQ BA BW BV BR IO BN BG BF BI KH CM CA CV KY CF TD CL CN CX CC CO KM CG CD CK CR CI HR CU CW CY CZ DK DJ DM DO EC EG SV GQ ER EE ET FK FO FJ FI FR GF PF TF GA GM GE DE GH GI GR GL GD GP GU GT GG GN GW GY HT HM VA HN HK HU IS IN ID IR IQ IE IM IL IT JM JP JE JO KZ KE KI KP KR KW KG LA LV LB LS LR LY LI LT LU MO MK MG MW MY MV ML MT MH MQ MR MU YT MX FM MD MC MN ME MS MA MZ MM NA NR NP NL NC NZ NI NE NG NU NF MP NO OM PK PW PS PA PG PY PE PH PN PL PT PR QA RE RO RU RW BL SH KN LC MF PM VC WS SM ST SA SN RS SC SL SG SX SK SI SB SO ZA GS SS ES LK SD SR SJ SZ SE CH SY TW TJ TZ TH TL TG TK TO TT TN TR TM TC TV UG UA AE GB US UM UY UZ VU VE VN VG VI WF EH YE ZM ZW |

**Example:**

```xml
<SubscriptionOffer>
  <Regions>
    <Country>US</Country>
    <Country>CA</Country>
  </Regions>
  ...
</SubscriptionOffer>
```

* * *

## Credits {#Credits}

Contains elements that represent a work's cast and crew members. The same person can appear as both cast or crew multiple times. Each work can contain only one Credits element. If present, Credits must contain at least one [CastMember](#CastMember) or [CrewMember](#CrewMember), though it can contain as many of each of those elements as needed.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [CastMember](#CastMember), [CrewMember](#CrewMember) (at least one of these two is required) |
| Attributes | None |

**Example:**

```xml
<Credits>
  <CastMember>
    <Name locale="en-US">Alan Smithee</Name>
    <Role locale="en-US">Self</Role>
  </CastMember>
</Credits>
```

* * *

## CrewMember {#CrewMember}

Contains elements that provide information about a person in the work's off-screen crew, such as a director, writer, cinematographer, best boy, animator, or grip. When present, the optional [Credits](#Credits) must include at least one entry, either a [CastMember](#CastMember) or a CrewMember. You can include as many CrewMember elements as needed.

**Required if no CastMember element is present, otherwise optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Credits](#Credits) |
| Child Elements | [Name](#Name) (required), [ExternalID](#ExternalID) (optional), [Job](#Job) (optional) |
| Attributes | None |

**Example:**

```xml
<Credits>
  <CrewMember>
    <Name locale="en-US">Alan Smithee</Name>
    <ExternalID scheme="imdb">tt0000000</ExternalID>
    <Job locale="en-US">Director</Job>
  </CrewMember>
</Credits>
```

* * *

## CustomerRating {#CustomerRating}

Contains elements that provide the average customer rating for a work, the maximum rating value, and the number of ratings that contributed to the score. Each work can contain only one CustomerRating element.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [Score](#Score) (required), [MaxValue](#MaxValue) (required), [Count](#Count) (optional, CDF v1.2 and later only) |
| Attributes | None |

**Example**:

```xml
<CustomerRating>
  <Score>8.2</Score>
  <MaxValue>10</MaxValue>
  <Count>512</Count>
</CustomerRating>
```

* * *

## Duration {#Duration}

Defines how long a work's rental lasts, measured in hours. A RentalOffer can contain only one Duration element.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [RentalOffer](#RentalOffer) |
| Child Elements | None |
| Attributes | None |

 **Example:**

```xml
<RentalOffer>
  ...
  <Duration>120</Duration>
</RentalOffer>
```

* * *

## EpisodeInSeason {#EpisodeInSeason}

A TV episode's sequence number within its season. Each [TvEpisode](#TvEpisode) can contain only one EpisodeInSeason element.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [TvEpisode](#TvEpisode) |
| Child Elements | None |
| Attributes | None |

**Example**:

```xml
<TvEpisode>
  ...
  <EpisodeInSeason>6</EpisodeInSeason>
  ...
</TvEpisode>
```

* * *

## EpisodeInSeries {#EpisodeInSeries}

A mini-series episode's sequence number within its series. Each [MiniSeriesEpisode](#MiniSeriesEpisode) can contain only one EpisodeInSeries element.

**Required:**

| Added | CDF version 1.3 |
| Parent Elements | [MiniSeriesEpisode](#MiniSeriesEpisode) |
| Child Elements | None |
| Attributes | None |
| Example |

```xml
<MiniSeriesEpisode>
  ...
  <EpisodeInSeries>13</EpisodeInSeries>
  ...
</MiniSeriesEpisode>
```

* * *


## ExternalID {#ExternalID}

An identifier for a work under an external classification, such as IMDb. This value is used in content matching, to compare a work or person against that in another catalog to determine whether they're the same work or person. It can also be used as the source of external content such as images. Each element that contains an ExternalID can contain as many as needed.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [CastMember](#CastMember), [CrewMember](#CrewMember), [Extra](#Extra), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Movie](#Movie), [TvEpisode](#TvEpisode), [TvSeason](#TvSeason), [TvShow](#TvShow), [TvSpecial](#TvSpecial) |
| Child Elements | None | | | |
| Attributes  | Attribute | Description | Accepted Values |
|  | scheme | **Required**. The external source that provided this ID. | imdb <br/>tms<br/>isan<br/>ean<br/>upc | |
| Comments <br/>(the values represent these sources) |  | |
|  | Value | Description |
|  | imdb | The Internet Movie Database (IMDb). IDs can be found as part of the URL of a given page. |
|  | tms | The ID used in the Gracenote™ database. |
|  | isan | The International Standard Audiovisual Number (ISAN), an alphanumeric strings of 26 characters, usually presented broken by dashes. |
|  | ean | The International Article Number (EAN), a barcode normally expressed in 13-digits. |  |
|  | upc | The Universal Product Code (UPC), a barcode normally expressed in 12-digits. |  | |

**Example:**

```xml
<Movie>
  <ID>MV123456</ID>
  <ExternalID scheme="imdb">tt0000000</ExternalID>
  <ExternalID scheme="tms">MV000000000000</ExternalID>
  <ExternalID scheme="isan">0000-0000-0F00-0000-X-0000-0000-Y</ExternalID>
  <ExternalID scheme="ean">0011559514120</ExternalID>
  <ExternalID scheme="upc">123456789990</ExternalID>
  ...
</Movie>
```

* * *

## Extra {#Extra}

One of the basic work types, Extra represents a clip or trailer that can be a standalone work or, more commonly, can be associated with another work (either external or in your catalog). Generally think of these as the equivalent of an extra feature included on a DVD.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to Extra | Required: [Type](#Type) <br/> Required: Either [RelatesToID](#RelatesToID) or [RelatesToExternalID](#RelatesToExternalID), but not both |
| Attributes | None |


**Example:**

```xml
<Extra>
  <ID>EXTRA-11111</ID>
  <Title locale="en-US">Wishenpoof! Trailer</Title>
  <Offers>
    <FreeOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </FreeOffer>
  </Offers>
  <Type>trailer</Type>
</Extra>
```

* * *

## FreeOffer {#FreeOffer}

One of the four offer types. Under this offer, the work is free to view at any time, optionally only during a given window. If necessary, you can have multiple FreeOffer elements under Offers.

**Optional, though at least one offer type is required under Offers:**

| Added | CDF version 1.0 |
| Parent Elements | [Offers](#Offers) |
| Child Elements | Required: [Regions](#Regions) <br/> Optional: [LaunchDetails](#LaunchDetails), [Quality](#Quality) (deprecated), [WindowStart](#WindowStart), [WindowEnd](#WindowEnd) |
| Attributes | None |

**Example:**

```xml
<FreeOffer>
  <Regions>
    <Country>US</Country>
    <Country>CA</Country>
  </Regions>
  <WindowStart>2014-02-06T12:00:00-07:00</WindowStart>
  <WindowEnd>2016-01-01T07:00:00-07:00</WindowEnd>
  <LaunchDetails>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <Subtitle>en-US</Subtitle>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
    <LaunchId>EXTRA-11113_HD_en-US</LaunchId>
  </LaunchDetails>
</FreeOffer>
```

* * *

## Genre {#Genre}

The genre of the work, such as comedy, horror, drama, or documentary. A work can be described through multiple Genre elements if necessary. For optimized search and matching, attempt to use standard genre descriptions. Use multiple Genre tags rather than combine several descriptions into a single string.

**Required if the optional Genres element is present:**

| Added | CDF version 1.0 | | |
| Parent Elements | [Movie](#Genres) | | |
| Child Elements | None | | |
| Attributes |  | |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<Genres>
  <Genre locale="en-US">horror</Genre>
  <Genre locale="en-US">sci-fi</Genre>
</Genres>
```

* * *

## Genres {#Genres}

Contains one or more [Genre](#Genre) tags used to describe the category of the work, such as comedy, horror, or documentary.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [Genre](#Genre) (at least one required when Genres is present) |
| Attributes | None |


**Example**:

```xml
<Genres>
  <Genre locale="en-US">horror</Genre>
  <Genre locale="en-US">sci-fi</Genre>
</Genres>
```

* * *

## ID {#ID}

An identifier string for a work. This value must be at least one character long and unique among all other IDs in your catalog. Two works with the same ID will cause your catalog to be rejected by the ingestion system. Devise an ID scheme and use it unfailingly to avoid duplicate IDs. For instance, you could use your Partner ID + the work type + a long identifier such as a GUID, for an ID such as `AmazonStudios_ Movie_01152ce2-de7e-44c1-9736-e8f3b15a1ddf`. Any scheme that assures unique IDs within your catalog is valid.

When you update an existing catalog, the IDs for your works should not change. If an ID disappears from your catalog, we assume that work is no longer available on your service and remove it from our index.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<Movie>
  <ID>AmazonStudios_Movie_01152ce2-de7e-44c1-9736-e8f3b15a1ddf</ID>
  ...
</Movie>
```

* * *

## ImageUrl {#ImageUrl}

The URL of an image that represents the work, sometimes called the "box art." Each work can contain only a single ImageUrl element. If you don't include ImageUrl, we attempt to use available art from other sources such as IMDb, or we might use a generic placeholder image. See [About the CDF][about-the-cdf] for image requirements.

{% include note.html content="Provide a unique image for each work that applies to the work's content. Do not use a generic placeholder image, such as a logo." %}

**Optional (see Comments):**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Comments | Any one given ImageUrl element is optional, but at least 50% of your entries must include it and the image it points to must be valid. If more than 50% of the images in your file are determined to be invalid (and simply not having an image is considered an invalidity), your uploaded catalog will be rejected by the ingestion system. An image also helps us in matching this work with the same work from other providers, which improves the customer search experience by bundling all offers for the work into a single search result. That, in turn, improves the discoverability of your particular offer. See [About the CDF][about-the-cdf] for image size and height-to-width ratio requirements. |

**Example:**

```xml
<TvShow>
  ...
  <ImageUrl>http://amazon.com/images/01152ce2de7e44c1/image.jpg</ImageUrl>
  ...
</TvShow>
```

* * *

## Job {#Job}

The position held by a work's CrewMember, such as director, cinematographer, writer, or animator. A CrewMember can have as many Job elements as needed.

**Optional:**

| Added | CDF version 1.0 | | |
| Parent Elements | [CrewMember](#CrewMember) | |
| Child Elements | None | |
| Attributes | | |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<CrewMember>
  <Name locale="en-US">Alan Smithee</Name>
  <ExternalID scheme="imdb">tt0000000</ExternalID>
  <Job locale="en-US">Grip</Job>
</CrewMember>
```

* * *

## JP_Require18PlusAgeConfirmation {#JP_Require18PlusAgeConfirmation}

Marks content for the Japanese marketplace intended to be viewed only by persons 18 years of age or older. In compliance with Japan's legal requirements, setting this flag to true requires viewers of this content in Japan to confirm that their age is above 18.

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | true <br/> false |


**Example:**

```xml
<Movie>
  ...
  <JP_Require18PlusAgeConfirmation>true</JP_Require18PlusAgeConfirmation>
</Movie>
```

* * *

## Language {#Language}

The language in which the work was originally produced, which can refer to either the audio or, in the case of a silent work, on-screen text. A work can contain only one Language element. Also use [AudioLanguage](#AudioLanguage) to specify any dubbed options.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR |


**Example:**

```xml
<Movie>
  ...
  <Language>ja</Language>
  ...
</Movie>
```

* * *

## LaunchDetails {#LaunchDetails}

Contains elements that specify a work's available video quality, audio language, and subtitle options under a particular offer. LaunchDetails also contains an optional LaunchId which allows a direct launch of the work in a specific configuration of quality, language, and subtitle.

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [SubscriptionOffer](#SubscriptionOffer), [FreeOffer](#FreeOffer), [PurchaseOffer](#PurchaseOffer), [RentalOffer](#RentalOffer) |
| Child Elements | [Quality](#Quality) (optional), [AudioLanguage](#AudioLanguage) (optional), [Subtitle](#Subtitle) (optional), [LaunchId](#LaunchId) (optional) |
| Attributes | None |

**Example**:

```xml
<FreeOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>SD</Quality>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <AudioLanguage>fr-FR</AudioLanguage>
    <Subtitle>en-US</Subtitle>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
    <LaunchId>EXTRA-11113_HD_en-US</LaunchId>
  </LaunchDetails>
</FreeOffer>
```

* * *

## LaunchId {#LaunchId}

An identifier that allows you to launch a work with a specific configuration of video quality, audio language, and subtitles (or any subset of those three). A LaunchId does not have a given format—the format must only be understood by your app's logic. Each LaunchDetails element can contain only a single LaunchId, so to specify more than one LaunchId, you must include multiple LaunchDetails elements.

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [LaunchDetails](#LaunchDetails) |
| Child Elements | None |
| Attributes | None |


**Example:**

```xml
<SubscriptionOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>SD</Quality>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <AudioLanguage>fr-FR</AudioLanguage>
    <Subtitle>en-US</Subtitle>
    <Subtitle>fr</Subtitle>
    <LaunchId>EXTRA-11113_HD_en-US</LaunchId>
  </LaunchDetails>
  <LaunchDetails>
    <Quality>SD</Quality>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <AudioLanguage>fr-FR</AudioLanguage>
    <Subtitle>en-US</Subtitle>
    <Subtitle>fr</Subtitle>
    <LaunchId>EXTRA-11113_SD_fr-FR_en-US</LaunchId>
  </LaunchDetails>
</SubscriptionOffer>
```

* * *

## MaxValue {#MaxValue}

The highest possible value for a work's customer rating. Each CustomerRating can contain only one MaxValue.

**Required when the optional CustomerRating element is present:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<CustomerRating>
  <Score>8.2</Score>
  <MaxValue>10</MaxValue>
  <Count>512</Count>
</CustomerRating>
```

* * *

## MiniSeries {#MiniSeries}

One of the basic work types, a MiniSeries is loosely defined as a television show that collects a small number of ordered episodes not presented in seasons. There is no explicit limit on the number of episodes a MiniSeries can contain, but it should be reasonably low.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to MiniSeries | [ReleaseDate](#ReleaseDate) (optional) |
| Attributes | None |

**Example:**

```xml
<MiniSeries>
  <ID>MS-2329880</ID>
  <Title locale="en-US">All the Best People</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <ReleaseDate>2005-04-29T20:00:00</ReleaseDate>
</MiniSeries>
```

* * *

## MiniSeriesEpisode {#MiniSeriesEpisode}

One of the basic work types, a MiniSeriesEpisode is a single episode in a MiniSeries. This content is not associated with a season and is sequenced in the context of the MiniSeries.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to MiniSeriesEpisode | Required: [EpisodeInSeries](#EpisodeInSeries) <br/> Required: Either [MiniSeriesID](#MiniSeriesID) or [MiniSeriesTitle](#MiniSeriesTitle), but not both <br/> Optional: [OriginalAirDate](#OriginalAirDate) |
| Attributes | None |

**Example**:

```xml
<MiniSeries>
  <ID>MS-123456789</ID>
  ...
</MiniSeries>
<MiniSeriesEpisode>
  <ID>MSE-2329880</ID>
  <Title locale="en-US">The First Steps on a New Planet</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <MiniSeriesID>MS-123456789</MiniSeriesID>
  <EpisodeInSeries>1</EpisodeInSeries>
  <OriginalAirDate>2012-07-02T20:00:00</OriginalAirDate>
</MiniSeriesEpisode>
```

* * *

## MiniSeriesID {#MiniSeriesID}

Used to specify the mini-series that an episode is a part of. The [MiniSeries](#MiniSeries) with this [ID](#ID) must be present in the same catalog as this MiniSeriesEpisode. You have the option of using MiniSeriesID or [MiniSeriesTitle](#MiniSeriesTitle) to specify the mini-series, but not both. MiniSeriesID should always be used when the mini-series is in your catalog. If it is not in your catalog, we advise you to create a MiniSeries entry to use.

**Required if no MiniSeriesTitle element is present:**

| Added | CDF version 1.3 |
| Parent Elements | [MiniSeriesEpisode](#MiniSeriesEpisode) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<MiniSeries>
  <ID>MS-2329880</ID>
  ...
</MiniSeries>
</MiniSeriesEpisode>
  ...
  <MiniSeriesID>MS-2329880</MiniSeriesID>
  <EpisodeInSeries>3</EpisodeInSeries>
</MiniSeriesEpisode>
```

* * *

## MiniSeriesTitle {#MiniSeriesTitle}

Specifies which mini-series an episode is part of when that mini-series is not part of your catalog. MiniSeriesTitle is simply a string for use in the UI and is not required to match any existing title in your catalog. You have the option of using [MiniSeriesID](#MiniSeriesID) or MiniSeriesTitle to specify the mini-series, but not both. Use MiniSeriesTitle _only_ in the absence of MiniSeriesID, which should be a rare occurrance.

**Required if no MiniSeriesID element is present:**

| Added | CDF version 1.3 |
| Parent Elements | [MiniSeriesEpisode](#MiniSeriesEpisode) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
</MiniSeriesEpisode>
  ...
  <MiniSeriesTitle>Cats, The Most Beautiful Creature</MiniSeriesTitle>
  <EpisodeInSeries>3</EpisodeInSeries>
</MiniSeriesEpisode>
```

* * *

## Movie {#Movie}

One of the basic work types, Movie generally represents a feature-length film, though it can also be used for short films. This work can be a theatrical release or a made-for-TV movie.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to Movie | [ReleaseDate](#ReleaseDate) (optional) |
| Attributes | None |

**Example:**

```xml
<Movie>
  <ID>MV-123456</ID>
  <Title locale="en-US">Chase the Prawns</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <ReleaseDate>2013-10-04T00:00:00</ReleaseDate>
</Movie>
```

* * *

## Name {#Name}

The name of a work's cast or crew member. For a cast member, this is the person's name and not their character's name.

**Required in a CastMember or CrewMember element:**

| Added | CDF version 1.0 | | |
| Parent Elements | [CastMember](#CastMember), [CrewMember](#CrewMember) | | |
| Child Elements | None | | |
| Attributes | | |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<CastMember>
  <Name locale="en-US">Alan Smithee</Name>
  ...
</CastMember>
```

* * *

## Offers {#Offers}

Contains the offers through which a viewer can play a given work: for free, by having a subscription to the service, through rental, or through purchase. Each work type can contain only one Offers element, and that Offers element must contain at least one offer type.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [SubscriptionOffer](#SubscriptionOffer) (optional), [FreeOffer](#FreeOffer) (optional), [PurchaseOffer](#PurchaseOffer) (optional), [RentalOffer](#RentalOffer) (optional) |
| Attributes | None |


**Example:**

```xml
<Offers>
  <FreeOffer>
    <Regions>
      <Country>CA</Country>
    </Regions>
    <WindowStart>2014-02-06T12:00:00-07:00</WindowStart>
    <WindowEnd>2016-01-01T07:00:00-07:00</WindowEnd>
    <LaunchDetails>
      <Quality>SD</Quality>
    </LaunchDetails>
  </FreeOffer>
  <SubscriptionOffer>
    <Regions>
      <Country>CA</Country>
    </Regions>
    <LaunchDetails>
      <Quality>HD</Quality>
    </LaunchDetails>
  </SubscriptionOffer>
</Offers>
```

* * *

## OriginalAirDate {#OriginalAirDate}

The date and time when a work was originally televised. The year portion of this value should match the [ReleaseYear](#ReleaseYear), if that optional element is present. This information is useful in matching this work to content in other catalogs. A match allows us to show a single listing for a work that shows all of its available sources rather than having multiple search results for same thing.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeriesEpisode](#MiniSeriesEpisode) |
| Child Elements | None |
| Attributes | None |
| Accepted values | An XML _dateTime_ value. This value takes the form YYYY-MM-DDThh:mm:ss where YYYY-MM-DD is the year, month, and date and hh:mm:ss is the hour, minute, and second. The 'T' separates the two portions. The entire value is required, from the year down to the second. If the time value is unknown to you, simply use 00:00:00\. You can also add an offset from UTC to the end of the value to account for a particular time zone. |


**Example:**

```xml
<TvSpecial>
  ...
  <OriginalAirDate>2012-05-13T00:00:00</OriginalAirDate>
</TvSpecial>
```

* * *


## Partner {#Partner}

Identifies you as the provider of this catalog. There is no required format, but it should be human-readable. As a good convention, use your app's name as it is seen in the Amazon Appstore. You might also use your full provider name. Each catalog file must contain a single Partner element.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#catalog) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<Catalog xmlns="http://www.amazon.com/FireTv/2014-04-11/ingestion" version="FireTv-v1.3">
  <Partner>Everything Ever Made Filmworks</Partner>
  ...
<Catalog>
```

* * *

## Price {#Price}

The cost to rent or purchase a work.

**Required in PurchaseOffer and RentalOffer:**

| Added | CDF version 1.0 |
| Parent Elements | [PurchaseOffer](#PurchaseOffer), [RentalOffer](#RentalOffer) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | currency | USD <br/> GBP <br/> JPY <br/> EUR | **Required**. The currency in which the price is given. Only one currency can be specified per offer type: dollar, pound, yen, or euro. |

**Example:**    

```xml
<PurchaseOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <Price currency="USD">1.99</Price>
</PurchaseOffer>
```

* * *

## PurchaseOffer {#PurchaseOffer}

One of the four offer types. Under this offer, the work can be purchased for a one-time payment to own and watch anytime. If necessary, you can have multiple PurchaseOffer elements under Offers. 

**Optional, though at least one offer type is required under Offers:**

| Added | CDF version 1.0 |
| Parent Elements | [Offers](#Offers) |
| Child Elements | Required: [Regions](#Regions), [Price](#Price) <br/> Optional: [LaunchDetails](#LaunchDetails), [Quality](#Quality) (deprecated), [WindowStart](#WindowStart), [WindowEnd](#WindowEnd) |
| Attributes | None |


**Example:**

```xml
<PurchaseOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
  </LaunchDetails>
  <Price currency="USD">1.99</Price>
</PurchaseOffer>
```

* * *

## Quality {#Quality}

The visual quality of the work: standard definition (SD), high definition (HD), and ultra high definition (UHD). A work can be offered with multiple visual quality options.

{% include note.html content="There are two elements named Quality. The first, a direct child of each offer type, was deprecated in CDF v1.2\. The second is a child of LaunchDetails, and therefore a grandchild of each offer type. This newer, non-deprecated Quality element is the one we discuss here, although other than their parent elements, the two are identical in form and content." %}

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [LaunchDetails](#LaunchDetails) |
| Child Elements | None |
| Attributes | None |
| Accepted values | SD <br/> HD <br/> UHD |

**Example:**

```xml
<FreeOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>HD</Quality>
    <Quality>UHD</Quality>
  </LaunchDetails>
</FreeOffer>
```

* * *

## Rank {#Rank}

A numerical popularity score relative to the other items in your catalog. The highest rank is defined as 1\. How you determine the rankings is up to you, but it is acceptable for multiple items to have the same rank. A work can have only a single Rank element.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<TvShow>
  ...
  <Rank>36</Rank>
  ...
</TvShow>
```

* * *

## Regions {#Regions}

Contains the countries in which a given offer is available. Each offer type can contain only a single Regions element.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [SubscriptionOffer](#SubscriptionOffer), [FreeOffer](#FreeOffer), [PurchaseOffer](#PurchaseOffer), [RentalOffer](#RentalOffer) |
| Child Elements | [Country](#Country) (at least one required) |
| Attributes | None |

**Example:**

```xml
<FreeOffer>
  <Regions>
    <Country>US</Country>
    <Country>CA</Country>
    <Country>MX</Country>
  </Regions>
</FreeOffer>
```

* * *

## RelatesToExternalID {#RelatesToExternalID}

Used to specify another work (such as a movie) with which an Extra (such as a trailer for that movie) is associated. RelatesToExternalID specifies an identifier by which that other work (such as the movie) is known in an external classificiation such as IMDb. RelatesToExternalID is used when the associated work is not a part of your catalog. You have the option of using [RelatesToID](#RelatesToID) or RelatesToExternalID to specify the association, but not both. Use RelatesToExternalID _only_ in the absence of RelatesToID.

Do not confuse RelatesToExternalID with [ExternalID](#ExternalID). ExternalID refers to the Extra itself, while RelatesToExternalID refers to the work that it's associated with.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Extra](#Extra) |
| Child Elements | None |
| Attributes |
| | Attribute | Description | Accepted Values |
| | scheme | **Required**. The external source that provided this ID. | imdb <br/> tms <br/> isan <br/> ean <br/> upc |
| Comments <br/>(the values represent these sources)| |
| | Value | Description |
| | imdb | The Internet Movie Database (IMDb). IDs can be found as part of the URL of a given page. |
| | tms | The ID used in the Gracenote™ database. |
| | isan | The International Standard Audiovisual Number (ISAN), an alphanumeric strings of 26 characters, usually presented broken by dashes. |
| | ean | The International Article Number (EAN), a barcode normally expressed in 13-digits. |
| | upc | The Universal Product Code (UPC), a barcode normally expressed in 12-digits. |

**Example:**

```xml
<Extra>
  ...
  <Type>trailer</Type>
  <RelatesToExternalID scheme="imdb">tt0000000</RelatesToExternalID>
  <RelatesToExternalID scheme="tms">MV000000000000</RelatesToExternalID>
  <RelatesToExternalID scheme="isan">0000-0000-0F00-0000-X-0000-0000-Y</RelatesToExternalID>
  <RelatesToExternalID scheme="ean">0011559514120</RelatesToExternalID>
  <RelatesToExternalID scheme="upc">123456789990</RelatesToExternalID>
</Extra>
```

* * *

## RelatesToID {#RelatesToID}

Used to specify another work (such as a movie) with which an Extra (such as a trailer for that movie) is associated. The work with this [ID](#ID) must be present in the same catalog as this Extra. You have the option of using RelatesToID or [RelatesToExternalID](#RelatesToExternalID) to specify the associated work, but not both. RelatesToID should always be used when the associated work is in your catalog.

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<TvShow>
  <ID>TV123456</ID>
  ...
</TvShow>
<Extra>
  ...
  <Type>trailer</Type>
  <RelatesToID>TV123456</RelatesToID>
</Extra>
```

* * *

## ReleaseCountry (deprecated) {#ReleaseCountry}

**Deprecated, do not use.** This element has no replacement.

The country that originally released the work.

**Optional:**

| Added | CDF version 1.0 |
| Deprecated | CDF version 1.3 |
| Parent Elements | [ReleaseInfo](#ReleaseInfo) (deprecated) |
| Child Elements | None |
| Attributes | None |
| Accepted values | The following subset of [ISO 3166 country codes](http://www.iso.org/iso/country_codes.htm): <br/> AF AX AL DZ AS AD AO AI AQ AG AR AM AW AU AT AZ BS BH BD BB BY BE BZ BJ BM BT BO BQ BA BW BV BR IO BN BG BF BI KH CM CA CV KY CF TD CL CN CX CC CO KM CG CD CK CR CI HR CU CW CY CZ DK DJ DM DO EC EG SV GQ ER EE ET FK FO FJ FI FR GF PF TF GA GM GE DE GH GI GR GL GD GP GU GT GG GN GW GY HT HM VA HN HK HU IS IN ID IR IQ IE IM IL IT JM JP JE JO KZ KE KI KP KR KW KG LA LV LB LS LR LY LI LT LU MO MK MG MW MY MV ML MT MH MQ MR MU YT MX FM MD MC MN ME MS MA MZ MM NA NR NP NL NC NZ NI NE NG NU NF MP NO OM PK PW PS PA PG PY PE PH PN PL PT PR QA RE RO RU RW BL SH KN LC MF PM VC WS SM ST SA SN RS SC SL SG SX SK SI SB SO ZA GS SS ES LK SD SR SJ SZ SE CH SY TW TJ TZ TH TL TG TK TO TT TN TR TM TC TV UG UA AE GB US UM UY UZ VU VE VN VG VI WF EH YE ZM ZW |

**Example:**

```xml
<TvEpisode>
  ...
  <ReleaseInfo>
    <ReleaseDate>2002-02-20</ReleaseDate>
    <ReleaseCountry>BT</ReleaseCountry>
  </ReleaseInfo>
  ...
</TvEpisode>
```

* * *

## ReleaseDate {#ReleaseDate}

The date and time when the work was originally released to the public, or the first air date in the case of television. The year portion of this value should match the [ReleaseYear](#ReleaseYear), if that optional element is present. This information is particularly useful in matching this work to content in other catalogs. A match allows us to show a single listing for a work that shows all of its available sources rather than having multiple search results for same thing.

{% include note.html content="For the deprecated element of the same name, see [ReleaseDate (deprecated)](#ReleaseDateDep)." %}

**Optional:**

| Added | CDF version 1.3 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [MiniSeries](#MiniSeries) |
| Child Elements | None |
| Attributes | None |
| Accepted values | An XML _dateTime_ value. This value takes the form YYYY-MM-DDThh:mm:ss where YYYY-MM-DD is the year, month, and date and hh:mm:ss is the hour, minute, and second. The 'T' separates the two portions. The entire value is required, from the year down to the second. If the time value is unknown to you, simply use 00:00:00\. You can also add an offset from UTC to the end of the value to account for a particular time zone. |

**Example:**

```xml
<TvShow>
  ...
  <ReleaseDate>2012-05-13T00:00:00</ReleaseDate>
</TvShow>
```

* * *

## ReleaseDate (deprecated) {#ReleaseDateDep}

**Deprecated, do not use**. Use [ReleaseDate](#ReleaseDate) (same name, different location and data type) or [OriginalAirDate](#OriginalAirDate) instead.

The date when the work was originally released to the public, or the first air date in the case of television. The year portion of this value should match the [ReleaseYear](#ReleaseYear), if that optional element is present. This information is particularly useful in matching this work to content in other catalogs. A match allows us to show a single listing for a work that shows all of its available sources rather than having multiple search results for same thing.

**Required in ReleaseInfo:**

| Added | CDF version 1.0 |
| Deprecated | CDF version 1.3 |
| Parent Elements | [ReleaseInfo](#ReleaseInfo) (deprecated) |
| Child Elements | None |
| Attributes | None |
| Accepted values | An XML _date_ value. This value takes the form YYYY-MM-DD (year, month, and date). |


**Example:**

```xml
<TvShow>
  ...
  <ReleaseInfo>
    <ReleaseDate>2012-05-13</ReleaseDate>
  </ReleaseInfo>
</TvShow>
```

* * *

## ReleaseInfo (deprecated) {#ReleaseInfo}

**Deprecated, do not use**. For release date information, use [ReleaseDate](#ReleaseDate) or [OriginalAirDate](#OriginalAirDate) as appropriate to the work type. Release country information is no longer used.

Contains elements that specify a work's country of release and release date.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [ReleaseDate](#ReleaseDateDep) (required, deprecated), [ReleaseCountry](#ReleaseCountry) (optional, deprecated) |
| Attributes | None |

**Example:**

```xml
<TvEpisode>
  ...
  <ReleaseInfo>
    <ReleaseDate>2002-02-20</ReleaseDate>
    <ReleaseCountry>BT</ReleaseCountry>
  </ReleaseInfo>
  ...
</TvEpisode>
```

* * *

## ReleaseYear {#ReleaseYear}

The year in which the work was originally released to the public, or the first air date in the case of television. Note that this value should match the year given in the same work's [ReleaseDate](#ReleaseDate) or [OriginalAirDate](#OriginalAirDate) element.

**Optional, but highly recommended:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Comments | While this element is optional, we recommend that you include it. This value is particularly helpful in allowing us to match this work with the same work from other providers, which improves the customer search experience by bundling all offers for the work into a single search result. That, in turn, improves the discoverability of your particular offer. |

**Example:**

```xml
<TvEpisode>
  ...
  <ReleaseYear>1959</Releaseyear>
  ...
</TvEpisode>
```

* * *

## RentalOffer {#RentalOffer}

One of the four offer types. Under this offer, the work can be viewed for a limited amount of time for a one-time payment. If necessary, you can have multiple RentalOffer elements under Offers.

**Optional, though at least one offer type is required under Offers:**

| Added | CDF version 1.0 |
| Parent Elements | [Offers](#Offers) |
| Child Elements | Required: [Regions](#Regions), [Price](#Price), [Duration](#Duration) <br/> Optional: [LaunchDetails](#LaunchDetails), [Quality](#Quality) (deprecated), [WindowStart](#WindowStart), [WindowEnd](#WindowEnd) |
| Attributes | None |

**Example:**

```xml
<RentalOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
  </LaunchDetails>
  <Price currency="USD">1.99</Price>
  <Duration>120</Duration>
</RentalOffer>
```

* * *

## Role {#Role}

The character's name in a work, as played by a CastMember. Examples are Robin Hood, Sir Lancelot du Lac, Athena, or Self. Do not use "actor" (all CastMember entries are actors) or "unknown" for this value. A CastMember element can contain multiple Role elements if that person played multiple roles.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [CastMember](#CastMember) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<Credits>
  <CastMember>
    <Name locale="en-US">Alan Smithee</Name>
    <ExternalID scheme="imdb">tt0000000</ExternalID>
    <Role locale="en-US">Robin Hood</Role>
    <Role locale="en-US">Self</Role>
  </CastMember>
</Credits>
```

* * *

## RuntimeMinutes {#RuntimeMinutes}

The overall running time of the content, in minutes. This is a non-negative number and is expected to be less than 2880, though there may be instances that legitimately exceed that value. Each work can have only one RuntimeMinutes element.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<TvEpisode>
  ...
  <RuntimeMinutes>37</RuntimeMinutes>
  ...
</TvEpisode>
```

* * *

## Score {#Score}

An average score (rating) for a work based on customer feedback. How you gather that information is up to you, as well as setting the [MaxValue](#MaxValue) to give a scale to the rating system. You can add an optional [Count](#Count) of how many votes contributed to the Score. A CustomerRating can contain only a single Score.

**Required in CustomerRating:**

| Added | CDF version 1.0 |
| Parent Elements | [CustomerRating](#CustomerRating) |
| Child Elements | None |
| Attributes | None |
| Example |

```xml
<CustomerRating>
  <Score>8.2</Score>
  <MaxValue>10</MaxValue>
  <Count>512</Count>
</CustomerRating>
```

* * *

## SeasonID {#SeasonID}

The ID of the season of which a TVEpisode is a part. The [TVSeason](#TVSeason) with this [ID](#ID) must be present in the same catalog as this TvEpisode. You have the option of using SeasonID or [SeasonInShow](#SeasonInShow) to specify the season, but not both. SeasonID should always be used when the season is in your catalog. If is not in your catalog, consider creating a TvSeason entry.

**Required if no SeasonInShow element is present:**

| Added | CDF version 1.0 |
| Parent Elements | [TvEpisode](#TvEpisode) |
| Child Elements | None |
| Attributes | None |
| Example |

```xml
<TvSeason>
  <ID>SEA-2329880</ID>
  ...
</TvSeason>
<TvEpisode>
  ...
  <SeasonID>SEA-2329880</SeasonID>
  ...
</TvEpisode>
```


* * *

## SeasonInShow {#SeasonInShow}

The number of a season that a TVEpisode is a part of, when that season is not part of your catalog. SeasonInShow is simply a number for use in the UI and is not required to match anything. You have the option of using [SeasonID](#SeasonID) or SeasonInShow to specify the season, but not both. Use SeasonInShow _only_ in the absence of SeasonID, which should be a rare occurrance.

**Required if no SeasonID element is present:**

| Added | CDF version 1.0 |
| Parent Elements | [TvEpisode](#TvEpisode) |
| Child Elements | None |
| Attributes | None |

**Example:**

```xml
<TvEpisode>
  ...
  <SeasonInShow>2</SeasonInShow>
  ...
</TvEpisode>
```

* * *

## SeasonTitle {#SeasonTitle}

A title for the season in which a TvEpisode appeared, such as "Season 2". Note that if the corresponding TvSeason element is included in your catalog, this value is not required to match its [Title](#Title), though it should. Each TvEpisode can have only one SeasonTitle.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [TvEpisode](#TvEpisode) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<TvEpisode>
  ...
  <SeasonInShow>2</SeasonInShow>
  <SeasonTitle locale="en-US">Season 2</SeasonTitle>
  ...
</TvEpisode>
```

* * *

## ShortDescription {#ShortDescription}

A two- or three-line description of a work's content. Do not use information included elsewhere, such as the work's title, for the ShortDescription. Each work can contain multiple ShortDescription elements for the purpose of providing localized descriptions. To provide a longer, more detailed description, use the [Synopsis](#Synopsis) element.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's l anguage setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<TvSpecial>
  ...
  <ShortDescription locale="en-US">Alan shows us some trees and sings songs about them.</ShortDescription>
  ...
</TvSpecial>
```

* * *

## ShowID {#ShowID}

Used to tie a TvEpisode, TvSeason, or TvSpecial to a TvShow in your catalog. This value must match the [ID](#ID) value in a TvShow element. You have the option of using ShowID or [ShowTitle](#ShowTitle) to specify the show, but not both. Always use ShowID when the TvShow is in your catalog.

**Required in the absence of ShowTitle for TvEpisode and TvSeason:**<br/>
**Optional for TvSpecial:**

| Added | CDF version 1.0 |
| Parent Elements | [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial) |
| Child Elements | None |
| Attributes | None |
| Accepted values |

**Example:**

```xml
<TvShow>
  <ID>TV-2329880</ID>
  ...
</TvShow>
<TvSeason>
  ...
  <ShowID>TV-2329880</ShowID>
  ...
</TvSeason>
<TvEpisode>
  ...
  <ShowID>TV-2329880</ShowID>
  ...
</TvEpisode>
```

* * *

## ShowTitle {#ShowTitle}

Used to tie a TvEpisode, TvSeason, or TvSpecial to a TvShow in your catalog. This value is simply a string for use in the UI and is not required to match any title in your catalog. You have the option of using [ShowID](#ShowID) or ShowTitle to specify the show, but not both. Use ShowTitle _only_ in the absence of ShowID.

**Required in the absence of ShowID for TvEpisode and TvSeason:**<br/>
**Optional for TvSpecial:**

| Added | CDF version 1.0 |
| Parent Elements | [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<TvSeason>
  ...
  <ShowTitle locale="en-US">Depth of Field</ShowTitle>
  ...
</TvSeason>
```

* * *

## Source {#Source}

Where the work originated. Each work can have only one Source.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | original <br/> licensed <br/> unknown <br/> other |
| Comments <br/>(the values represent these sources) | |
| | Value | Description |
| | original | The provider of this catalog produced or created this content. |
| | licensed | The provider of this catalog has licensed the work from another party. |
| | unknown | The source of this content is unknown. |
| | other | The source of the work is known but is neither original nor licensed. |

**Example:**

```xml
<MiniSeriesEpisode>
  ...
  <Source>licensed</Source>
  ...
</MiniSeriesEpisode>
```

* * *

## Studio {#Studio}

The studio that produced the work. A work can have multiple Studio entries.

**Required in the optional Studios element:**

| Added | CDF version 1.0 |
| Parent Elements | [Studios](#Studios) |
| Child Elements | None |
| Attributes | None |
| Example |

```xml
<TvEpisode>
  ...
  <Studios>
    <Studio>Amazon Studios</Studio>
    <Studio>Another Production Company</Studio>
  </Studios>
  ...
</TvEpisode>
```

* * *

## Studios {#Studios}

Contains one or more Studio elements that identify the studio(s) that produced the work.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | [Studio](#Studio) (at least one required) |
| Attributes | None |

**Example**:

```xml
<TvEpisode>
  ...
  <Studios>
    <Studio>Amazon Studios</Studio>
    <Studio>Another Production Company</Studio>
  </Studios>
  ...
</TvEpisode>
```

* * *

## SubscriptionOffer {#SubscriptionOffer}

One of the four offer types. Under this offer, the work can be watched by subscribers to the provider's service. If necessary, you can have multiple SubscriptionOffer elements under Offers.

**Optional, though at least one offer type is required under Offers:**

| Added | CDF version 1.0 |
| Parent Elements | [Offers](#Offers) |
| Child Elements | Required: [Regions](#Regions) <br/> Optional: [LaunchDetails](#LaunchDetails), [Quality](#Quality) (deprecated), [WindowStart](#WindowStart), [WindowEnd](#WindowEnd) |
| Attributes | None |

 **Example:**

```xml
<SubscriptionOffer>
  <Regions>
    <Country>US</Country>
    <Country>CA</Country>
  </Regions>
  <WindowStart>2014-02-06T12:00:00-07:00</WindowStart>
  <WindowEnd>2016-01-01T07:00:00-07:00</WindowEnd>
  <LaunchDetails>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <Subtitle>en-US</Subtitle>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
    <LaunchId>EXTRA-11113_HD_en-US</LaunchId>
  </LaunchDetails>
</SubscriptionOffer>
```

* * *

## Subtitle {#Subtitle}

A language option for the work's subtitles. A work can have multiple subtitle options.

**Optional:**

| Added | CDF version 1.2 |
| Parent Elements | [LaunchDetails](#LaunchDetails) |
| Child Elements | None |
| Attributes | None |
| Accepted values | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR |

**Example:**

```xml
<SubscriptionOffer>
  <Regions>
    <Country>US</Country>
  </Regions>
  <LaunchDetails>
    <Quality>HD</Quality>
    <AudioLanguage>en-US</AudioLanguage>
    <Subtitle>fr</Subtitle>
    <Subtitle>es</Subtitle>
  </LaunchDetails>
</SubscriptionOffer>
```

* * *

## Synopsis {#Synopsis}

A description of a work's content. Synopsis is intended to give more detail than [ShortDescription](#ShortDescription). Do not use the ShortDescription or the work's [Title](#Title) as the Synopsis. Each work can contain multiple Synopsis elements for the purpose of providing localized descriptions.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<TvSpecial>
  ...
  <ShortDescription locale="en-US">Alan shows us some trees and sings songs about them.</ShortDescription>
  <Synopsis locale="en-US">Alan Smithee, man about town and fervent urban arborist, takes us on a
    musical journey around his home town, stopping by some favorite trees to sing about them. As
    expected from Mr. Smithee, no path runs straight and his plans meander as he encounters guest
    stars and battles a lumberjack with a literal ax to grind.</Synopsis>
  ...
</TvSpecial>
```

* * *

## System {#System}

The rating system, normally an official organization, that determined a work's rating. Each rating can have only one System.

**Required in ContentRating:**

| Added | CDF version 1.0 |
| Parent Elements | [ContentRating](#ContentRating) |
| Child Elements | None |
| Attributes | None |


**Example:**

```xml
<ContentRatings>
  <ContentRating>
    <System>MPAA</System>
    <Certification>G</Certification>
  </ContentRating>
</ContentRatings>
```

* * *

## Title {#Title}

A work's title. Each work can contain multiple Title elements for the purpose of providing localized titles.

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Child Elements | None |
| Attributes |
| | Attribute | Accepted Values | Description |
| | locale | Standard XML/HTML language codes, such as en, en-US, fr, or fr-FR | **Required**. The device or software's language setting under which to use this string. |
| | pronunciation | String | **Optional**. Used when the element's text is given in kanji. The expected sort order in Japanese is based on pronunciation (which cannot be determined from the kanji) rather than characters. The _pronunciation_ attribute provides that information, typically using hiragana. |

**Example:**

```xml
<TvShow>
  <ID>TV123456</ID>
  <Title locale="en-US">Office Factor</Title>
  ...
</TvShow>
```

* * *

## TvEpisode {#TvEpisode}

One of the basic work types, a TvEpisode is a single episode of a TvShow, normally also associated with a TvSeason.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements <br/>Specific to TvEpisode | Required: [ShowID](#ShowID) or [ShowTitle](#ShowTitle), but not both <br/> Required: [SeasonID](#SeasonID) or [SeasonInShow](#SeasonInShow), but not both <br/> Required: [EpisodeInSeason](#EpisodeInSeason) <br/> Optional: [SeasonTitle](#SeasonTitle), [OriginalAirDate](#OriginalAirDate) |
| Attributes | None |

**Example:**

```xml
<TvShow>
  <ID>ABC-123457</ID>
  ...
</TvShow>
<TvSeason>
  <ID>TVS-987654</ID>
  ...
</TvSeason>
<TvEpisode>
  <ID>TVE2329880</ID>
  <Title locale="en-US">What's in a Name?</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <ShowID>ABC-123457</ShowID>
  <SeasonID>TVS-987654</SeasonInShow>
  <EpisodeInSeason>5</EpisodeInSeason>
</TvEpisode>
```

* * *

## TvSeason {#TvSeason}

One of the basic work types, a TvSeason is a single season of a TvShow. When a TvEpisode's [SeasonID](#SeasonID) value is the same as the TvSeason's [ID](#ID), that episode declares itself part of the season.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to TvSeason | Required: [ShowID](#ShowID) or [ShowTitle](#ShowTitle), but not both <br/> Required: [SeasonInShow](#SeasonInShow) |
| Attributes | None |

**Example:**

```xml
<TvShow>
  <ID>ABC-123457</ID>
  ...
</TvShow>
<TvSeason>
  <ID>TVS2329880</ID>
  <Title locale="en-US">Season Five</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <ShowID>ABC-123457</ShowID>
  <SeasonInShow>5</SeasonInShow>
</TvSeason>
<TvEpisode>
  ...
  <ShowID>ABC-123457</ShowID>
  <SeasonID>TVS2329880</SeasonID>
  ...
</TvEpisode>
```

* * *

## TvShow {#TvShow}

One of the basic work types, a TvShow is a televised series made up of seasons and episodes. It can also have associated specials outside of the regular sequence of episodes. When a TvSeason, TvEpisode, or TvSpecial's [ShowID](#ShowID) value is the same as the TvShow's [ID](#ID), that season, episode, or special declares itself part of the show.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/>Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to TvShow | [ReleaseDate](#ReleaseDate) (optional) |
| Attributes | None |

**Example:**

```xml
<TvShow>
  <ID>RS-2329880</ID>
  <Title locale="en-US">Office Factor</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
</TvShow>
<TvSeason>
  <ID>TVS2329880</ID>
  ...
  <ShowID>RS-2329880</ShowID>
  ...
</TvSeason>
<TvEpisode>
  ...
  <ShowID>RS-2329880</ShowID>
  <SeasonID>TVS2329880</SeasonID>
  ...
</TvEpisode>
```

* * *

## TvSpecial {#TvSpecial}

One of the basic work types, TvSpecial is used for televised events that don't belong to the traditional show-season-episode television hierarchy. These can be one-time events such as a holiday special, or they can account for programs in which each episode has a unique airdate rather than an episode number (news programs, for instance). A TvSpecial can be associated with a TvShow (though not a TvSeason) or it can be a standalone event. Some other examples are awards programs, televised concerts, and retrospectives.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements Common to All Work Types | Required: [ID](#ID), [Offers](#Offers), [Title](#Title) <br/> Optional: [AdultProduct](#AdultProduct) (deprecated), [Color](#Color), [ContentRatings](#ContentRatings), [JP_Require18PlusAgeConfirmation](#JP_Require18PlusAgeConfirmation), [Copyright](#Copyright), [Credits](#Credits), [CustomerRating](#CustomerRating), [ExternalID](#ExternalID), [Genres](#Genres), [ImageUrl](#ImageUrl), [Language](#Language), [Rank](#Rank), [ReleaseInfo](#ReleaseInfo) (deprecated), [ShortDescription](#ShortDescription), [ReleaseYear](#ReleaseYear), [RuntimeMinutes](#RuntimeMinutes), [Source](#Source), [Studios](#Studios), [Synopsis](#Synopsis) |
| Child Elements Specific to TvShow | Required: [OriginalAirDate](#OriginalAirDate) (added in CDF v1.3)
Optional: [ShowID](#ShowID) or [ShowTitle](#ShowTitle), but not both (added in CDF v1.3) |
| Attributes | None |

**Example:**

```xml
<TvSpecial>
  <ID>SP-2329880</ID>
  <Title locale="en-US">Cheese: Friend or Enemy?</Title>
  <Offers>
    <SubscriptionOffer>
      <Regions>
        <Country>US</Country>
      </Regions>
    </SubscriptionOffer>
  </Offers>
  <OriginalAirDate>2005-04-29T20:00:00</OriginalAirDate>
</TvSpecial>
```

* * *

## Type {#Type}

Specifies an Extra as a trailer (preview) or a clip. You can regard anything that isn't a trailer as a clip, though it might be something as extensive as a making-of documentary about the Extra's associated Movie. Each Extra can have only one Type.

**Required:**

| Added | CDF version 1.3 |
| Parent Elements | [Extra](#Extra) |
| Child Elements | None |
| Attributes | None |
| Accepted values | clip <br/> trailer |

**Example:**

```xml
<Extra>
  ..
  <Type>trailer</Type>
  ...
</Extra>
```

* * *

## WindowEnd {#WindowEnd}

The date and time after which the work will no longer be available under a particular offer. Each offer can have only one WindowEnd. Using [WindowStart](#WindowStart) and WindowEnd, you can declare an offer to be available only for a specific window of time. After the time specified by WindowEnd, the offer is no longer presented to the viewer. If all offers have expired, the work itself is not shown to the viewer.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [SubscriptionOffer](#SubscriptionOffer), [FreeOffer](#FreeOffer), [PurchaseOffer](#PurchaseOffer), [RentalOffer](#RentalOffer) |
| Child Elements | None |
| Attributes | None |
| Accepted values | An XML _dateTime_ value. This value takes the form YYYY-MM-DDThh:mm:ss where YYYY-MM-DD is the year, month, and date and hh:mm:ss is the hour, minute, and second. The 'T' separates the two portions. The entire value is required, from the year down to the second. You can also add an offset from UTC to the end of the value to account for a particular time zone. |
| Comments | WindowsStart and WindowsEnd can be used together or separately to control a work's availablility under an offer. <br/><br/>&bull; WindowsStart only: The work is available indefinitely from that time forward unless it is removed from the catalog. <br/><br/>&bull; WindowsEnd only: The work is available immediately, but only until that time. <br/><br/>&bull; WindowsStart + WindowsEnd: The work is available only in that window of time. <br/><br/>&bull; Neither WindowsStart nor WindowsEnd: The work is immediately available and always will be unless it is removed from the catalog

**Example:**

```xml
<FreeOffer>
  ...
  <WindowStart>2014-02-06T12:00:00-07:00</WindowStart>
  <WindowEnd>2016-01-01T07:00:00-07:00</WindowEnd>
  ...
</FreeOffer>
```

* * *

## WindowStart {#WindowStart}

The date and time after which the work becomes available under a particular offer. Using WindowStart and [WindowEnd](#WindowEnd), you can declare an offer to be available only for a specific window of time. Before and after that window, that offer is not shown to the viewer. If no offer is available at the time, the work itself is not shown to the viewer. Each offer can have only one WindowStart.

**Optional:**

| Added | CDF version 1.0 |
| Parent Elements | [SubscriptionOffer](#SubscriptionOffer), [FreeOffer](#FreeOffer), [PurchaseOffer](#PurchaseOffer), [RentalOffer](#RentalOffer) |
| Child Elements | None |
| Attributes | None |
| Accepted values | An XML _dateTime_ value. This value takes the form YYYY-MM-DDThh:mm:ss where YYYY-MM-DD is the year, month, and date and hh:mm:ss is the hour, minute, and second. The 'T' separates the two portions. The entire value is required, from the year down to the second. You can also add an offset from UTC to the end of the value to account for a particular time zone. |
| Comments | WindowsStart and WindowsEnd can be used together or separately to control a work's availablility under an offer. <br/><br/>&bull; WindowsStart only: The work is available indefinitely after that time unless it is removed from the catalog. <br/><br/>&bull; WindowsEnd only: The work is available immediately, but only until that time <br/><br/>&bull; WindowsStart + WindowsEnd: The work is available only in that window of time <br/><br/>&bull; Neither WindowsStart nor WindowsEnd: The work is available immediately and always will be unless it is removed from the catalog


**Example:**

```xml
<FreeOffer>
  ...
  <WindowStart>2014-02-06T12:00:00-07:00</WindowStart>
  <WindowEnd>2016-01-01T07:00:00-07:00</WindowEnd>
  ...
</FreeOffer>
```

* * *

## Works {#Works}

Contains all of the individual entriesin your catalog: movies, TV shows, seasons, specials, mini-series, episodes, and extras. Each catalog file must contain a single Works element. The Works element can contain as many work entries as needed, and as many of each type as needed.

{% include note.html content="In CDF v1.0 to v1.2, the Works element was required to contain at least a single work. As of CDF v1.3, it can be empty. This will have the effect of removing your catalog's contents from Amazon Fire TV's universal browse and search." %}

**Required:**

| Added | CDF version 1.0 |
| Parent Elements | [Catalog](#Catalog) |
| Child Elements | [Movie](#Movie), [TvShow](#TvShow), [TvSeason](#TvSeason), [TvEpisode](#TvEpisode), [TvSpecial](#TvSpecial), [MiniSeries](#MiniSeries), [MiniSeriesEpisode](#MiniSeriesEpisode), [Extra](#Extra) |
| Attributes | None |
| Accepted values |

**Example:**

```xml
<Catalog>
  <Partner>Everything Ever Made Filmworks</Partner>
  <Works>
    ...
  </Works>
</Catalog>
```

{% include links.html %}
