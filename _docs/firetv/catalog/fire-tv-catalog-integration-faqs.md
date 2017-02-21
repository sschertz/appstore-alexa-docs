---
title: Fire TV Catalog Integration Frequently Asked Questions (FAQs)
permalink: fire-tv-catalog-integration-faqs.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

This page contains some of the most Frequently Asked Questions (FAQs) related to Fire TV Catalog Integration (sometimes called Universal Browse and Search).

* TOC
{:toc}

## Getting Started with Fire TV Catalog Ingestion

Q: What is Fire TV catalog integration?
: A: Catalog integration enables your catalog metadata to be included with other catalog data on the Fire TV platform. When a user searches for media or browses to a specific item in the Fire TV user interface, your content appears alongside the content from Amazon and from other providers and can be played directly. Catalog integration makes your content easier to find and more available to your viewers.

Q: What is the typical process for integrating my catalog with Fire TV?
:   A: Most apps typically use the following process for integrating their catalogs with Fire TV:

    1. Export your data from a database and transform the data to a valid XML file that is formatted using our Catalog Data Format (CDF) schema. See [About the Catalog Data Format][about-the-cdf], [Fire TV Catalog Data Format (CDF) Schema][catalog-data-format-schema-reference], and download the XSD schema file: [CDF Schema Definition][3]. (If for whatever reason, you are unable to programmatically generate your CDF file, you can still create the XML file by hand, using the CDF schema as a template to help create your file. If your organization needs to create its CDF files by hand, Amazon recommends that you assign this task to a person within your org who is familiar and comfortable working with XML.)
    2. Upload your CDF XML file to Amazon's AWS S3 service. See [Uploading Your Catalog][uploading-your-catalog].
    3. Configure Fire TV's home screen launcher to work with your uploaded catalog. This step also involves making a few changes to your app. See [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]. You can [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb] or [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app].

Q: How often should we go through this process of updating our catalog?   
:   A: As a best practice, Amazon recommends resourcing an engineer or developer to automate the catalog export and upload process. Once automated, set the process to run weekly, regardless of whether there have been any catalog updates.

Q: Some of this sounds pretty technical. Who in my org should be handling this process?
:  A: Preferably, an engineer or IT professional will be handling the creation of and uploading of your CDF file. Amazon highly recommends having an engineer automate this process. If an engineer is unavailable, Amazon strongly recommends that the person creating and uploading the CDF file be comfortable working with XML and be comfortable executing commands using a command line interface.

## User Experience

Q: How does catalog integration drive traffic to my app?   
:   A: Integrating your app's catalog with Fire TV can drive traffic to your app in several ways:

    * Encourage new users to download your app and enroll in a free trial subscription (if applicable).
    * Increased opportunities to surface your content through browse and search.
    * Retaining existing customers through prioritized browse and search results that favor current subscribers.

    For more benefits of catalog integration, see [Understanding Fire TV Catalog Integration][understanding-fire-tv-catalog-integration].

Q: When a user selects an episode or series through browse or search, where should playback start from?   
:   A: For a good user experience, playback should start by deep linking directly to the episode or series from the search or browse result. The user should not be routed to your app's home screen or anywhere else.

Q: When a user selects an episode or series, how immediately should playback begin?
:   A: Playback time can vary by app depending on buffering and other factors; however playback should begin directly after selection without any intermediate steps.Users should not be redirected to your app's home screen or elsewhere before beginning playback.

Q: When playback completes, where should the user land?   
:   A: Users should land in the same place that they would expect to land if they had reached the content through your app as opposed to from an integrated catalog. For some apps, the next episode begins playing automatically. For other apps, users are taken to their personal recommendations. Just keep your user experience consistent whether they access your content through Fire TV or from within your app.

## Creating the CDF file

Q: How do I create my CDF file?   
:   A: Ideally, you would create this file programmatically by exporting your metadata to an XML file that fits the CDF format. To learn more about creating your CDF file, see [Getting Started with Fire TV Catalog Integration][getting-started-catalog-integration] and the [About the Catalog Data Format][about-the-cdf].

Q: Ok, I've created my CDF file. Now what? Can I just upload it to AWS S3?    
:   A: Amazon strongly recommends validating the XML in your CDF file before uploading it to AWS S3. If your CDF contains any poorly formed or invalid XML, the file will be rejected by Amazon.

