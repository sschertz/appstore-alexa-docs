---
title: Beginning-to-End Process for Building an App
toc: false
permalink: fire-app-builder-end-to-end-process.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---
<style>
th {
background-color: #444;
color: white;
font-weight: bold;
}
</style>


The following steps show the beginning-to-end process for building an app with Fire App Builder. Assuming you already have the [requirements][fire-app-builder-overview#requirements], you can build your app by completing the topics in each of the following sections:

|1. Get Set Up |
|---|-----|
| a. [Download Fire App Builder and Build the Sample App][fire-app-builder-download-and-build] | Download the Fire App Builder code and build the "Application" in Fire App Builder. Then clone Fire App Builder to create your own app. |
| b. [Connect to Fire TV Through ADB][fire-app-builder-connecting-adb-to-fire-tv] | Connect your computer to your Fire TV device using ADB so you can test your app. |
| c. [Take an App Tour][fire-app-builder-app-tour] | Get more familiar with the basic features and screens in the sample app in Fire App Builder. |

| 2. Configure Your Feed |
|---|
| a. [Load Your Media Feed][fire-app-builder-load-media-feed] | Load your media feed in the app. Your feed contains all of your media assets, including the titles, descriptions, thumbnails, and media objects. |
| b. [Recipe Configuration Overview][fire-app-builder-set-up-recipes-overview] | Learn about what recipes are in Fire App Builder and requirements for configuration. |
| c. [Set Up the Category Recipe][fire-app-builder-set-up-recipes-categories] | Configure how Fire App Builder reads the categories in your feed. Categories organize your content into different groups.  |
| d. [Set Up the Contents Recipe][fire-app-builder-set-up-recipes-content] | Configure how Fire App Builder reads the content in your feed. Content refers to all the elements in your feed, such as the title, description, and video URLs. |
| e. [Navigator Configuration Overview][fire-app-builder-configure-navigator] | Learn about the role of the Navigator file and what needs configuration. |
| f. [Configure Navigator -- Token-based Feeds][fire-app-builder-configure-navigator-token-feeds] | Associate the categories and contents recipes with the screens in your app's UI. Follow these instructions if your feed requires a token to access it.|
| g. [Configure Navigator -- Open Feeds][fire-app-builder-configure-navigator-open-feeds] | Associate the categories and contents recipes with the screens in your app's UI. Follow these instructions if your feed is openly accessible without a token. |

| 3. Customize Your App's Appearance |
|---|
| a. [Customize the Look and Feel of Your App][fire-app-builder-customize-look-and-feel] | Customize the appearance of your app through the custom.xml file. You can customize almost every element of the app, from the font to background colors, homepage layout, splash screen, and more.|

| 4. Add Components |
|---|
| a. [Components Overview][fire-app-builder-interfaces-and-components] | Set up authentication, in-app purchasing, analytics, ads, or the media player by loading already coded components that implement interfaces in Fire App Builder. |

When you finish building your app and are ready to submit it to the Amazon Appstore, see [Publishing Android Apps to 
the Amazon Appstore][appstore-understanding-submission]. As you prepare your app for publishing, you will need to [take various screenshots](/support/submitting-your-app/tech-docs/04-taking-screenshots) and gather [Fire TV image assets][asset-guidelines-for-app-submission#firetvassets].

{% include links.html %}
