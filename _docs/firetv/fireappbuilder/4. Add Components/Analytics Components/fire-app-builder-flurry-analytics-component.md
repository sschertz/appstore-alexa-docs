---
title: Flurry Analytics Component
permalink: fire-app-builder-flurry-analytics-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

Flurry provides analytics for your Android app that shows details related to media playback. According to [Flurry Analytics](https://developer.yahoo.com/flurry/docs/analytics/):

>The Flurry Analytics SDK provides you with the tools and resources you need to gain a deep level of understanding about your users’ behavior in your apps. Set up advanced analysis of complex events, with metrics, segments and funnels to better track your users’ habits and performance. 

Follow the sections below to configure Flurry Analytics.

* TOC
{:toc}

## Step 1. Get a Flurry API Key {#step1}

1.  Create an account with [Flurry Analytics](https://developer.yahoo.com/analytics/) and get your API key.
2.  Copy your Flurry API key into a convenient place.

## Step 2. Configure the Flurry Analytics Component {#step2}

1.  Load the Flurry Analytics Component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component].
    
2.  Remove any other analytics components that are loaded in your app (such as Crashlytics, OmnitureAnalyticsComponent, or LoggerAnalyticsComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.
    
    {% include_relative componentnote_analytics.html %}
    
2.  Go to **FlurryAnalyticsComponent > res > values > custom.xml** and copy the following strings:

    ```xml
    <string name="encrypted_flurry_api_key">YOUR_ENCRYPTED_FLURRY_API_KEY</string>

    <string name="flurry_key_1">flurry_random_key_1</string>
    <string name="flurry_key_2">flurry_random_key_2</string>
    <string name="flurry_key_3">flurry_random_key_3</string>
    <string name="flurry_key_4">flurry_random_key_4</string>
    <string name="flurry_key_5">flurry_random_key_5</string>
    <string name="flurry_key_6">flurry_random_key_6</string>
    ```
    
3.  Paste these strings in your app's **custom.xml** file (located inside res > values).  

     {% include tip.html content="As a best practice to integrate future updates, always copy values from the component's XML files into your app's custom.xml file. Your app's XML values will overwrite any values in your component's XML files." %}
          
     The `encrypted_flurry_api_key` string will hold the encrypted version of your Flurry API key. The `flurry_key_[#]` strings are used to create the encryption.

## Step 3. Encrypt your Flurry API Key {#step3}

You must encrypt your Flurry API key.

1.  Open your app's **custom.xml file** (inside res > values).
2.  Type a random alphanumeric string for each of the `flurry_key_[#]` values. For example:
    
    ```xml
    <string name="flurry_key_1">flurryblurry8837</string>
    <string name="flurry_key_2">furryBEAR2999</string>
    <string name="flurry_key_3">homer2YHfurrybaLL28_sneezun</string>
    <string name="flurry_key_4">curry88fl@vrczng</string>
    <string name="flurry_key_5">someFlurryBlurry9911</string>
    <string name="flurry_key_6">yrrulFbackwardZZ44</string>
    ```
    
4.  In the Android View, expand the **Utils > java > com > amazon > utils > security** folder and open the **ResourceObfuscationStandaloneUtility** class.
5.  In the `getRandomStringsForKey()` method, enter the values you used for `flurry_key_1`, `flurry_key_2`, and `flurry_key_3` respectively. 
    
    For example, assuming these first 3 keys are the ones displayed in the previous code sample, you would enter the following:
        
    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "flurryblurry8837",
                "furryBEAR2999",
                "homer2YHfurrybaLL28_sneezun"
        };
    }
    ```
    
    In this example, the values are as follows:
     
    *  `flurryblurry8837` is the value used for `flurry_key_1`. 
    *  `furryBEAR2999` is the value used for `flurry_key_2`. 
    *  `homer2YHfurrybaLL28_sneezun` is the value used for `flurry_key_3`.
    
6.  In the `getRandomStringsForIv()` method, enter the values you used for `flurry_key_4`, `random_key_5`, `random_key_6` respectively. For example:
    
    ```java
        private static String[] getRandomStringsForIv() {
    
            return new String[]{
                    "someFlurryBlurry9911",
                    "calypsoIslandShipBLLd99",
                    "yrrulFbackwardZZ44"
            };
        }
    }
    ```
    
    In this example, the values are as follows:
     
    *  `someFlurryBlurry9911` is the value used for `flurry_key_4`. 
    *  `someFlurryBlurry9911` is the value used for `flurry_key_5`.
    *  `yrrulFbackwardZZ44` is the value used for `flurry_key_6`.
    
7.  In the `getPlainTextToEncrypt()` method, insert your Flurry API key in place of `Encrypt_this_text`:
    
    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```
    
8.  Right-click the **ResourceObfuscationStandaloneUtility.java** file and select **Run 'ResourceObfusc...main()**.
9.  Look for the encrypted result printed to the console. It will look something like this:

    ```
    Encrypted version of plain text 123456789 is Hgei944983ljdfHoaQ==
    ```
    
10. Copy your encrypted Flurry API key and paste it into the `encrypted_flurry_api_key` string's value in your app's **custom.xml** file. For example:
                                                                                        
    ```xml
    <string name="encrypted_flurry_api_key">Hgei944983ljdfHoaQ==</string>
    ```
    
    {% include tip.html content="Save your random keys in a safe place, such as your company wiki, so that you always have an easy way to retrieve them. You will need these keys to decrypt the value." %}

Flurry Analytics is now integrated into your app and will start sending in information about events.

## Checking Flurry in Your Logs

When you build your app, view logcat with a filter on the word "flurry" to see how the Flurry Analytics Component gets triggered with events.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_flurrytracking" type="png" %}

Although it takes several hours before the activity data populates in Flurry Analytic's dashboards, you can see event logs in a matter of minutes. In Flurry Analytics, go to **Events > Event Logs**.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_flurryeventlog" type="png" %}

{% include links.html %}