Q: How do I validate my XML? Do you provide any utility that I can use to check my file before uploading?    
:   A: Amazon does not provide a utility for validating CDF files; however, many of these utilities are easily available. If you are on a Mac or Linux, use [xmllint][10] to validate your CDF file. This utility should be pre-installed with your OS. (If you are on a Windows machine, you can use the Google project version of xmllint.

## Uploading and Publishing Your CDF File

Q: How do I upload my validated CDF file to Amazon?   
:   A: To upload your CDF file, see [Uploading Your Catalog to Amazon][uploading-your-catalog].

Q: What's the maximum frequency at which we can publish a CDF file to our AWS S3 bucket?    
:   A: Amazon does not have limits on publishing frequency; we adjust our ingestion pipeline as appropriate for the volume of uploaded files.

Q: After uploading it to my S3 buck, how soon can I expect my CDF file to be ingested?    
:   A: Amazon retrieves new catalog uploads from partner S3 buckets every four hours. If your CDF file can be successfully ingested, it will be ingested at this time. If your CDF file fails ingestion, you'll need to fix your file, re-upload, and wait for the next four hour pickup window. If your catalog update must be faster than this, please discuss this with your Amazon representative. Note that this four hour pickup window is subject to change.

Q: What is the delay between a CDF file upload and the content being available on the Fire TV device?    
:   A: Typically content will be available to customers within 2-4 hours after the catalog is ingested; however, due to possible caching and other conditions, content may take as long as 72 hours to become available for some apps.

Q: Do we need to upload a full CDF file every time we update? Can we upload a CDF file that only includes new and updated data?    
:   A: You need to provide the full CDF file with each upload. Amazon uses the full file to calculate what needs to be deleted. Additionally, having the full file helps ensure that Amazon's catalog will not diverge from yours over time.

Q: What if we send several CDF updates within quick succession of each other? What happens if we produce CDF updates faster than Amazon can process them?    
:   A: If you upload your CDF files faster than Amazon can process them, Amazon will just ignore the older copies and use the most recent file version. In other words, you cannot overload Amazon's system by uploading an abundance of catalogs to the S3 system.

Q: Does Amazon provide any tools to test my catalog integration?    
:   A: You can test your app's integration with Amazon's home screen launcher using the Android Debug Bridge (ADB). See [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb]. You can also download and try out Amazon's test app, which simulates the requests made by Amazon's home screen launcher and can be used to test your app's integration with the launcher. See [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app]. Additionally, to assist you with testing, we can whitelist specific Fire TV devices on request.

Q: Do I need to rename my CDF file every time that I upload a new version of it to S3? How does Amazon determine which CDF file to use if my S3 bucket contains multiple files?    
:   A: Amazon uses timestamps to determine the newest version of your CDF file and will always ingest and use the most recent version.   

## Seasons, Episodes, and Other Content Types

Q: I have a TV show with seasons and episodes. What content types should I use to categorize my show?   
:   A: Use `TVShow`, `TVSeason`, and `TVEpisode`.

Q: I have a TV show with episodes that are sequential, but that do not have seasons. What content type should I use?    
:   A: Use the `MiniSeries` and `MiniSeriesEpisode` types. You will need to specify values for `EpisodeInSeries` to represent the episode sequence.

Q: I have a TV show with sequential episodes, but that don't have a sequence number. Instead of sequence number, these episodes are ordered by their air-date. Can I still use `MiniSeries` as my type? If not, which content type should I use?   
:   A: Use the `TVShow` and `TVSpecial` types. In the `TVSpecial`, link it to the `TVShow` using the `ShowID` or `ShowTitle` fields, and include the mandatory air date.

Q: I have a news-type content where the episodes do not have seasons but do have an air date. What content type should I use?    
:   A: Use the `TVShow` and `TVSpecial` types. In the `TVSpecial`, link it to the `TVShow` using the `ShowID` or `ShowTitle` fields, and include the mandatory air date.

Q: I have a show with seasons, but I do not have the actual values for the seasons. What should I do?    
:   A: You are required to include season values for a show with seasons, which prevents a bad user experience. To obtain season information, try looking at  an authority catalog such as IMDb. Amazon usually recommends excluding season-based content that lacks season values, as opposed to trying to make the show fit into another content type. Omitting this content should help preserve a positive viewer experience for your users.

Q: We don't have season or episode information for our series. What should I do? Should I just make up fake values?    
:   A: Do not provide fake `Season` numbers and `Episode` numbers in your catalog. Instead, replace the `TvEpisode` entry type with `TvSpecial`. Continue to map the entries (`TvSpecials`) to the `TVShow`, so that they are associated with one another through the `TvShow`. However, note that in `TvSpecials`, `OriginalAirDate` is a required field. This allows us to better match (and use other sources to identify what the Season number and Episode number are) and to properly order the episodes in the `TvShow`.

Q: These all seem like an awful lot of rules to remember. How can I tell what to do without having to memorize every little nuance?    
:   A: Generally, go by what your customers would expect. For example, if you have a TV series that is a talk show that airs nightly, your customers probably will not tend to think of that series as having seasons. In this case, air date is more important. However, if you have a popular series that tends to be binge-watched by users, season and episode number will likely be much more important to that viewer base.

## Ingestion Reports

Q: What is an ingestion report?   
:   A: This report tells you whether your catalog was successfully integrated into Fire TV's universal browse and search, and if not, why. After you upload your latest catalog, a new copy of the report is added to your S3 bucket as _report.html_, normally within four hours. You can use this report to troubleshoot issues in your catalog file. It contains the ingestion (integration) success or failure status plus any errors, warnings, and suggestions. See [Receiving and Understanding the Fire TV Catalog Ingestion Report][receiving-and-understanding-the-catalog-ingestion-report].

Q: How do I know if my uploaded catalog file was integrated into the system?    
:   A: You can receive the ingestion results through an opt-in email, which also includes a link to download the full report.

Q: How do I get added to the email distribution list for my catalog's ingestion results?    
:   A: Send mail to [p11-catalog-subscriptions@amazon.com][13] asking to be added. Because the success and failure mail reports are separate email lists, ask to be added to both lists.

Q: My catalog ingestion failed. What do I do next?    
:   A: Look at your ingestion report for the specific errors that caused the failure. Explanations and follow-up instructions for each error are given in the ingestion report documentation. If necessary, your Amazon Business Development Manager can help you work through problems with your catalog. See [Catalog Data Format (CDF) Ingestion Report Messages][catalog-data-format-ingestion-report-messages].   



[3]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog-13.xsd
[10]: http://xmlsoft.org/xmllint.html
[13]: mailto:p11-catalog-subscriptions@amazon.com

{% include links.html %}
