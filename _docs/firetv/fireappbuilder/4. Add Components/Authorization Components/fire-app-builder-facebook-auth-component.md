---
title: Facebook Authorization Component
permalink: fire-app-builder-facebook-auth-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

With the Facebook Auth Component, you can prompt users to log in using Facebook before they watch media (or perform some other action).

* TOC
{:toc}

## The User Experience with Facebook Authorization

If you require users to log in before playing media, users will see the following prompt after clicking "Watch Now" on the Content Details screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_loginprompt" type="png" caption="Login prompt when verifyScreenAccess is set to true for the PlaybackActivity" %}

If a user clicks **Later**, he or is able to watch media without logging into Facebook. (Currently, there isn't a later prompt that asks the user again.)

If a user clicks **Now**, he or she is prompted to log into Facebook via a computer browser:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbprompt" type="png" caption="Prompt to log into Facebook." %}

After logging in, Facebook shows the following screenshot:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbloginsuccessbrowser" type="png" caption="Facebook login success on the browser" %}

After clicking **Continue** on the browser window, the user clicks **Submit** on the Facebook Login prompt on the Fire TV screen.

## Configuration Overview

Follow these steps to configure the Facebook Authorization Component

* [Step 1. Get a Facebook App ID and Client Token](#getappidtoken)
* [Step 2. Configure Your App](#configurefireappbuilderfb)
* [Step 3. Encrypt the Facebook App ID and Token Values](#step3)
* [Step 4. Decide When to Prompt Users to Log In](#step4)
* [Step 5. Customize the UI Text](#step5)

## Step 1. Get a Facebook App ID and Client Token {#getappidtoken}

First you must set up a Facebook app to get the app ID and client token. Note that the app name you choose will be visible when users log in via their computer browsers, so choose a name (usually your Fire TV app's name) that you want users to see.

Note that these instructions were written in June 2016. Facebook may have changed some of the button names, steps, or workflow since that time.

To create a Facebook app:

1.  Go to [Register and Configure an App](https://developers.facebook.com/docs/apps/register) in the Facebook for Developers site and create a Facebook app. Be sure to follow steps 1, 2, and 3.
2.  After you click **Create new Facebook App** in step 3, select **Android**.
3.  In the "Quick Start for Android" screen, type a name for your app (use the same name as your Fire TV app). Then click **Create New Facebook App ID**.
4.  Complete the required fields to create a Facebook app. In the "Tell us about your Android project" section, note that the "Package Name" and "Default Activity Class Name" are required but won't actually be used since these fields relate to Google Play.
4.  When you click **Next**, you're prompted with a "Google Play Package Name" dialog box indicating that it can't find your package name in Google Play. Click **Use this package name** to ignore the message.
5.  An additional section appears: "Add your development and release key hashes." Follow the steps in this section to generate a key hash and add it in the **Key Hashes** field. Then click **Next**.
6.  You've finished the Quick Start for Android. In the upper-right corner of the screen, select **My Apps** and select your new app.
7.  In the left sidebar, under **PRODUCTS**, click **+ Add Product**.
8.  Next to **Facebook Login**, click **Get Started**.
9.  In the **Client OAuth Settings** section, turn on **Login from Devices**.
    
    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fbloginsetting" type="png" caption="Client OAuth settings for your Facebook app." %}
    
10. Click **Save Changes**.
11. In the left sidebar, click **Settings > Basic**. Copy the **App ID** into a convenient place.
11. In the left sidebar, click **Settings > Advanced**. Copy the **Client Token** into a convenient place.

## Step 2. Configure Your App {#configurefireappbuilderfb}

The Facebook Auth Component should already be loaded in the sample app in Fire App Builder, but you will need to configure the component.

1.  The Facebook Auth Component is already loaded in the sample app built with Fire App Builder. If it isn't loaded due to updates you've made, see [Load a Component in Your App][fire-app-builder-load-a-component]. 
2.  Remove any other authentication components that are loaded in your app (such as AdobepassAuthComponent or LoginWithAmazonComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.
    
    {% include_relative componentnote_authentication.html %}

## Step 3. Encrypt the Facebook App ID and Token Values {#step3}

You must insert an encrypted version of the Facebook app ID and client token into your app. To encrypt your app ID and client token:

1.  Expand the **FacebookAuthComponent > res > values** folder and open the **strings.xml** file.
2.  Copy the following strings and paste them into your app's **custom.xml** file:
    
    ```xml
    <string name="encrypted_fb_client_token">YOUR_ENCRYPTED_FB_APP_CLIENT_TOKEN</string>
    <string name="encrypted_fb_app_id">YOUR_ENCRYPTED_FB_APP_ID</string>
    
    <string name="fb_key_1">fb_random_key_1</string>
    <string name="fb_key_2">fb_random_key_2</string>
    <string name="fb_key_3">fb_random_key_3</string>
    <string name="fb_key_4">fb_random_key_4</string>
    <string name="fb_key_5">fb_random_key_5</string>
    <string name="fb_key_6">fb_random_key_6</string>
    ```
     
     {% include tip.html content="As a best practice to integrate future updates, always copy values from the component's XML files into your app's custom.xml file. Your app's XML values will overwrite any values in your component's XML files." %}
      
     The `encrypted_authentication_client_token` is the encrypted version of the client token you generated when you created your Facebook app. The `encrypted_authentication_app_id` is the encrypted version of the Facebook app ID. You'll encrypt these values in the upcoming steps. The random keys are used to create the encryption.
    
3.  Type a random alphanumeric string for each of these `fb_key_[#]` values. For example:
    
    ```xml
    <string name="fb_key_1">odysseusgrEEk2000bc</string>
    <string name="fb_key_2">helengreekFAce5000ships</string>
    <string name="fb_key_3">homer@1storyTllr20</string>
    <string name="fb_key_4">latinusOdysseusson332</string>
    <string name="fb_key_5">calypsoIslandShipBLLd99</string>
    <string name="fb_key_6">athenazeusEPICodysseY77</string>
    ```
    
4.  In the Android View, expand the **Utils > java > com > amazon > utils > security** folder and open the **ResourceObfuscationStandaloneUtility** class.
5.  In the `getRandomStringsForKey()` method, enter the values you used for `fb_key_1`, `fb_key_2`, and `fb_key_3` respectively. 
    
    For example, assuming these first 3 keys are the ones displayed in the previous code sample, you would enter the following:
        
    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "odysseusgrEEk2000bc",
                "helengreekFAce5000ships",
                "homer@1storyTllr20"
        };
    }
    ```
    
    In this example, the values are as follows:
     
    *  `odysseusgrEEk2000bc` is the value used for `fb_key_1`. 
    *  `helengreekFAce5000ships` is the value used for `fb_key_2`. 
    *  `homer@1storyTllr20` is the value used for `fb_key_3`.
    
6.  In the `getRandomStringsForIv()` method, enter the values you used for `fb_key_4`, `random_key_5`, and `random_key_6` respectively. For example:
    
    ```java
        private static String[] getRandomStringsForIv() {
    
            return new String[]{
                    "latinusOdysseusson332",
                    "calypsoIslandShipBLLd99",
                    "athenazeusEPICodysseY77"
            };
        }
    }
    ```
    
    In this example, the values are as follows:
     
    *  `latinusOdysseusson332` is the value used for `fb_key_4`. 
    *  `calypsoIslandShipBLLd99` is the value used for `fb_key_5`. 
    *  `athenazeusEPICodysseY77` is the value for `fb_key_6`.
    
7.  In the `getPlainTextToEncrypt()` method, insert your Facebook client token in place of `Encrypt_this_text`:
    
    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```
    
8.  Right-click the **ResourceObfuscationStandaloneUtility.java** file and select **Run 'ResourceObfusc...main()**.
    
 
9.  Look for the encrypted result printed to the console. It will look something like this:

    ```
    Encrypted version of plain text 123456789 is mTWxLhZeHslQFwpN3irjfQ==
    ```
    
10. Copy your encrypted app ID and paste it into the `encrypted_fb_client_token` string's value in your app's **custom.xml** file. For example:
                                                                                        
    ```xml
    <string name="encrypted_fb_client_token">rneiu89EIxnk9489faoPoaQ</string>
    <string name="encrypted_fb_app_id">YOUR_ENCRYPTED_FB_APP_ID</string>
    ```
    
11. Now insert your Facebook app ID into the `getPlainTextToEncrypt()` method, and run the script again (using the same random strings). Copy the encrypted key into the `encrypted_fb_app_id` string value in your app's **custom.xml** file. For example:
    
    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">AQ/9Qtc26GzLVSHRe1ftPw==</string>
    ```
    
    {% include tip.html content="Save your random keys in a safe place, such as your company wiki, so that you always have an easy way to retrieve them. You will need these keys to decrypt the value." %}
      
## Step 4. Decide When to Prompt Users to Log In{#step4}

You can configure the screen where users should be prompted to log in to Facebook.

1.  Open the **Navigator.json** file (located in your app's **assets** folder).
2.  Set the `verifyScreenAccess` value to `true` for the screen where you want users to log in. For example, if you want users to log in before playing media, you would verify screen access at the `PlaybackActivity`:

    ```xml
      "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": true,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
    ```
    
    Now when users launch your app and try to watch media, they will be prompted to log in.

## Step 5. Customize the UI Text{#step5}

To change the text that appears in the dialog box prompting users to log in:

1.  Go to **ContentBrowser > res > values** and open the **strings.xml** file. 
2.  Copy the following strings into your app's **custom.xml** file (inside res > values):

    ```xml
    <string name="optional_login_dialog_title">Login</string>
    <string name="optional_login_dialog_message">Do you want to get most of your app by logging in now?</string>
    <string name="now">Now</string>
    <string name="later">Later</string>
    ```
3.  Customize the string values.
    
## Disable User's Ability to Postpone Login
    
By default, users have the option of postponing the Facebook login. Currently if users click **Later** at this prompt, they aren't prompted ever again -- this is a bug. To disable the user's ability to postpone the Facebook login:

1.  Expand the **FacebookAuthComponent > res values** folder and open the **custom.xml** file. 
2.  Copy the following strings into your app's **custom.xml** file (inside res > values):
    
    ```xml
    <bool name="is_authentication_can_be_done_later">true</bool>
    ```
    
4.  Change the string's value to `false`. 

## Looking at the Logs for Facebook Auth Component Actions

After a user has already logged in, when he or she restarts the Fire TV app, the Facebook Auth Component will automatically check to see if the user is already logged in. Filtering the logcat to show "facebook" only, you will see the following if a user is not logged in:

```
06-24 17:39:18.602 29089-29089/com.amazon.android.calypso D/FacebookAuthenticationModuleInitReceiver: IAuthenticationModule initialized.
06-24 17:39:18.684 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Facebook configured and previous access token is:
06-24 17:39:43.566 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Checking if user is logged in
06-24 17:39:43.569 29089-29089/com.amazon.android.calypso D/FacebookAuthentication: Access token is null. User not logged in.
06-24 17:39:43.612 29089-29089/com.amazon.android.calypso D/Navigator: FacebookAuthenticationActivity onActivityCreated
```

In this case, the user is not logged in, so the FacebookAuthenticationActivity gets created.

If a user is logged in, the logcat will show something like the following:

```
06-24 17:46:27.933 29089-29148/com.amazon.android.calypso D/FacebookApi: Making http call to Facebook url: https://graph.facebook.com/v2.6/device/login_status
06-24 17:46:28.235 29089-29148/com.amazon.android.calypso D/FacebookApi: Response from HTTP call: {"access_token":"AAOA4zsiSjMBAJa42JB2LTTyIjq3hQTAl9RTq5FVA8QxKQFhhBlGlqGXpsqQYX9Puo5ZAZBW2eUgoyYquifpTaZAKS9SJhvJefv0hUMjGqAmjuOVNOxNjZCxQQmA23dc4Xcqhs8goZBIuYmbYKuJnltAopk5dQF4ZD","expires_in":5183946}
06-24 17:46:28.235 29089-29089/com.amazon.android.calypso D/com.amazon.android.auth.facebook.FacebookAuthentication: Storing access token: AAOA4zsiSjMBAJa42JB2LTTyIjq3hQTAl9RTq5FVA8QxKQFhhBlGlqGXpsqQYX9Puo5ZAZBW2eUgoyYquifpTaZAKS9SJhvJefv0hUMjGqAmjuOVNOxNjZCxQQmA23dc4Xcqhs8goZBIuYmbYKuJnltAopk5dQF4ZD
06-24 17:46:28.257 29089-29089/com.amazon.android.calypso D/Navigator: FacebookAuthenticationActivity onActivityPaused
06-24 17:46:28.257 29089-29089/com.amazon.android.calypso D/AnalyticsManager: FacebookAuthenticationActivity onActivityPaused, analytics tracking.
```

Here the access token is retrieved because the session is still active. As a result, the user is automatically logged in.

If you want to change the way the events are reported, you can manually customize the string values in **FacebookAuthComponent > java > com.amazon.android.auth.facebook > FacebookApi.java**. For example, if you do not want to use the term `name` in your analytics, you can customize this to another value:

```
public static final String NAME = "name";
```

To clear any login settings, scroll down to the bottom of your Fire TV app and click the **Logout** button.

{% include links.html %}
