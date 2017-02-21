---
title: Amazon In-App Purchasing Component
permalink: fire-app-builder-amazon-in-app-purchase-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

The Amazon In-App Purchasing Component uses Amazon's [In-App Purchasing](https://developer.amazon.com/public/apis/earn/in-app-purchasing) (IAP) API to integrate two different purchasing options in your app:

* **Daily Pass**: Allows uses to watch any media in your app for a day (starting from the time of purchase for 24 hours). This is similar to an unlimited movie rental for 24 hours.
* **Go Premium**: Allows users to pay a subscription fee to watch any media in your app during the subscription time period. You define the subscription time period when you set up the in-app items in the Amazon Developer Console.

With the In-App Purchasing Component, you don't have to worry about handling transactions with users. Users make purchases through their Amazon account, which they set up when they configure their Fire TV. The In-App Purchasing API takes care of all of the transaction details within your app.

{% include note.html content="You can customize the button text that says \"Daily Pass\" or \"Go Premium.\" More details about how to customize the UI strings are provided in [\"Customize the Button Text.\"](#buttontext)" %}

* TOC
{:toc}

## The User Experience with Amazon In-App Purchasing Component

After you implement the Amazon In-App Purchase Component in your app, users see a "Daily Pass" and "Go Premium" button on the Content Details screen (instead of a Watch Now button).  

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iapprompt" type="png" caption="<b>Figure 1.</b> Options to either subscribe or purchase a daily pass appear to the user on the Content Details screen." %}

When a user clicks the "Go Premium" or "Daily Pass" button, the user is prompted to make a purchase using his or her Fire TV account. For example, if the user clicks "Go Premium," the following screen appears:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iapsubscriptionoptions" max-width="650px" type="png" caption="<b>Figure 2.</b> Subscription options dialog box that appears to users." %}

If a user clicks "Daily Pass," the following screen appears:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions"  max-width="650px" type="png" caption="<b>Figure 3.</b> Rental options dialog box that appears to users." %}

{% include note.html content="When you configure the in-app purchasing items, you can customize the text that displays in these dialog boxes." %}

The user completes the transaction through this Fire TV dialog box using his or her Amazon account, which the user entered when setting up Fire TV. When the transaction is finished, the user sees this screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iappromptcomplete" type="png" caption="<b>Figure 4.</b> The order is completed." %}

The user can now start watching the content.

## Notes and Limitations with In-App Purchasing

Note the following details with the Amazon In-App Purchase component:

*  You must offer both Daily Pass (24-hour access) and Go Premium (subscription) options for your media.
*  When users purchase Daily Pass or Go Premium, they get access to *all* the media in your app. You can't individually separate out some items for subscription or rental while keeping other items free.
*  If users are in the middle of a movie when their Daily Pass or Subscription expires, the users will be able to finish their movie. The In-App Purchasing component won't stop their movie.
*  You cannot offer multiple subscription types for the same media. For example, you can't offer both an SD or HD option to watch content, nor can you offer monthly or yearly subscriptions.
*  Daily Pass can be used only on the device in which the purchase was made. A user cannot log into another Fire TV device and expect the Daily Pass to work.
*  Go Premium subscriptions work across devices (meaning a user can buy a subscription in one Fire TV device and get the same benefits on another Fire TV device). However, users who log into another device will need to restart the app before the subscription takes effect.
*  If a user purchases a Daily Pass, but then the Fire TV app crashes and the data is lost, the user will no longer have access to Daily Pass on the app. However, with Go Premium (subscriptions), app crashes will not void the user's access after he or she restarts the app.
*  If a user subscribes to content and then clicks the Back button, he or she will see the Go Premium button again. But if the user tries to subscribe a second time, the app will recognize that the user is already a subscriber and will provide access to the media.

## Amazon In-App Purchasing Component Workflow

The following steps describe the payment workflow for the Amazon In-App Purchasing Component:

