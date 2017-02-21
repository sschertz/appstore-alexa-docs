---
title: Create a New Fire App Builder Component
permalink: fire-app-builder-create-a-new-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

In addition to using the existing components, you can create your own components that implement the interfaces. When you create your own components, you will need to write the Java code that implements the interface.

**To create your own component:**

1.  Identify the interface you want to implement. See the [Components Overview][fire-app-builder-interfaces-and-components] for a list of the various components and the interfaces each component implements.
    
    For example, if you're implementing an analytics component, the Components Overview notes that each analytics component implements the `IAnalytics` interface. Ads components implement the `IAds` interface, and so on.
    
2.  Open the interface folder and examine the contents of the interface (or press **Shift+Shift** and type the interface name to search directly for it).
    
    Continuing with the same example, the `IAnalytics.java` file is located in **AnalyticsInterface > java > com.amazon.analytics > IAnalytics**.
    
3.  Create a new component that implements each of the fields and methods of the interface. The Javadoc for each interface is available as a ZIP file download. 
    
    If you look at the Interface IAnalytics Javadoc, you will see that the following fields are required:
    
    *  `major`
    *  `minor`
    
    And the following methods are required:
    
    *  `collectLifeCycleData`
    *  `configure`
    *  `trackAction`
    *  `trackCaughtError`
    *  `trackState`

    {% include tip.html content="For an example, look at how other components have implemented the interfaces." %}
    
4.  In your component's AndroidManifest.xml file, follow the pattern of other Analytics components. The manifest file should contain at least the following elements (customized to your component name):

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
              package="com.amazon.analytics.yourcomponentname">
    
        <application
                android:allowBackup="true"
                android:label="@string/app_name"
                android:supportsRtl="true">
    
            <receiver android:name="com.amazon.analytics.yourcomponentname.YourComponentNameModuleInitReceiver"
                      android:enabled="true"
                      android:exported="false">
                <intent-filter>
                    <action android:name="android.amazon.module.init"/>
                </intent-filter>
            </receiver>
        </application>
    
    </manifest>
    ```
    
5.  In your component's build.gradle file, again follow the example of the existing components. Your component's build.gradle file must include the dependencies for at least the following:

    ```js
    dependencies {
        compile fileTree(include: ['*.jar'], dir: 'libs')
    
        compile project(':ModuleInterface')
        compile project(':AuthInterface')
        compile project(':Utils')

    }
    ```
6.  After you finish creating the component, you can load the component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component]. 

{% include links.html %}
