---
title: Integrate Entitlements Into Web Apps
permalink: iap-entitlements-webapps.html
sidebar: in_app_purchasing
product: In-App Purchasing
reviewers: jeffersd
github: true
toc: false
---

Entitlement represents a one-time purchase to unlock access to features or content within an app or game.

* TOC
{:toc}

## Entitled Content Overview? 

Entitled content includes any type of content that you sell within your app that requires access rights. This type of content is available anywhere the customer is logged into the Amazon Client. The entitled content does not expire. If Amazon needs to rescind the entitled content, your app will be notified via an observable event.

Customers purchase a specific entitlement only once. The entitled content is granted to the customer's Amazon account. That entitled content is used to access the content from any eligible device the customer has linked to their account.

Entitled content typically include items such as:

*   Additional levels for a game
*   Access to a previously purchased item like a magazine issue
*   Unlocking functionality already in your app

The PurchasingManager grants the entitlement; you do not have to do anything outside of initiating the purchase. Through the PurchasingManager you can retrieve purchase data and revoked entitlements to determine if a customer has an entitlement for a specific SKU.


{% include image.html file="in_app_purchasing/entitled-content" type="png"   %}

## Sample Implementation

The following sample code is an excerpt of the Button Clicker Sample App showing the relevant logic for integrating entitled content. In the app, button colors are purchasable entitled content items. The complete app and source code is provided as part of the Amazon Web App Resources.

### ButtonClicker Sample Code Entitlements

```javascript
/**
     * Button Clicker
     * Sample Implementation of the In-App Purchasing APIs
     * © 2013, Amazon.com, Inc. or its affiliates.
     * All Rights Reserved.
     * Licensed under the Apache License, Version 2.0 (the "License").
     * You may not use this file except in compliance with the License.
     * A copy of the License is located at
     * http://aws.amazon.com/apache2.0/
     * or in the "license" file accompanying this file.
     * This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
     * implied.
     * See the License for the specific language governing permissions and limitations under the License.
     */

    // This is a simple model view controller setup to deal with
    // the fact that the onPurchaseResponse can get called at any
    // time.
    //
    // The fact that responses can come at any time is important to
    // understand when using the API. Your web app could have been
    // shut down prior to a receipt being delivered for instance. The
    // next time your application runs the receipt will be delivered
    // upon initializing the API in this case.

    // Model Data

    var activeButton = "";
    var hasRedButton = false;
    var hasGreenButton = false;

    $(function() {
        initialize();
    });

    // Setup

    function initialize() {
        amzn_wa.enableApiTester(amzn_wa_tester);
        refreshPageState();

        // Setup button press handlers
        $("#redButton").click(function() { redButtonPressed(); });
        $("#greenButton").click(function() { greenButtonPressed(); });

        document.addEventListener("amazonPlatformReady", function(oData) {
            // Ensure we can call the IAP API
            if (amzn_wa.IAP == null) {
                console.log("Amazon In-App-Purchasing only works with Apps from the Appstore");
            } else {
                // Registers the appropriate callback functions
                amzn_wa.IAP.registerObserver({
                        // Called the the IAP API is available
                        'onSdkAvailable': function(resp) {
                            if (resp.isSandboxMode) {
                                // In a production application this should trigger either
                                // shutting down IAP functionality or redirecting to some
                                // page explaining that you should purchase this application
                                // from the Amazon Appstore.
                                //
                                // Not checking can leave your application in a state that
                                // is vulnerable to attacks. See the supplied documention
                                // for additional information.
                                alert("Running in test mode");
                            }

                            // You should call getPurchaseUpdates to get any purchases
                            // that could have been made in a previous run.
                            amzn_wa.IAP.getPurchaseUpdates(amzn_wa.IAP.Offset.BEGINNING);
                        },

                        // Called as response to getUserId
                        'onGetUserIdResponse': function(resp) {},

                        // Called as response to getItemData
                        'onItemDataResponse': function(data) {},

                        // Called as response to puchaseItem
                        'onPurchaseResponse': function(data) { onPurchaseResponse(data); },

                        // Called as response to getPurchaseUpdates
                        'onPurchaseUpdatesResponse': function(resp) { onPurchaseUpdatesResponse(resp); }
                });
            }
        });
    }

    // Controller

    // Excercises entitlements
    function redButtonPressed() {
        if (hasRedButton) {
            activeButton = "red";
        } else {
            buyButton("sample.redbutton");
        }
        refreshPageState();
    }

    // Excercises entitlements
    function greenButtonPressed() {
        if (hasGreenButton) {
            activeButton = "green";
        } else {
            buyButton("sample.greenbutton");
        }
        refreshPageState();
    }

    function buyButton(buttonName) {
        if (amzn_wa.IAP == null) {
            alert("You cannot buy this button, Amazon In-App-Purchasing works only with Apps from the Appstore.");
        } else {
            alert("Calling purchaseItem: " + buttonName);
            amzn_wa.IAP.purchaseItem(buttonName);
        }
    }

    // Handler functions that are called from the callbacks

    // purchaseItem will cause a purchase response with one receipt
    function onPurchaseResponse(e) {
        if (e.purchaseRequestStatus == amzn_wa.IAP.PurchaseStatus.SUCCESSFUL) {
            handleReceipt(e.receipt);
        } else if (e.purchaseRequestStatus == amzn_wa.IAP.PurchaseStatus.ALREADY_ENTITLED) {
            amzn_wa.IAP.getPurchaseUpdates(amzn_wa.IAP.Offset.BEGINNING)
        }
        refreshPageState();
    }

    // getPurchaseUpdates will return an array of receipts
    function onPurchaseUpdatesResponse(e) {
        for (var i = 0; i < e.receipts.length; i++) {
            handleReceipt(e.receipts[i]);
        }
        refreshPageState();
    }

    // In either case, the contents of the receipt are handled in the same way
    function handleReceipt(receipt) {
        alert(receipt.sku);
        if (receipt.sku == "sample.redbutton") {
            // Entitlement
            hasRedButton = true;
        } else if (receipt.sku == "sample.greenbutton") {
            // Entitlement
            hasGreenButton = true;
        }
    }

    // Make the view (HTML) look like the model

    function refreshPageState() {

        var button = $("#theButton");
        var redButton = $("#redButton");
        var greenButton = $("#greenButton");

        setClass(redButton, "locked", !hasRedButton);
        setClass(greenButton, "locked", !hasGreenButton);

        setClass(redButton, "active", activeButton == "red");
        setClass(greenButton, "active",  activeButton == "green");

        if (activeButton != "") {
            button.css("background-color", activeButton);
        } else {
            button.css("background-color", "gray");
        }
    }

    function setClass(element, className, condition) {
        if (condition) {
            element.addClass(className);
        } else {
            element.removeClass(className);
        }
    }
```

[Next Step: Integrating Subscription Content Into Your App][iap-subscription-webapps]

{% include links.html %}