1.  When the app starts, the app uses the In-App Purchasing API to fetch the existing purchases for the current logged-in user and updates the products with the latest receipts.
2.  On a Content Details screen, the user clicks either a Daily Pass or Go Premium button (assuming the user hasn't yet purchased the media).
3.  The app makes a call to the In-App Purchasing API to perform the transaction with the user for the items. The user completes the purchase through the Fire TV account options the user has already set up.
4.  If the transaction is successful, the In-App Purchasing API returns a receipt ID from the transaction to the app.
5.  (Optional) To ensure the receipt is valid, you can set up your own service to send a request to the Receipt Verification service and check if the receipt ID is valid.
6.  If the receipt ID is valid, the app lets the user view the media.

## Configuration Overview

To set up the In-App Purchasing Component, you create a Daily Pass item (consumable) and Go Premium item (subscription) in the In-App Items section of the Developer Portal. You configure your app with the SKU information for these items. The component sends the necessary information to the In-App Purchasing API.

The Amazon In-App Purchasing Component uses a wrapper over the IAP API. Only a limited feature set from IAP is available; as such, the documentation here is a modified subset of the [IAP documentation](https://developer.amazon.com/public/apis/earn/in-app-purchasing).

### Daily Pass (Consumables) and Go Premium (Subscriptions)

In-App Purchasing offers 3 types of items: consumables, entitlements, and subscriptions. Only consumables and subscriptions are relevant to the In-App Purchasing Component in Fire App Builder (this component is a wrapper on top of the IAP API, so not all of the IAP API features apply). Both consumable and subscription items (not entitlements) map to the Amazon In-App Purchase Component.

Here's the difference between consumables and subscriptions:

*  **Consumables** (mapped to **Daily Pass**) are consumed within the app and can be purchased multiple times. Consumables are mapped to "Daily Pass" in Fire App Builder. Choose this type of item if you want users to have access to all your media for a 24-hour access period.
*  **Subscriptions** (mapped to **Go Premium**) offer access to a premium set of content or features for a limited period of time. Subscriptions are mapped to "Go Premium" in Fire App Builder. Choose this type of item if you want users to have access to all your media throughout the subscription period.

### Amazon In-App Purchasing Component Configuration

To configure the In-App Purchase Component, follow these steps:

*  [Step 1. Create an App in the Developer Portal](#createapp)
*  [Step 2. Create a Consumable (Daily Pass) In-App Item](#creategopassitem)
*  [Step 3. Create a Subscription (Go Premium) In-App Item](#creategopremiumitem)
*  [Step 4. Download and Push the JSON Data File](#downloadjson)
*  [Step 5. Enable the In-App Purchasing Component in your App](#enableiap)
*  [Step 6. Map In-App Items to Content in Your App](#mapinappitems)
*  [Step 7. Customize the Button Text](#buttontext)
*  [Step 8. Set Up the App Tester](#apptester)

## Step 1. Create an App in the Developer Portal {#createapp}

If you have already created an app, you can skip this section jump to the [next](#creategopassitem).

1.  Log in to the Developer Portal.
2.  Click **Add a New App** and select **Android** in the "Choose a Platform" dialog box. Then click **Next**.
3.  Complete the basic fields for your app:

    *  **App title**: The name of your app.
    *  **App SKU**: A unique identifier for your app (for example, `mycalypsomediapp1234`). The SKU has a max length of 150 characters and can contain the characters a-z, A-Z, 0-9, underscores, periods, and dashes. SKUs are case-sensitive.
    *  **Category**: Choose a category appropriate for your app and media. {% comment %} is this the same app that gets submitted to the appstore? will users eventually need to come back to this and add their app when they submit to to the app store?{% endcomment %}
4.  For **Customer Support Contact**, select **Use my default support information** or customize the fields. Then click **Save**.

## Step 2. Create a Consumable (Daily Pass) In-App Item {#creategopassitem}

1.  Log in to the Developer Portal, go to the **Dashboard**, and click your app.
2.  Click the **In-App Items** tab in your app.
4.  Click the **Add a Consumable** button (for Daily Pass).

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" max-width="650px" type="png" caption="<b>Figure 5.</b> Rental options dialog box that appears to users." %}

5.  Enter values for the fields defined in the following table:

    <table class="grid">
      <thead>
        <tr>
          <th>Field</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Title</strong></td>
          <td>A name to refer to this item. This title doesn’t appear to users. Type <strong>Daily Pass</strong> for convenience.</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>A unique-to-your-app string that becomes the ID for the item. For example, “com.amazon.example.iap.mydailypassitem”. The SKUs used by your app must match the SKUs that you submit to the Amazon Appstore. The same SKU character requirements as before (when you created an app) apply here too.</td>
        </tr>
        <tr>
          <td><strong>Content delivery</strong></td>
          <td>Choose <strong>No additional file required</strong> since the content to be delivered is included as part of the app.</td>
        </tr>
      </tbody>
    </table>

6.  Click **Save**.

    {% include note.html content="You must save the information that you have entered on one tab before moving to the next tab." %}

7.  Click the **Availability & Pricing** tab and complete the details as described in the following table.

    <table class="grid">
      <thead>
        <tr>
          <th>Field</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Are you charging for this consumable / entitlement?</strong></td>
          <td>Select <strong>Yes, my base price is…</strong> A field displays allowing you to set the base price and currency for the item. After you set the base price, you can manually set the price for other currencies, or allow the Amazon Appstore to set those prices for you, based on conversion rates and taxes. Valid prices (in USD) can either be $0.00 or range from $0.99 to $99.00.</td>
        </tr>
        <tr>
          <td><strong>When would you like this item to be available?</strong></td>
          <td>Specify a date (IAP uses the PST time zone) for the item to become available in your app. If you leave this field blank, the item will become available as soon as it is approved by the Amazon Appstore.</td>
        </tr>
      </tbody>
    </table>

8.  Click **Save**.    
9.  Click the **Description** tab.
10. Complete the **Display Title** and **Description** fields.

    These fields appear in the dialog box after users click **Daily Pass**:

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" max-width="650px" type="png" caption="<b>Figure 6.</b> Title and Description fields that appear to users." %}

11. Click **Save**.
12. Click the **Images** tab.
13. Upload the two images as indicated based on the size requirements. Only the 114px image appears in the Daily Pass dialog box users see in the app. In the previous screenshot (Figure 6), the image is simply an orange square. For convenience in creating the 512px image, you can upload this [generic image](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/512.png).
14. Click **Save**.

    {% include warning.html content="Do not click **Submit In-App Item** button yet. After you submit the in-app item, you cannot edit or remove it. Only click this button after you have fully tested the functionality and content and are ready to submit your app." %}

## Step 3. Create a Subscription (Go Premium) In-App Item {#creategopremiumitem}

1.  If necessary, log in to the Developer Portal, go to the **Dashboard**, and click your app.
2.  Click the **In-App Items** tab in your app.
4.  Click the **Add a Subscription** button (for Go Premium). (In the "Subscriptions are not supported by Amazon Underground" prompt, click the **Add a Subscription** button again.)
5.  Enter values for the fields defined in the following table:

    <table class="grid">
      <thead>
        <tr>
          <th>Field</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Title</strong></td>
          <td>A name to refer to this item. This title doesn’t appear to users. Type <strong>Go Premium</strong> for convenience.</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>A unique-to-your-app string that becomes the ID for the item. For example, “com.amazon.example.iap.mygopremiumitem.” The SKUs used by your app must match the SKUs that you submit to the Amazon Appstore. The same SKU character requirements as before (when you created an app) apply here too.</td>
        </tr>
        <tr>
          <td><strong>Content delivery</strong></td>
          <td>Choose <strong>No additional file required</strong> since the content to be delivered is included as part of the app.</td>
        </tr>
      </tbody>
    </table>

6.  Click **Save**.

    {% include note.html content="You must save the information that you have entered on one tab before moving to the next tab." %}

7.  Click the **Subscription Periods** tab and complete the fields as defined in the following table.

     <table class="grid">
      <thead>
        <tr>
          <th>Field</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Subscription Period</strong></td>
          <td>Select the subscription duration you want. Subscription periods start on the date of purchase.</td>
        </tr>
        <tr>
          <td><strong>SKU</strong></td>
          <td>Enter a unique SKU (using the same guidelines as the other SKU fields) that corresponds to this subscription period. For example, “com.amazon.example.calypso.monthly”. This SKU is a child SKU of the SKU that you entered on the item detail page.</td>
        </tr>
        <tr>
          <td><strong>Free Trial</strong></td>
          <td>If desired, specify an optional free trial period for the subscription.</td>
        </tr>
        <tr>
          <td><strong>Are you charging for this subscription?</strong></td>
          <td>Select <strong>Yes, my base price is…</strong>. A field displays allowing you to set the base price and currency for the item. After you set the base price, you can manually set the price for other currencies, or allow the Amazon Appstore to set those prices for you, based on conversion rates and taxes. Valid prices (in USD) can either be $0.00 or range from $0.99 to $99.00.</td>
        </tr>
        <tr>
          <td><strong>When would you like this item to be available?</strong></td>
          <td>Specify a date (IAP uses the PST time zone) for the item to become available in your app. If you leave this field blank, the item will become available as soon as it is approved by the Amazon Appstore.</td>
        </tr>
      </tbody>
    </table>

    {% include note.html content="Ignore the \"Save and Add a Subscription Period\" button. The Purchasing Component supports only one subscription period." %}

8.  Click **Save**.
9.  Click the **Description** tab.
10. Complete the **Display Title** and **Description** fields.

    These fields appear in the dialog box after users click **Go Premium**:

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaprentaloptions" max-width="650px" type="png" caption="<b>Figure 7.</b> Title and Description fields that appear to users." %}

11. Click **Save**.
12. Click the **Images** tab.
13. The image icons are required by the Developer Console, but unlike with Daily Pass (consumables), *neither* image is used in the Go Premium dialog box users see in the app. For convenience, you can upload this generic [114px image](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/114.png) and this generic [512px image](https://images-na.ssl-images-amazon.com/images/G/01/mobile-apps/dex/firetv/512.png) image.
14. Click **Save**.

    {% include warning.html content="Do not click **Submit In-App Item** button yet. After you submit the in-app item, you cannot edit or remove it. Only click this button after you have fully tested the functionality and content and are ready to submit your app." %}

## Step 4. Download and Push the JSON Data File {#downloadjson}

After you have configured both the Daily Pass and Go Premium in-app items, you will download a JSON file containing details about your in-app items.

1.  If necessary, log in to the Developer Portal, go to the **Dashboard**, and click your app.
2.  Click the **In-App Items** tab in your app.
4.  Click the **JSON Data File** button to download the JSON file containing your in-app item details.

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_jsonfiledownload" max-width="650px" type="png" content="<b>Figure 8.</b> Download the JSON file." %}

4.  In your terminal, `cd` to the directory containing this file.
5.  Push the JSON file to your Fire TV device using the following command:

    ```bash
    $ adb push amazon.sdktester.json /mnt/sdcard/
    ```

    The response will show the following: `[100%] /mnt/sdcard/amazon.sdktester.json`

## Step 5. Enable the In-App Purchasing Component in your App {#enableiap}

Now that you've created your in-app items in the Developer Console, you need to enable the IAP component in your app.

1.  Load the Amazon In-App Purchasing component into your app. See the [Load a Component][fire-app-builder-load-a-component] for details about how to load a component into your app.
2.  In Android Studio (in the Android view), expand the **ContentBrowser** folder and go to **res > values** and open the custom.xml file.
3.  Copy the following string:

    ```xml
    <bool name="is_iap_disabled">true</bool>
    ```

    (Ignore the `override_all_contents_subscription_flag` field.)

4.  In your **app** folder, open the **custom.xml** file (located in **res > values**).
5.  Paste the string you copied earlier and change the value to `false`. Make sure to paste this string inside the `resources` tags:

    ```xml
    <bool name="is_iap_disabled">false</bool>
    ```

    {% include tip.html content="Note that this is a double negative, so in this case `false` is actually enabling IAP in your app." %}

    The values in your app's custom.xml will overwrite any values in the components.

## Step 6. Map In-App Items to Content in Your App {#mapinappitems}

1.  In Android Studio (in the Android view), expand the **PurchaseInterface** folder and go to **assets**. Then open the **skuslist.json** file.

2.  For "Go Premium" items, replace the fields in red with your Go Premium item's SKU value:

    <pre>
    {
      "skusList": [
        {
          "sku": "<span class="red">testSubSku</span>",
          "productType": "SUBSCRIBE",
          "purchaseSku": "<span class="red">testSubSku</span>"
        },
        {
          "sku": "RentUnPurchased",
          "productType": "RENT",
          "purchaseSku": "RentUnPurchased"
        }
      ],
      "actions": {
        "CONTENT_ACTION_DAILY_PASS": "RentUnPurchased",
        "CONTENT_ACTION_SUBSCRIPTION": "<span class="red">testSubSku</span>"
      }
    }
    </pre>

    Go Premium items are subscriptions.

2.  For "Daily Pass" items, replace the fields in red with your Daily Pass item's SKU value:

    <pre>
    {
      "skusList": [
        {
          "sku": "testSubSku",
          "productType": "SUBSCRIBE",
          "purchaseSku": "testSubSku"
        },
        {
          "sku": "<span class="red">RentUnPurchased</span>",
          "productType": "RENT",
          "purchaseSku": "<span class="red">RentUnPurchased</span>"
        }
      ],
      "actions": {
        "CONTENT_ACTION_DAILY_PASS": "<span class="red">RentUnPurchased</span>",
        "CONTENT_ACTION_SUBSCRIPTION": "testSubSku"
      }
    }
    </pre>

    Daily Pass items are rentals.

## Step 7. Customize the Button Text {#buttontext}

You can customize the "Daily Pass" or the "Go Premium" text that appears on the buttons on the Content Details screen.

To customize the text:

1.  Go to **ContentBrowser > res > values** and open the **strings.xml** file.
2.  Copy the following strings and paste them into your app's **custom.xml** file:

    ```xml
    <string name="premium_1">Go</string>
    <string name="premium_2">Premium</string>
    <string name="daily_pass_1">Daily</string>
    <string name="daily_pass_2">Pass</string>
    ```

    You can also copy other strings related to the Purchasing Component here as well:

    ```xml
    <string name="iap_error_dialog_title">Purchase Error</string>
    <string name="subscription_expired">Your subscription expired!</string>
    <string name="ok">Ok</string>
    ```
3. Customize the string values as desired.

## Step 8. Set Up the App Tester {#apptester}

Now that you've set up your in-app items and configured your app, it's time to test out the integration and see how IAP interacts with your media.

To test the in-app items in your app &mdash; without actually making purchases and before submitting your app to the Appstore &mdash; you need to use the IAP App Tester. The App Tester simulates calls to the In-App Purchasing API using a sandbox server. The App Tester intercepts the outgoing call to the In-App Purchasing API and returns a valid receipt, authorizing the transaction. (For more details, see [Installing and Configuring the App Tester](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/installing-and-configuring-app-tester).)

To set up the App Tester:

1.  Using your computer's browser, go to the [App Tester from the AppStore](https://www.amazon.com/gp/product/?ie=UTF8&ASIN=B00BN3YZM2&ref=mas_ty), get the app (it's free), and deliver it to your device.

    To deliver this app to your device, you must be signed in to Amazon with the same credentials as you used in setting up your Fire TV. Use the "Deliver to" options to deliver the app to your device:

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_apptesterdelivery" max-width="650px" type="png" caption="<b>Figure 9.</b> The Delivery options allows you to send the app to your Fire TV device." %}

2.  On your Fire TV, sync your purchases with your device by going to **Settings > My Account > Sync Amazon Content**.
3.  From the Fire TV home screen, go to **Apps** and select **Your Apps Library**.
4.  Select the **Amazon App Tester** app and download and install the app from the cloud.
5.  Launch the app after the download completes.
6.  Click the **In-App Purchasing API** section.

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_iaptester" type="png" max-width="650px" caption="<b>Figure 10.</b> In-App Purchasing API option." %}

7.  Select **5. IAP Items in JSON File** and verify that items from your amazon.sdktester.json file appear here.

### Simulate Calls

With your app and the testing tool configured, you can now simulate calls to the IAP service.

1.  Using [ADB][fire-app-builder-connecting-adb-to-fire-tv], start your app.
2.  Browse to media in your app.
2.  Click the **Daily Pass** or **Subscribe** button.

### Adjust In-App Items

You may want to adjust the Title and Description fields on the Description tab based on how the text appears in your app.

You can adjust the text or other features in your in-app items by logging into the Developer Console, clicking your app, and going to the In-App Items tab. As long as you haven't submitted your app, all the fields remain editable.

### Reset Purchases

As you're testing the functionality, you can reset any Daily Pass or Go Premium purchases by doing the following:

1.  Go to **Settings > Applications > Manage Installed Applications > Amazon App Tester**.
2.  Select **Clear data** and **Clear cache**.

## Implementing Additional Security Through the Receipt Verification Service {#additionalsecurity}

When users purchase a Daily Pass or Subscription to your media, the IAP API returns a receipt to your app indicating whether the transaction was valid. Since your users own the Fire TV device, it's possible for hackers to re-route the outgoing call to the IAP API and spoof the receipt.

To verify that the receipt returned from the IAP API is valid, you can set up *your own service* to call Amazon's [Receipt Verification Service](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/verifying-receipts-in-iap-2.0).

Here's the general process for verifying receipts with the Receipt Verification Service using Fire App Builder:

*  You set up your own server (using any language/platform) to send the receipt and an accompanying secret to the Receipt Verification Service, asking whether the receipt is valid.
*  Your app sends a request with the required properties and shared secret to the Receipt Verification Service to check the receipt. The required values can be found in **AmazonInAppPurchaseComponent > res > values values.xml**.
*  After verifying the secret, the Receipt Verification Service determines whether the receipt is valid and responds with a true or false answer.

This workflow is not included in Fire App Builder. By default, Fire App Builder just makes a call to a dummy service that always returns valid. Specifically, in Fire App Builder, the `AReceiptVerifier` class (in the AmazonInAppPurchaseComponent folder) is an abstract class that you extend with your own subclass to call the Receipt Verification Service.

`DefaultReceiptVerificationService` is a sample subclass in Fire App Builder that extends `AReceiptVerifier`. The `validateReceipt` method inside `DefaultReceiptVerificationService` implements all the required parameters needed by the Receipt Verification Service:

*  `context`:   The application context
*  `requestId`: The request id for this request
*  `sku`:       SKU for the receipt
*  `userData`:  User data of the current user
*  `receipt`:   The receipt to validate
*  `listener`:  The listener of this request
*  `return`: The request id

However, `validateReceipt` doesn't actually call the Receipt Verification Service. Instead, this method just returns a status of successful.

You must create your own subclass to extend `AReceiptVerifier` with an actual call to the Receipt Verification Service. (Alternatively, you can customize the `DefaultReceiptVerificationService` subclass.)

When you create the subclass, you must provide your subclass name in your app's custom.xml file by doing the following:

1.  Go to **AmazonInAppPurchaseComponent > res > values** and open the **strings.xml** file.
2.  Copy the following `receipt_verification_service_iap` string, and paste it into your app's **custom.xml** file (inside app > resources):

    ```xml
    <string name="receipt_verification_service_iap">com.amazon.inapppurchase.DefaultReceiptVerificationService</string>
    ```

3.  In your app's **custom.xml** file, update the string's value with the complete path to your subclass. By default, the `DefaultReceiptVerificationService` is configured. You can either create a new subclass or simply overwrite the `DefaultReceiptVerificationService` class.

For more details on calling the Receipt Verification Service, see [Receipt Verification Service](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/verifying-receipts-in-iap-2.0).

{% include note.html content="The level of security you implement depends on the resources you want to expend verifying legitimate transactions. If it makes sense to manage your own server to verify receipts (because your revenue exceeds the cost of managing your own server), you should implement this additional security with the Receipt Verification Service. However, if revenue from your app doesn't exceed the cost or hassle of managing your own server, you might want to omit this extra setup." %}

## Submitting In-App Items to Production

So far this documentation has explained how to configure your app in a test environment. When you're ready to submit your app to the app store, you need to also submit the in-app items.

To submit the in-app items:

1.  Log in to the Developer Portal, go to the **Dashboard**, and click your app.
2.  Click the **In-App Items** tab in your app.
3.  Click the **Submit In-App Item** button.

After you submit your in-app items, you can no longer edit them. If you need to change the in-app items that have already been submitted, you will need to either [create a new version of your app](https://developer.amazon.com/public/support/submitting-your-app/tech-docs/submitting-your-app#Updating an Existing App) or create a new app and add new in-app items to the app.

{% include links.html %}
