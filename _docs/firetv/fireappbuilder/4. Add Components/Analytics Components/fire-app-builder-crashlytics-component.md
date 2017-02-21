---
title: Crashlytics Component
permalink: fire-app-builder-crashlytics-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

[Crashlytics](http://try.crashlytics.com/) (by Fabric) provides analytics around app crashes. According to Crashlytics:

>Crashlytics is the #1 crash reporting platform for iOS and Android. With Crashlytics, you get real-time information on crashes inside your app, and all the details you need to tackle the most impactful stability issues head-on. Simply including Crashlytics will enable crash reporting right away - there’s no extra code to write. &mdash; [Crashlytics](https://docs.fabric.io/android/crashlytics/overview.html)

 You can read more in the [Crashlytics documentation for Android](https://docs.fabric.io/android/crashlytics/overview.html). The Crashlytics Component implements the `IAnalytics` interface. You must have a Crashlytics account to use this component.

Follow these steps to configure Crashlytics:

* TOC
{:toc}

## Step 1. Get Your Crashlytics Key {#step1}

Assuming you don't already have a Crashlytics account, sign up for one and provide details about your app:

1.  Go to [https://fabric.io/sign_up](https://fabric.io/sign_up) and sign up for an account.
2.  Walk through the confirmation and welcome screens.
3.  When you get to the "You'll be up and running in 3 steps!" screen, **ignore it**.

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fabricgettingstarted" type="png" caption="Instead of downloading and installing the Fabric plugin with these 3 steps, ignore this screen entirely. You do not need to install the Fabric plugin in Android Studio to install or configure the Crashlytics component. This plugin simply ensures you have made updates to your code that allow Crashlytics to work. In the steps that follow, you will be making adjustments to the Fire App Builder code that will accomplish a similar end." %}

4.  Go to the [Install Crashlytics via Gradle](https://fabric.io/kits/android/crashlytics/install) page and look in the "Add Your API Key" section.

    Assuming you're still logged in, you'll see your API key automatically populated in the code sample:

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_fabricapikey" type="png" caption="The code samples automatically populate with your API key." %}

5.  Copy this API key into a convenient place to use in later steps.

(If you already have an existing Crashlytics account and have viewed crashlytics stats previously, you can also view your API key from your organization's settings. See [Fabric API Keys](https://docs.fabric.io/android/fabric/settings/api-keys.html).)

## Step 2. Configure the Crashlytics Component {#step2}

These instructions assume you're in the Android view.

1.  Load the Crashlytics component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component] for details about how to load a component into your app.
2.  Remove any other analytics components that are loaded in your app (such as FlurryAnalyticsComponent, OmnitureAnalyticsComponent, or LoggerAnalyticsComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.    

    {% include_relative componentnote_analytics.html %}

3.  Expand **Gradle Scripts** and and open the **build.gradle (Project: Application)** file. (Your app will not be named "Fire App Builder," but open the build.gradle file for your app's name.)
4.  Uncomment the code as noted in the Crashlytics code comments. The following code sample shows the code correctly uncommented:

    <pre>
    buildscript {
        repositories {
            mavenCentral()
            jcenter()
            <span class="red">// Uncomment when using CrashlyticsComponent</span>
            maven { url 'https://maven.fabric.io/public' }
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:1.5.0'
            classpath 'me.tatarka:gradle-retrolambda:3.2.3'
        <span class="red">// Uncomment when using CrashlyticsComponent</span>
            classpath 'io.fabric.tools:gradle:1.+'
        //         NOTE: Do not place your application dependencies here; they belong
        //         in the individual module build.gradle files
        }
    }

    allprojects {
        repositories {
            jcenter()
            mavenCentral()
            maven { url 'http://repo.brightcove.com/releases' }
            <span class="red">// Uncomment when using CrashlyticsComponent</span>
            maven { url 'https://maven.fabric.io/public' }
        }
    }
    </pre>

7.  Go to **CrashlyticsComponent > manifests** and open the **AndroidManifest.xml** file.
8.  Insert your Crashlytics key in the `value` property:

    ```xml
    android:name="io.fabric.ApiKey"
    android:value="0c508f569dc758c937d5c0046a17871dcccce2c8"/>
    ```

    {% include note.html content="There's a string named `your_api_key` in **CrashlyticsComponent > res > values > strings.xml**. However, you don't need to do anything with this string. The comment above the code instructs you to insert the Crashlytics key in the AndroidManifest.xml file instead." %}

9.  Expand the **Gradle Scripts** folder and open the **build.gradle (Module: app)** file.
10. Uncomment the line as indicated by the Crashlytics comment:

    <pre>
    <span class="red">// Uncomment when using CrashlyticsComponent</span>
    apply plugin: 'io.fabric'
    </pre>

11. Expand the **Gradle Scripts** folder open the **build.gradle (Module: app)** file.
12. Uncomment the lines as indicated in the Crashlytics comment:

    <pre>
      androidTestCompile 'com.jayway.android.robotium:robotium-solo:5.3.1'

        compile project(':TVUIComponent')
        compile project(':UAMP')
        compile project(':AMZNMediaPlayerComponent')
        compile project(':PassThroughAdsComponent')
        compile project(':FacebookAuthComponent')
        compile project(':AmazonInAppPurchaseComponent')
        compile project(':LoggerAnalyticsComponent')
        <span class="red">// Uncomment when using CrashlyticsComponent</span>
        compile('com.crashlytics.sdk.android:crashlytics:2.6.1@aar') {
            transitive = true;
        }
    </pre>
13.  Build and run your app.

When you build and run your app, Crashlytics will detect that your code is correctly configured. At this point, you will no longer be blocked at the [Crashlytics onboarding screen](https://www.fabric.io/onboard/pending) and will be able to proceed to your Crashlytics dashboard.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_crashlyticsonboardsuccess" type="png" caption="When fabric detects that your Crashlytics configuration is correct (based on you building your app), you see a screen like this that lets you proceed to your Dashboard, where you can view your crash analytics." url="https://www.fabric.io/onboard/pending" %}

## Step 3. Make a Test Crash {#step3}

To test out the Crashlytics configuration, perform a test crash:

1.  In the Android view, go to **TVUIComponent > java > com.amazon.android > tv.tenfoot > ui > fragments** and open the **ContentDetailsFragment.java** file.
2.  Add the following inside the `onStart()` method near the top:

    ```java
    if(true) {
    throw new RuntimeException("Causing fake crash for Crashlytics test");
    }
    ```

3.  Click the **Run 'app'** button  {% include inline_image.html alt="Run 'app' button" file="firetv/fireappbuilder/images/fireappbuilder_runappbutton" type="png" %} to start your app.

When the app starts, it immediately crashes. If you filter your logcat console by the keyword "crashlytics", you'll see the following logs:

```
07-01 13:04:02.021 28688-28688/com.amazon.android.calypso D/CrashlyticsAnalyticsModuleInitReceiver: IAnalyticsModule initialized.
07-01 13:04:02.064 28688-28688/com.amazon.android.calypso I/CrashlyticsCore: Initializing Crashlytics 2.3.8.97
07-01 13:04:04.296 28688-28787/com.amazon.android.calypso I/CrashlyticsCore: Crashlytics report upload complete: 5776CC9E029B-0001-6E05-6C83E6F7D1F2.cls
```

## Step 4. Explore the Crashlytics Dashboard {#step4}

After you have set up the code, run the app, and caused a crash, Crashlytics will let you bypass the [onboarding page](https://www.fabric.io/onboard/pending) and take you to the Dashboard where you can view details about the crash.

1.  Log into [Crashlytics](https://fabric.io/kits/android/crashlytics).
2.  Click **Dashboard** in the upper-right corner.
3.  View the information and settings on the Dashboard.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_crashlytics_dashboard" type="png" caption="Sample crash appearing in the Crashlytics Dashboard" %}

{% include links.html %}
