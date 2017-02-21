---
title: Setting Up Your Amazon Fling Development Environment for Android
sidebar: fling
product: Fling SDK
permalink: setting-up-your-amazon-fling-development-environment-for-android.html
reviewers: jeffersd
github: true
---

To enable this functionality in your app, you have to integrate our SDK in your development environment. This document describes how to set up your Android Studio projects to include the SDK. Before you start, install the following software packages on your development computer:

*  [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
*  [Android Studio](http://developer.android.com/sdk/index.html)

{% include note.html content="The Android SDK, Android Studio, the Java Development Kit, and certain other development tools are provided by third parties, not by Amazon. Our links for these tools will take you to third-party sites for download and installation of the tools." %}

## Setting up Android Studio

To use our SDK with your Android Studio project, add the JAR file included in the SDK package as a file dependency. Use these steps to set up your Android Studio environment. If you are developing an app for an Android platform that does not run Fire OS, there are additional libraries to add as well.  

### Adding the Amazon Fling Libraries  

1.  In Android Studio, create a new project or open an existing one.  
2.  Copy the **AmazonFling.jar** file from the Amazon Fling library package into the **libs** folder for your project.  

    {% include image.html file="firetv/fling/images/fling_studio_1" type="png" border="true" %}

3.  Right-click on your project's root folder and select **Open Module Settings** to open the project folder.  
4.  From the **Module** tab in Project Structure, select the **Dependencies** tab.
5.  Click the + button and select the **File dependency** option. Then, select AmazonFling.jar in the project **libs** folder.

    {% include image.html file="firetv/fling/images/fling_studio_2" type="png"  border="true" %}

6.  Under the **Dependencies** tab check the library scope and make sure it is set as **Compile**.

    {% include image.html file="firetv/fling/images/fling_studio_3" type="png"  border="true"%}

7.  Click **OK**.

### Adding Additional Libraries (Generic Android Only)

If you are developing an app that runs on generic Android devices (not Fire OS devices), use these steps to add additional libraries to your project:

1.  Copy the **WhisperPlay.jar** file in the **/lib/Android/** folder in the Amazon Fling SDK package to the **libs** folder in your project.  

2.  Right-click on your project's root folder and choose **Open Module Settings** to open the project folder.
3.  From the **Module** tab in Project Structure, select the **Dependencies** tab.
4.  Click the + button and select the **File dependency** option. Then, select the Whisperplay.jar file in the project **libs** folder.
5.  Under the **Dependencies** tab check the library scope and make sure it is set as **Compile**.  

    {% include image.html file="firetv/fling/images/fling_studio_4" type="png"  border="true" %}

6.  Click **OK**.

## Next Steps

To integrate Amazon Fling into your app, see [Integrating Amazon Fling into your Android App][integrating-amazon-fling-into-your-android-app].

If your Android app supports the Google Cast API and you want to use Fling as well, see [Integrating Amazon Fling with an Existing Android Cast App][integrating-amazon-fling-with-an-existing-android-cast-app].

If your app uses Android [MediaRouter](https://developer.android.com/guide/topics/media/mediarouter.html), see [Using Amazon Fling with Android MediaRouter][using-amazon-fling-with-android-mediarouter].

{% include links.html %}
