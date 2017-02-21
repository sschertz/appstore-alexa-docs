---
title: Load a Component in Your App
permalink: fire-app-builder-load-a-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

For an overview of the available components, see [Components Overview][fire-app-builder-interfaces-and-components]. Although documentation for each component is specific, you load all components in the same way.

* TOC
{:toc}

## Remove a Component {#removeacomponent}

With the exception of analytics components, you can load only one component per interface. For example, you cannot load both the VAST Ads component and the Freewheel Ads component, because both components use the same `IAds` interface.

However, all components require you to have a component loaded in your app for that interface. Because of this requirement, some components are dummy components that simply fulfill the requirements of having a component. The following are dummy components:

*  LoggerAnalyticsComponent
*  PassThroughAdsComponent

These dummy components don't do anything. For a list of the component interface groups, see [Available Components and Interfaces][fire-app-builder-interfaces-and-components#componentgroups] in Components Overview.

Before you load a component in your app, check to make sure you have only one component for that interface. If another component is already loaded (as is probably the case, even if it's just a dummy component) you must remove it.

To remove a component from your app, follow the same steps as listed in the next section, "Load a Component in Your App," but do the opposite in each step. For example, instead of adding a reference to something like `FacebookAuthComponent`, you would remove the reference.

## Load a Component in Your App {#loadcomponent}

To load a component in your app:

*  [1. Define the Component](#define)
*  [2. Compile the Component in Your Project](#compile)
*  [3. Configure the Component's Values](#configure)

### 1. Define the Component {#define}

{% include note.html content="These instructions assume you're in the Android view." %}

First you need to first define the implemented components in the "settings.gradle (Project Settings)" file:

1.  In Android Studio, expand **Gradle Scripts**.
2.  Open the **settings.gradle (Project Settings)** file. (This file appears near the bottom of the list.)
3.  Define the implementations that you want to include in your app. In settings.gradle (Project Settings), there are two places to define the components. Each area is identified through the `/* Implementations */` comments, which are set off in <span class="red">red</span> here:

    <pre>
    include ':app',
        /* Frameworks */
        ':TVUIComponent',
        ':UAMP',
        /* Libraries */
        ':ContentModel',
        ':ContentBrowser',
        ':DynamicParser',
        ':DataLoader',
        ':Utils',
        /* Interfaces */
        ':PurchaseInterface',
        ':AuthInterface',
        ':AdsInterface',
        ':AnalyticsInterface',
        ':ModuleInterface',
        <span class="red">/* Implementations */</span>
        ':PassThroughAdsComponent',
        ':AMZNMediaPlayerComponent',
        ':FlurryAnalyticsComponent',
        ':LoginWithAmazonComponent',
        ':AmazonInAppPurchaseComponent'

    /* Frameworks */
    project(':TVUIComponent').projectDir = new File(rootProject.projectDir, '../TVUIComponent/lib')
    project(':UAMP').projectDir = new File(rootProject.projectDir, '../UAMP')

    /* Libraries */
    project(':ContentModel').projectDir = new File(rootProject.projectDir, '../ContentModel')
    project(':ContentBrowser').projectDir = new File(rootProject.projectDir, '../ContentBrowser')
    project(':DynamicParser').projectDir = new File(rootProject.projectDir, '../DynamicParser')
    project(':DataLoader').projectDir = new File(rootProject.projectDir, '../DataLoader')
    project(':Utils').projectDir = new File(rootProject.projectDir, '../Utils')

    /* Interfaces */
    project(':ModuleInterface').projectDir = new File(rootProject.projectDir, '../ModuleInterface')
    project(':AdsInterface').projectDir = new File(rootProject.projectDir, '../AdsInterface')
    project(':AuthInterface').projectDir = new File(rootProject.projectDir, '../AuthInterface')
    project(':AnalyticsInterface').projectDir = new File(rootProject.projectDir, '../AnalyticsInterface')
    project(':PurchaseInterface').projectDir = new File(rootProject.projectDir, '../PurchaseInterface')

    <span class="red">/* Implementations */</span>
    project(':AMZNMediaPlayerComponent').projectDir = new File(rootProject.projectDir, '../AMZNMediaPlayerComponent')
    project(':PassThroughAdsComponent').projectDir = new File(rootProject.projectDir, '../PassThroughAdsComponent')
    project(':FlurryAnalyticsComponent').projectDir = new File(rootProject.projectDir, '../FlurryAnalyticsComponent')
    project(':FacebookAuthComponent').projectDir = new File(rootProject.projectDir, '../FacebookAuthComponent')
    project(':AmazonInAppPurchaseComponent').projectDir = new File(rootProject.projectDir, '../AmazonInAppPurchaseComponent')
    </pre>

    Look in the source directory of the Fire App Builder project to get the exact directory names for each of the components, or just consult the tables that list each component name above.

    In the the sample app in Fire App Builder, the following components are implemented by default:

    * AMZNMediaPlayerComponent    
    * PassThroughAdsComponent
    * FlurryAnalyticsComponent
    * FacebookAuthComponent
    * AmazonInAppPurchaseComponent

4.  Adjust the implementations by adding or removing the components you want to use in your app. Be sure to make the updates in both places that <span class="red">`/*Implementations*/`</span> is mentioned.

    For the component names, you can either browse to the directory where you downloaded the project (using your File Explorer or Finder) to see the component folders. Or you can use the names as listed in the tables above.

5.  You can implement only one component per interface, so remove any previous components for the same interface.

    For example, if you added the LoginWithAmazonComponent, which implements the `IAuthentication` interface, you must remove any other authentication components (such as AdobepassAuthComponent or FacebookAuthComponent).

### 2. Compile the Component in Your Project {#compile}

After defining the component in the settings.gradle file, you must load the component through the "build.gradle (Module: app)" file.

1.  In Android Studio, expand **Gradle Scripts**.
2.  Open the **build.gradle (Module: app)** file.
3.  In the **dependencies** object, include the components you want to include in your app.

    By default, Fire App Builder shows these components:

    ```
    compile project(':TVUIComponent')
    compile project(':UAMP')
    compile project(':AMZNMediaPlayerComponent')
    compile project(':PassThroughAdsComponent')
    compile project(':FlurryAnalyticsComponent')
    compile project(':FacebookAuthComponent')
    compile project(':AmazonInAppPurchaseComponent')
    ```

    Add or remove the components you want to implement following the same pattern shown here.

4.  You can implement only one component per interface, so remove any previous components for the same interface .

    For example, if you added the LoginWithAmazonComponent, which implements the `IAuthentication` interface, you must remove any other authentication components (such as AdobepassAuthComponent or FacebookAuthComponent).

5.  Resync your project with Gradle by clicking the **Resync Project with Gradle Files** button {% include inline_image.html file="firetv/fireappbuilder/images/fireappbuilder_syncgradlebutton" type="png" %}.

    When the project rebuilds, you will see the new components reflected in the Project pane.

6.  Go to **Build > Rebuild Project** to get remove the old build artifacts from previous components.


### 3. Configure the Component's Values {#configure}

Each component has a list of customizable strings that have been extracted out into an XML file. Because of this, to customize a component with your app, you merely need supply the right values for the strings. You don't need to modify the Java classes.

However, different components may require a little more setup than others, especially because they usually involve implementing and setting up a third-party service. Because of this, see the documentation for each component for the specifics of how to configure the component:

*  [AdobepassAuthComponent][fire-app-builder-adobe-pass-auth-component]
*  [FacebookAuthComponent][fire-app-builder-facebook-auth-component]
*  [FlurryAnalyticsComponent][fire-app-builder-flurry-analytics-component]
*  [OmnitureAnalyticsComponent][fire-app-builder-omniture-analytics-component]
*  [CrashlyticsComponent][fire-app-builder-crashlytics-component]
*  [FreeWheelAdsComponent][fire-app-builder-freewheel-ads-component]
*  [VastAdsComponent][fire-app-builder-vast-ads-component]
*  [AmazonInAppPurchasingComponent][fire-app-builder-amazon-in-app-purchase-component]
*  [AMZNMediaPlayerComponent][fire-app-builder-amazon-media-player-component]
<!--*  [BrightCoveMediaPlayerComponent][fire-app-builder-brightcove-media-player-component]-->

The strings that have been extracted for each component are the most common parameters only. If you need an additional feature within the component that hasn't been extracted out into a string, you will need to customize it in the component's class that implements the interface.

The XML files for each component are usually located in the component's **res > values** folder.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_stringcustomization" type="png" caption="The most common values that you would need to customize for each component are extracted out into XML files." %}

{% include note.html content="Instead of customizing the values directly in the component's files, copy the strings into your **app**'s custom.xml file instead. Any settings in your app's custom.xml file will overwrite the XML file values in the components. This will allow you to incorporate updates to the component when new releases are available." %}

{% include links.html %}
