---
title: Implementing Search in Fire TV
permalink: implementing-search-fire-tv.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

Implementing search in Fire TV requires you to understand some key differences with text search and voice search.

* TOC
{:toc}

## Text Search in Apps

Text search within an app refers to any specific text-search features you have specifically coded within your app. Text search for your app is not available by default on Fire TV.

## Global Text Search

Fire TV provides a global text search available from the Fire TV home screen. The global text search returns results from the Fire TV catalog. In order for your media to appear in global text search results, you must [integrate your app's media into the Fire TV Catalog][integrating-your-catalog-with-fire-tv].

## Voice Search {#voicesearch}

Fire TV also provides voice capabilities through the voice-enabled remote control. In addition to the [Alexa voice capabilities on Fire TV](https://www.amazon.com/gp/help/customer/display.html?nodeId=201859020), users can also use natural language to search for TV shows, movies, and other media.

Regardless of where users are in Fire TV, when they press the microphone button on a voice-enabled remote and say the TV show or Alexa actions they want, this action initiates a global search *using the Alexa cloud service instead of the Leanback library*.

Media requests through voice always return content from the Fire TV catalog. If you want your app's media to appear in these results, you must [integrate your app's media into the Fire TV Catalog][integrating-your-catalog-with-fire-tv].

If the user already has your app installed, your app's content can appear directly in the catalog results. If a user doesn't have your app, a "More Ways to Watch" option appears for users to get your app and view the content. (Note that catalog integration is only recommended as an option for apps whose content is the same for all customers.)

## Avoiding Speech Recognition Errors from Leanback

Because Alexa is used for voice interactivity on Fire TV instead of the Leanback library, you must disable any speech recognition with the Leanback library’s [`SearchFragment`]((http://developer.android.com/reference/android/support/v17/leanback/app/SearchFragment.html)) class for your Fire TV app. If you don't disable speech recognition, your app will potentially return errors when users perform searches.

In Leanback's `SearchFragment` class, the `startActivityForResult` method looks for the speech recognizer. Since FireTV does not support this speech recognizer, this action produces an error. To avoid the error, override the `onCreate()` method and comment out the speech recognition callback so that the method does not execute. Here's an example:

```java
setSpeechRecognitionCallback(() -> {
    if (DEBUG) Log.v(TAG, "recognizeSpeech");
    try {
        //startActivityForResult(getRecognizerIntent(), REQUEST_SPEECH);
    }
    catch (ActivityNotFoundException e) {
        Log.e(TAG, "Cannot find activity for speech recognizer", e);
    }
```

Here the `startActivityForResult` method is simply commented out, so the speech recognition feature will not execute and no error will result.

## Alexa Skills and Apps

You cannot create an Alexa-powered voice search specific to your app, returning media results from your app only. You can access any Alexa skill on Fire TV, but the skills are voice-only experiences. The voice skills do not interact with native apps on Fire TV. As such, there are no ways for an Alexa skill you create to control your app.

{% include links.html %}
