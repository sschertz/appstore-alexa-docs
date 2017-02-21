---
title: Integrating Your App with the Fire TV Home Screen Launcher
permalink: launcher-integration.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

Modifying your app to be used with Amazon Fire TV's home screen launcher integrates your media content and your users' subscription statuses with other content in the Amazon Fire TV user interface. This process is also known as "deep linking" Amazon Fire TV's content to your content. This deep linking is the other major piece (in addition to [uploading your catalog][uploading-your-catalog]) in integrating your app's catalog with Fire TV.

This page describes the required app changes to integrate your catalog with the Fire TV home screen launcher. After you have modified your app, test your integration before re-publishing your app. See [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb] and [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app] for information.

* TOC
{:toc}

## Process Overview

There are five steps to integrate your app with the home screen launcher:

1.  Broadcast an Android Intent that contains the user's subscription status and other information about your app's capabilities.
2.  Receive capability requests from the launcher and respond with the same information in Step 1.
3.  Receive playback or sign in requests from the launcher.
4.  Add error-handling code to deal with sync issues between your catalog and available content.
5.  Configure your Android manifest to handle all of these changes.
6.  Validate your launcher integration.
7.  Re-publish your updated app to make your changes available to your user base.

Your app is still responsible for:

*   Playback and sign-in Activities and behavior.
*   Management of users' subscription status with your service.

## Step 1: Broadcast An Intent From Your App Containing Its Capabilities

To play your content, the Fire TV home screen launcher needs user information from your app to be able to later play your content or to request that the user sign in to your service. Your app must send this information as an Android broadcast Intent at these times:

*   Each time your app starts up.
*   When the launcher requests it (see Step 2).
*   When your user's subscription status changes, for example, if they sign into or out of your service.

### Types of Intents: Playback and Sign-in Intents

For Fire TV Launcher integration and deep-linking, create two different types of Android Intents, which your app will need to broadcast:

*   **Sign-In Intents**: Broadcast a sign-in Intent if the current user is not signed in to your app.
*   **Playback Intents**: Broadcast a playback Intent if the current user is already signed in to your app, such as when a user has just signed in to or activates your app.

Use these two Intent types to communicate your app's capabilities to Amazon Fire TV.

### Building and Sending an Intent

To broadcast your app's capabilities, build a standard Android Intent, and send it to the app's context with the [`sendBroadcast()`][1] method. When you construct your Intent:

*   Name the Intent package `com.amazon.tv.launcher`.
*   Set the Intent action to `com.amazon.device.CAPABILITIES`.
*   Specify all other Intent information as Intent extras.

The following example shows a sample `BroadcastCapabilities()` method that broadcasts different Intent extras based on a Boolean `userIsSignedIn` variable. All values in this example are sample data and must be replaced with your own values.

```java
public void BroadcastCapabilities(Context context)
{
    Intent intent = new Intent();
    intent.setPackage("com.amazon.tv.launcher");
    intent.setAction("com.amazon.device.CAPABILITIES");
    if (userIsSignedIn) {
        intent.putExtra("amazon.intent.extra.PLAY_INTENT_ACTION","android.intent.action.VIEW");
        intent.putExtra("amazon.intent.extra.PLAY_INTENT_PACKAGE","com.contentcompany.player");
        intent.putExtra("amazon.intent.extra.PLAY_INTENT_CLASS","com.contentcompany.player.PlayActivity");
        intent.putExtra("amazon.intent.extra.PLAY_INTENT_FLAGS", Intent.FLAG_ACTIVITY_NEW_TASK | Intent.MORE_FLAGS);
    } else {
        intent.putExtra("amazon.intent.extra.SIGNIN_INTENT_ACTION","android.intent.action.VIEW");
        intent.putExtra("amazon.intent.extra.SIGNIN_INTENT_PACKAGE","com.contentcompany.player");
        intent.putExtra("amazon.intent.extra.SIGNIN_INTENT_CLASS","com.contentcompany.player.SignInActivity");
        intent.putExtra("amazon.intent.extra.SIGNIN_INTENT_FLAGS", Intent.FLAG_ACTIVITY_NEW_TASK | Intent.MORE_FLAGS);
    }
    intent.putExtra("amazon.intent.extra.PARTNER_ID","contentcompany");

    //Send the intent to the Launcher
    context.sendBroadcast(intent);
}
```

### Intent Extras

Specify the capabilities of your app in the keys and values of the extras field, using the [`Intent.putExtra()`][2] method. All of the Intent extras have a prefix package of `amazon.intent.extra`.

The following table shows the all required Intent extras and the specific extras for playback and sign-in. All of the Intent extra values are prefixed with `amazon.intent.extra`.

