---
title: Navigator Configuration Overview
permalink: fire-app-builder-configure-navigator.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

The Navigator.json file (located in **app > assets**) defines what type of user interface elements are in the app and how they communicate with each other, what activities are associated with the apps, the layouts, and more.

After you have [set up your recipes][fire-app-builder-set-up-recipes-overview], you need to configure the Fire App Builder UI to use this information. To do this, you will configure the `globalRecipes` object in the Navigator.json file with the categories and contents recipes you customized earlier.

* TOC
{:toc}

## DataLoaderRecipe

The DataLoaderRecipe file loads a media feed URL. Because DataLoaderRecipe can load only one media feed URL at a time, if you have multiple media feed URLs, you will have to create multiple DataLoaderRecipe files, each time referencing the same category and contents recipes. This can be somewhat manual to set this up, but once set up, you won't need to touch it again.

## Two Kinds of Feeds

The approaches for configuring Navigator differ for token or non-token-based feeds. See one of the following:

* [Configure Navigator -- Token-based Feeds](fire-app-builder-configure-navigator-token-feeds): Use these instructions if you publish your media details in a media feed whose access is restricted by a token.
* [Configure Navigator -- Open Feeds](fire-app-builder-configure-navigator-open-feeds): Use these instructions if you publish your media details in a media feed that is open and unrestricted.

{% include links.html %}
