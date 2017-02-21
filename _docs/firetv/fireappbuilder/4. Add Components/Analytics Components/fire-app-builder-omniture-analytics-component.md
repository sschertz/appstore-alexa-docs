---
title:  Omniture Analytics Component
permalink: fire-app-builder-omniture-analytics-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

Omniture gives you a JAR file to integrate into your app (instead of relying on API keys). The JAR file stores your security keys and other configuration information.

{% include note.html content="These instructions assume you have the **Project** view selected in Android Studio." %}

**To configure the Omniture Analytics Component:**

1.  Load the Omniture Analytics component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component] for details about how to load a component into your app.
2. Remove any other analytics components that are loaded in your app (such as FlurryAnalyticsComponent, CrashlyticsComponent, or LoggerAnalyticsComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.    

    {% include_relative componentnote_analytics.html %}
    
2.  Inside your app's folder, create a folder called **AdobeMobileLibrary**.
3.  Inside this new **AdobeMobileLibrary** folder, put the JAR file that Adobe gives you.

    For example, if the JAR file were named adobeMobileLibrary-4.6.1.jar, the path to the file would look like this:  (your app) > AdobeMobileLibrary > adobeMobileLibrary-4.6.1.jar.

4.  In the **AdobeMobileLibrary** folder, open the **build.gradle** file  add a reference to your JAR file:

    ```
    configurations.create("default")
    artifacts.add("default", file('adobeMobileLibrary-4.6.1.jar'))
    ```

5.  Inside your app's **settings.gradle** file, include the AdobeMobileLibrary component:

    ```
    include  ':app',
             ':AdobeMobileLibrary',
    ```
6.  In the **OmnitureAnalyticsComponent** folder, open the **build.gradle** file and make sure there's a dependency for the **AdobeMobileLibrary**:

    ```xml
    dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile project(':ModuleInterface')
    compile project(':AnalyticsInterface')
    compile project(':AdobeMobileLibrary')
    }
    ```

    Your app will now start tracking activities using Omniture Analytics.

{% include links.html %}