| `PARTNER_ID` | Always | Your Partner ID (supplied by Amazon) is the same ID that you use for catalog integration in the Partner field of your CDF file. Note that this ID is unique to your app, not to an individual or organization; if you or your organization has multiple Fire TV apps, each app will all have different Partner IDs.|
| `DATA_EXTRA_NAME` | Conditional | The name of the extra containing the content ID for your data, for example, titleData. Specify this value only if your content ID is not described by a URI. If your content ID is not in URI format (for example, `123456`), use `DATA_EXTRA_NAME` to indicate the name of the Intent extra the launcher should use for that ID. See Step 3 for more information. Access that content ID with the [`Intent.getStringExtra()`][3] method. |
| `PLAY_INTENT_ACTION` | If user is signed in | For playback Intents, the Intent action the launcher sends, usually `android.intent.action.VIEW`. |
| `PLAY_INTENT_PACKAGE` | If user is signed in | For playback Intents, the package for your app, for example `com.contentcompany.player`. |
| `PLAY_INTENT_CLASS` | If user is signed in | For playback Intents, the full package and class name of your app, for example, `com.contentcompany.player.PlayActivity`. |
| `PLAY_INTENT_FLAGS` | If user is signed in | For playback Intents, any flags your app needs from the Intent, as an integer. See the Android reference guide for [Intent](http://developer.android.com/reference/android/content/Intent.html) for possible flag values. |
| `SIGNIN_INTENT_ACTION` | If user is signed out | For sign in Intents, the Intent action the launcher sends, usually `android.intent.action.VIEW`. |
| `SIGNIN_INTENT_PACKAGE` | If user is signed out | For sign in Intents, the package for your app, for example `com.contentcompany.player`. |
| `SIGNIN_INTENT_CLASS` | If user is signed out | For sign in Intents, the class name of your app, for example, `SignInActivity`. |
| `SIGNIN_INTENT_FLAGS` | If user is signed out | For sign in Intents, any flags your app needs from the Intent, as an integer. See the Android reference guide for [Intent][4] for possible flag values. |

Two extras are always required:

*   Your Amazon-provided partner ID (`PARTNER_ID`). This is the same ID you use for catalog integration.
*   The display name of your app (`DISPLAY_NAME`). The display name is used by the launcher to label the playback or sign in button on a given title.

The remainder of the Intent extras vary based on whether or not the user is authorized to play your content (they are signed in), or if they need to sign in before proceeding.

*   If the user is signed in, extras with the `PLAY_` prefix are required.
*   If the user is not signed in, extras with the `SIGNIN_` prefix are required.

### Examples

For example, when a user signs in or activates your app, send an Intent with the following extras:

*   `PARTNER_ID`
*   `DISPLAY_NAME`
*   `PLAY_INTENT_ACTION`
*   `PLAY_INTENT_PACKAGE`
*   `PLAY_INTENT_CLASS`
*   `PLAY_INTENT_FLAGS`

In another example, if the launcher requests your app's capabilities, and the user is not signed in, send an Intent with the following extras:

*   `PARTNER_ID`
*   `DISPLAY_NAME`
*   `SIGNIN_INTENT_ACTION`
*   `SIGNIN_INTENT_PACKAGE`
*   `SIGNIN_INTENT_CLASS`
*   `SIGNIN_INTENT_FLAGS`

Do not provide both sets of Intent extras at once; the launcher assumes that the existence of `PLAY_` keys means that the user is authorized to view your content.

If your app has multiple subscription levels (for example, free, basic, premium), you can provide multiple sets of broadcast Intents for each level with different values of `PARTNER_ID` and `DISPLAY_NAME`.

## Step 2: Receive Capability Requests from the Launcher

Amazon Fire TV's home screen launcher occasionally requests your app's capabilities and the subscription status of the user. The launcher requests this information with an Android broadcast Intent, whose action is `com.amazon.device.REQUEST_CAPABILITIES`. Your app must respond to this request with the same Intent that you previously created in Step 1.

{% include note.html content="Do not wait for the launcher to make a request. As described in Step 1, you must send your app's capabilities information each time your app starts up and if the user's subscription status changes." %}

The following example shows a simple class to handle the launcher's broadcast Intent. The capability information you send to the launcher is the same information you send in Step 1, so you can use the same method (here, `broadcastCapabilities())` to manage both those tasks.

```java
public class CapabilityRequestReceiver extends BroadcastReceiver
{
   @Override public void onReceive(Context context, Intent intent)
   {
      broadcastCapabilities(); //the method you use to broadcast your app's information to the launcher
   }
}
```

## Step 3: Handling Playback and Sign In Intents from the Launcher

When the user views the details for your content in the Fire TV user interface, Fire TV shows different options based on the user's status with your service:

*   If the user is logged into your service (you provided `PLAY_` intent extras in the capabilities broadcast), a button labeled with the display name of your app appears (for example, "MyCompany Player"). If the user chooses that button, the launcher sends a playback intent to your app.
*   If the user is not logged in (you provided `SIGNIN_` intent extras) a button labeled "Launch" with the display name of your app appears ("Launch MyCompany Player"). If the user chooses that button, the launcher sends a sign in intent to your app.

In both cases, the launcher uses the information you provided in the capabilities broadcast to construct both playback and sign in intents it sends your app.

To handle either playback or signin intents, implement an activity in your app to receive and handle those intents and to play the requested content.

{% include note.html content="In handling these intents, your app **must** begin playback immediately (or immediately after the user signs in). Do not display an additional details page or other type of landing page in your app."%}

This code shows the skeleton of a playback activity:

```java
public class PlayActivity extends Activity {
   @Override
   public void onCreate(Bundle bundle) {
      super.onCreate(bundle);
      Uri data = getIntent().getData();
      //Play the content specified by 'data'
      // OR
      String data = getIntent.getStringExtra("titleExtra");
      //Play the content specified by the extra you indicated in DATA_EXTRA_NAME
   }
}
```

Both the playback and signin intents include the ID of the requested content to enable your app to play that content immediately (playback intents) or after the user signs in (sign in). You specify the IDs and the ID format in your media catalog as part of [catalog integration][integrating-your-catalog-with-fire-tv].

If your content IDs are in URI format (for example, `myapp://Media/123456`), the launcher sends that URI as part of the intent data (`Intent.getData()`). If your content ID is not in URI format (for example, `123456`), the launcher sends that ID in the extra you specified in `DATA_EXTRA_NAME` as part of the capabilities broadcast. You can access that content ID with the [`Intent.getStringExtra()`][5] method.

{% include note.html content="The launcher only sends the data that you specify in your CDF catalog, and we do not require that ID to be in any specific format. Make sure that the IDs you specify in your catalog are in the same format that your app expects for playback, or in a format your app can use." %}

If users press the Back button consecutively (usually 3 times if starting from a media playback scenario), the app should ultimately return users to the last location where the users entered the application. In this case, the Search Results in the Launcher.

## Step 4: Add Error-handling Code to Deal with Sync Issues Between Your Catalog and Available Content

One common issue that many Fire TV-integrated apps face is the occasional issue of their catalogs becoming out-of-sync with the actual content that is available. If a user selects a content item via search or browse, and that items is not actually available to play, the user will be taken to a black screen with no clear way to navigate out. This experience can be very frustrating for users and reflect negatively on accessing your app through Fire TV.

This syncing issue can happen for several reasons:

*   You just uploaded a new catalog and it was successfully ingested in the window of a few hours before the actual content was available to play.
*   You rotated out some of your content, and an updated catalog reflecting these changes has not been uploaded/ingested yet.

To prevent users from encountering the black screen after selecting an unavailable piece of content, Amazon recommends implementing error-handling code in your app. This code should address the use case where a user tries to select unavailable content, provides a meaningful error message to the user, and re-directs them to an appropriate location. This type of graceful failure should lead to a more positive user experience for your customers.

## Step 5: Configure the Android Manifest

Configure your Android manifest to handle broadcast Intents from the launcher. Specifically, you'll need to add three things:

*   Add Intent filters to handle playback and sign-in Intents.
*   Add a receiver for the launcher's capabilities to request Intent.
*   Add a permission to enable your Intents to be sent with the right permissions.

{% include note.html content="Declaring the broadcast in your manifest is mandatory for Fire TV to detect that the app has catalog integration enabled." %}

### Add Intent Filters for Playback and Sign in

Add Intent filters for your playback and sign in activities to receive Intents from the launcher. This example uses a playback activity named `PlayActivity`:

```java
<activity
    android:name=".PlayActivity"
    android:label="Playback Activity">
    ...
    <intent-filter>
        <action android:name="com.contentcompany.player.PlayActivity">
        <category android:name="android.intent.category.DEFAULT">
    </intent-filter>
</activity>
```

### Add a Receiver for Launcher Requests

Add a `receiver` element to the manifest with the name of your `BroadcastReceiver` class, and an `intent-filter` for the action `com.amazon.device.REQUEST_CAPABILITIES`. In this example the receiver class is `CapabilityRequestReceiver`:

```java
<receiver android:name="CapabilityRequestReceiver" >
    <intent-filter>
    <action android:name="com.amazon.device.REQUEST_CAPABILITIES" />
    </intent-filter>
</receiver>
```

### Add Permissions

Add a `uses-permission` element to your manifest to ensure that the launcher accepts your intents:

```java
<uses-permission android:name="com.amazon.device.permission.COMRADE_CAPABILITIES" />
```

## Step 6: Test Your Launcher Integration

Once you have integrated your app with Fire TV's Home Screen Launcher, you will need to validate your launcher integration before submitting your app to the Amazon Appstore:

See [Test Cases for Verifying Fire TV Deep Links][test-cases-for-verifying-deep-links-from-your-fire-tv-catalog] for a flow of test cases to run through before submitting your app.

[1]: http://developer.android.com/reference/android/content/Context.html#sendBroadcast%28android.content.Intent%29
[2]: http://developer.android.com/reference/android/content/Intent.html#putExtra%28java.lang.String,%20java.lang.String%29
[3]: http://developer.android.com/reference/android/content/Intent.html#getStringExtra%28java.lang.String%29
[4]: http://developer.android.com/reference/android/content/Intent.html
[5]: http://developer.android.com/reference/android/content/Intent.html#getStringExtra%28java.lang.String%29

{% include links.html %}
