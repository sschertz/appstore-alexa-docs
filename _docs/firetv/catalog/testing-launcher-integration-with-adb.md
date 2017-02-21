---
title: Testing Launcher Integration with ADB
permalink: testing-launcher-integration-with-adb.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

After integrating your app with the Fire TV Home Screen Launcher (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]), you can use Android Debug Bridge (ADB) to test your app and ensure that it responds correctly to sign in and playback intents.

Use this testing option when you have not yet completed development on your app or have not yet implemented the code to respond to Intents from the launcher (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]). You will also need a Fire TV device to be able to test your app's launcher integration.

If you want to test launcher integration but haven't completed your app yet or at least haven't implemented the code to respond to Intents, use the Test App option. See [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app].

{% include note.html content="ADB is provided by the Android Open Source Project, not by Amazon. Any links to ADB reference materials re-direct you to the Android developer website." %}

* TOC
{:toc}

## Process Overview

Use the following high-level process to test your Fire TV launcher integration using ADB:

1.  Install your app to a Fire TV device using ADB.
2.  Test your sign-in requests for your app.
3.  Test playback requests for your app.

## Step 1: Install Your App to a Fire TV Device using ADB

Before you can test your app, you will need to install your app to a Fire TV device.

To install your app to a Fire TV Device:

1.  Use ADB to connect the Fire TV device to your development computer. (See [Connecting ADB to Your Fire TV Device][connecting-adb-to-fire-tv-device].)
2.  Sideload your app onto the device for testing. (See [Installing and Running Your App][installing-and-running-your-app]).

All of the `adb` commands on this page use the Android activity manager (`am`) to send Intents to your app. For more information on the syntax and options for `am`, see [Android Debug Bridge](http://developer.android.com/tools/help/adb.html) on the Android developer website.

## Step 2: Testing Sign In Requests

Sign-in requests from the Fire TV Launcher include both the request itself and a reference to the requested content, which should play immediately after the user signs in. The content reference may be a URI or a data extra name/value pair, depending on your implementation.

To test sign-in requests:

1.  On the computer that you have connected to the Fire TV device via ADB, open a terminal window so that you can access the command line.
2.  At the shell prompt (indicated by the "$"), type one of the following commands:
    *   If your content is specified by URI, enter:

        ```bash
        $ adb shell am start -a <signin_intent_action>  -n <signin_component> -f <signin_intent_flags> -d <content_uri>
        ```

    *   If your content is specified by a data extra, enter:

        ```bash
        $ adb shell am start -a <signin_intent_action>  -n <signin_component> -f <signin_intent_flags> -e <data_extra_name> <content_id>
        ```

### Command Options

The following table describes the available options for these two commands. Replace the text in the <angle brackets> with the correct values for your app:

| `-a <signin_intent_action>` | Your sign-in Intent action, from the `SIGNIN_INTENT_ACTION` extra. <br/><br/>Example: `com.yourcompany.player.SIGN_IN` |
|`-n <signin_component>`| The full component name of your sign-in Activity, which consists of the package and class names separated by a slash. These values are from the `SIGNIN_INTENT_PACKAGE` and `SIGNIN_INTENT_CLASS` extras, respectively. <br/><br/>Example: `com.yourcompany.player/com.yourcompany.player.MainActivity` |
| `-f <signin_intent_flags>` | The Intent flags that you specified in the `SIGNIN_INTENT_FLAGS` extraas a decimal value and combined with the appropriate default launcher flags. By default, the launcher sends the following flags: <br/><br/>&bull; `Intent.FLAG_ACTIVITY_NEW_TASK` <br/><br/>&bull; `Intent.FLAG_INCLUDE_STOPPED_PACKAGES` <br/><br/>&bull; `Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED` <br/><br/>Example decimal: 270532640 |
| `-d <content_uri>` | URI, if the link to your content is specified by URI. Do not use the `-e` option if you use the -d option. |
| `-e <data_extra_name>`<br/>`<content_id>` | If the link to your content is specified in a way other than a URI, this is the key/value pair for that content. The key is from the `DATA_EXTRA_NAME` extra. Do not use the `-d` option if you use the -e option. |

### Examples

The following example shows the command that you might type for a sign-in Intent if your content is specified by URI:

```
$ adb shell am start -a com.yourcompany.player.SIGN_IN  -n com.yourcompany.player/com.yourcompany.player.MainActivity -f 270532640 -d myplayer://content/12345`
```

The following command shows the command that you might type for a sign-in Intent if your content is specified in some way other than by URI:

```
$ adb shell am start -a com.yourcompany.player.SIGN_IN  -n com.yourcompany.player/com.yourcompany.player.MainActivity -f 270532640 -e content_id 12345`
```

## Step 3: Testing Playback Requests

As with sign-in requests, playback requests use the data you supply to the launcher during the capability exchange. Playback requests include a content reference, which may either be a URI or a data extra name/value pair, depending on your implementation.

To playback requests:

1.  On the computer that you have connected to the Fire TV device via ADB, open a terminal window so that you can access the command line.
2.  At the shell prompt (indicated by the "$"), type one of the following commands:
    *   If your content is specified by URI, enter:

        ```
        adb shell am start -a <playback_intent_action>  -n <playback_component> -f <playback_intent_flags> -d <content_uri>
        ```

    *   If your content is specified by a data extra key/value pair, enter:

        ```
        adb shell am start -a <playback_intent_action>  -n <playback_component> -f <playback_intent_flags> -e <data_extra_name> <content_id>`
        ```

### Command Options

The following table describes the available options for these two commands. Replace the text in the <angle brackets> with the correct values for your app:


| `<playback_intent_action>` | Your playback Intent action, from the `PLAY_INTENT_ACTION` extra. <br/> Example: `com.yourcompany.player.PLAY` |
| `<playback_component>` | The full component name of your playback Activity, which consists of package and class names separated by a slash. These values are from the `PLAY_INTENT_PACKAGE` and `PLAY_INTENT_CLASS` extras, respectively. <br/> Example: `com.yourcompany.player/com.yourcompany.player.MainActivity` |
| `<playback_intent_flags>` | The Intent flags that you specified in the `PLAYBACK_INTENT_FLAGS` extra as a decimal value and combined with the appropriate default launcher flags. By default, the launcher sends the following flags: <br/><br/>&bull; `Intent.FLAG_ACTIVITY_NEW_TASK` <br/><br/>&bull; `Intent.FLAG_INCLUDE_STOPPED_PACKAGES` <br/><br/>&bull; `Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED` <br/><br/> Example decimal: 270532640.|
| `<content_uri>` | URI, if the link to your content is specified by URI. Do not use the `-e` option if you use the -d option. |
| `<data_extra_name>`<br/>`<content_id>` | If the link to your content is specified in a way other than a URI, this is the key/value pair for that content. The key is from the `DATA_EXTRA_NAME` extra. Do not use the `-d` option if you use the -e option. |

### Examples

The following example shows the command that you might type for a playback Intent if your content is specified by URI:

```
$ adb shell am start -a com.yourcompany.player.PLAY  -n com.yourcompany.player/com.yourcompany.player.MainActivity -f 270532640 -d myplayer://content/12345
```

The following command shows the command that you might type for a playback Intent if your content is specified in some way other than by URI:

```
$ adb shell am start -a com.yourcompany.player.PLAY  -n com.yourcompany.player/com.yourcompany.player.MainActivity -f 270532640 -e content_id 12345`
```

{% include links.html %}
