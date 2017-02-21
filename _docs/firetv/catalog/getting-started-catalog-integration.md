---
title: Getting Started with Catalog Integration
permalink: getting-started-catalog-integration.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

This page provides an end-to-end walkthrough of the process for creating the catalog file required for Fire TV integration, submitting the file to Amazon, and making the required changes to your app for catalog integration.

* TOC
{:toc}

## Before You Begin {#beforeyoubegin}

If you aren't familiar with the basic concepts of Fire TV Catalog Integration, see [Understanding Fire TV Catalog Integration][understanding-fire-tv-catalog-integration].

Before starting the Catalog Integration process, make sure that you have or can easily obtain the following requirements:

*   **Easy access to your app's metadata**: Most apps have someone export their metadata from a database; if you cannot export your metadata from a database, you will need to manually create your catalog file by hand.
*   **An Amazon Web Services (AWS) account**: You or someone in your organization will need an [AWS account](https://developer.amazon.com/public/solutions/devices/fire-tv/docs/catalog/setting-up-your-aws-account-for-fire-tv-catalog-integration) and familiarity with the AWS S3 tools or a willingness to learn about the AWS S3 tools. You will need to execute several AWS commands using the Command Line Interface (CLI) to upload your catalog file to AWS, so that Fire TV can obtain your catalog for integration. Amazon recommends using the AWS SDK to automate this process.
*   **Access to your app's source code**: You will need to make a few small changes to your app's source code to integrate your app and its content with the Fire TV home screen launcher. The person making the changes to your app's source code should have a basic understanding of Android development.
*   **Onboarding process and setup**: It takes two weeks to set up our systems to provide you with a sandbox to test the catalog integration. Your Amazon business contact will also need to onboard you with the integration process. If you do not know who your Amazon business contact is, [contact us](http://www.amazon.com/gp/html-forms-controller/aftsdk-cdf-request).
*  **Minimal fields for matching**: To provide metadata for matching, you must supply some required and additional fields. The [Title][catalog-data-format-schema-reference#Title] field is required. Additional fields (a minimum of 2 but ideally 3 or more) are also required. The additional fields can include the following:
    *  [ReleaseYear][catalog-data-format-schema-reference#ReleaseYear]
    *  [Credits (Actor and Director)][catalog-data-format-schema-reference#Credits]
    *  [RuntimeMinutes][catalog-data-format-schema-reference#RuntimeMinutes]
    *  [SeasonInShow][catalog-data-format-schema-reference#SeasonInShow] or [SeasonID][catalog-data-format-schema-reference#SeasonID] (the SeasonID must reference a valid season)
    *  [ShowTitle][catalog-data-format-schema-reference#ShowTitle] or [ShowID][catalog-data-format-schema-reference#ShowID]

## Integration Process Overview

Catalog Integration has two main components:

*   Catalog creation and upload
*   Integration of your app with the Amazon Fire TV Home Screen Launcher

The catalog creation and upload component is a multi-step process that can be simplified using automation. Consider allocating a developer resource to help script and set up a cron job to automate and simplify this process for your organization. You will need a second developer, preferably someone with Android development experience, to help integrate your app with the Amazon Home Screen Launcher.

Development work on these two processes can take place in parallel. One component is not dependent on the other, although completion of both pieces is required for successful catalog integration.

### Catalog Creation and Upload

The following diagram shows the high-level steps for creating your media catalog file and uploading that file to AWS S3 storage:

{% include image.html file="firetv/catalog/images/catalog_Process" type="png" %}

The catalog creation and upload process is as follows:

1.  Create an XML file in Catalog Data Format (CDF) for your catalog metadata.
2.  Validate your CDF file against the [CDF schema (XSD)][1] file.  
3.  Upload the CDF file to the Amazon Web Services (AWS) S3 catalog bucket that Amazon creates for you.
4.  Review the Amazon-generated ingestion report to confirm that your CDF file was successfully ingested or to help troubleshoot an ingestion failure.

### Integration with the Amazon Fire TV Home Screen Launcher

The following diagram shows the high-level steps for integrating your app with the Amazon Fire TV Home Screen Launcher and testing that integration:

{% include image.html file="firetv/catalog/images/catalog_Launcher_Process" type="png" %}

1.  Modify your app's code to integrate your app with the Amazon Fire TV home screen launcher.
2.  Test your app's integration with the launcher.

## Step 1: Creating Your CDF File

To start, you will need to create a Catalog Definition Format (CDF) file that contains the metadata for your app's content catalog. A CDF file is an XML file that adheres to the format defined by Amazon's CDF schema.

For information about the structure of the catalog file, see [About the Catalog Data Format][about-the-cdf] the [Catalog Data Format Schema Reference][catalog-data-format-schema-reference], or download the [CDF XSD file][1].

You have two options for creating this file:

*   Use an automated process to create the file by exporting your data from a database, then transform that data to a compliant CDF XML file.
*   Manually create the CDF file.

### Creating the CDF file via Exporting and Transforming Your Data

Most apps that integrate with Fire TV will be able to use an automated process to create the CDF file for the app's catalog data.

To use an automated process to create your CDF file:

1.  Export your metadata from your app's database. If necessary, ask a database administrator or support person within your org for help exporting the data. The fields that you export should be roughly equivalent to those defined by the CDF schema (see the [About the CDF][about-the-cdf] and [Catalog Data Format Schema Reference][catalog-data-format-schema-reference].
2.  Consulting the CDF documentation, write an XSLT transform to copy the exported data into an XML file that conforms to the CDF schema. Ask a developer for help, if necessary.

### Manually Creating your CDF File

If your content catalog's metadata is not stored in a database, or if for any reason, you cannot export and transform that data to an XML file, you will need to manually create your CDF file.

To manually create your CDF file:

1.  Familiarize yourself with XML and its concepts, if you are not already comfortable working with XML.
2.  Download the CDF schema XSD file, which you can use as a template for your CDF file: [CDF XSD file download][1].
3.  Consulting the CDF documentation and using an XML editor (many are available for download from various sources), compose the CDF file for your app's catalog data.

## Step 2: Validating Your CDF File

Amazon highly recommends validating your CDF file before uploading the file to AWS. Validating your file locally can help catch errors that would prevent the file from being successfully ingested and integrated with the Fire TV catalog. Validate your CDF file against the schema provided in Amazon's CDF XSD file: [CDF XSD file][1].

Note that XML validation tools can only check that your XML is well-formed (for example, no broken or missing tags) and valid against the CDF schema (for example, no incorrectly nested elements). While XML validation should catch the most common errors in your CDF file, your CDF file could still contain errors that are not caught until the file is uploaded to AWS.

Use an XML validation tool to validate your CDF file. While Amazon does not provide a CDF validation tool, you can easily obtain one at no cost from a variety of sources:

*   If you created or edited your CDF file using an IDE, your IDE might have a built-in XML validation tool.
*   On a Mac or Linux, use the [xmllint][2] utility. Your Mac or Linux computer should come with these utilities already installed as part of the OS.
*   On Windows, a number of free XML validation tools are available for download.

To use xmllint to validate your CDF file:

1.  Download the [CDF XSD file][1] and copy or move this file to the same directory as the CDF file that you previously created.
2.  Open a terminal window (or a command window in Windows) to access your computer's command line interface.
3.  At the shell prompt, type the following command:

    ```bash
    $ xmllint --schema catalog.xsd --noout <cdf_file_name>.xml
    ```

    The `$` represents the shell prompt. Replace `<cdf_file_name>` with the actual name of your CDF file. The `--noout` option suppresses extra output as xmllint traverses your XML file. When finished, xmllint will report any errors that it found in your file or will report that your XML file is valid against the specified schema.

    {% include note.html content="If you copy-and-paste any of the commands from these examples, make sure that the double hyphens (\"`--`\") are not being auto-corrected to n-dashes by your browser or terminal editor." %}

## Step 3: Uploading Your CDF File to AWS S3

Once you have verified that your CDF file is valid, you can upload the CDF file to the S3 bucket that Amazon set up for your catalog using the following command:

```bash
$ aws s3api put-object --body <catalog_file_name.xml> --bucket <s3_bucket_name> --key catalogs/catalog.xml --acl bucket-owner-full-control
```

{% include note.html content="Make sure that if you copy-and-paste this command into a terminal window that the \"`--`\" characters paste as double-dashes and not as an n-dash. Replace the text in angle brackets (`< >`) with the actual values for your file name and bucket." %}

For detailed instructions, see [Uploading Your Catalog to Amazon][uploading-your-catalog].

## Step 4: Verifying the Uploaded CDF File

Amazon posts a report to your catalog bucket indicating success or failure for the import of your catalog data each time that you upload a new CDF file. To learn more about this report and how to use it to troubleshoot CDF upload failures, see [Receiving and Understanding the Catalog Ingestion Report][receiving-and-understanding-the-catalog-ingestion-report].

## Step 5: Integrating Your App with the Fire TV Home Screen Launcher

Once you have a valid catalog that has been ingested by Amazon, you will need to make a few changes to your app's source code to integrate your app with the Fire TV home screen launcher. The home screen launcher handles the following types of capabilities:

*   User sign-in
*   Universal browse and search
*   Playback of content

For more detailed steps and examples for launcher integration, see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration].

To integrate your app with the Fire TV home screen launcher:

1.  In your app's source code, set up an Intent that contains the user's subscription status and other information about your app:
    1.  In your app's source code, construct a standard Android Intent that your app can use to broadcast your its capabilities.
    2.  Make sure that the app broadcasts the Intent any time that the app starts up, when the Intent is requested by the Fire TV launcher, or if your users' subscription status changes.
    3.  Send the Intent using your app's context.
2.  In your code, add a class to receive capability requests from the Fire TV launcher.
3.  Implement an Activity in your app to receive and handle playback and sign-in Intents from the Fire TV launcher.
4.  Add the following elements to your app's Android manifest:
    *   An `<intent-filter>` element for playback and sign-in activities to recieve intents from the Fire TV launcher.
    *   A `<receiver>` element with an android:name attribute that specifies the name of your `BroadcastReceiver` class and containing a child element of an `<intent-filter>` for the `com.amazon.device.REQUEST_CAPABILITIES` action.
    *   A `<uses-permissions>` element to ensure that the Fire TV launcher accepts your Intents.

## Step 6: Testing Your App's Integration with the Fire TV Home Screen Launcher

Use the test cases described in [Test Cases for Verifying Fire TV Deep Links][test-cases-for-verifying-deep-links-from-your-fire-tv-catalog] to formulate a test plan for your integrated app. Execute these step-by-step procedures to help verify your launcher integration.

You have two options to test your app's integration with Fire TV. Use these options to verify that you have made the correct changes to your app for Catalog Integration with Fire TV to function as expected:

*   [Testing Launcher Integration with ADB][testing-launcher-integration-with-adb]: Use the Android Debug Bridge (ADB) utility to sideload your app onto a Fire TV device, and verify that your app responds correctly to sign-in and playback intents. Use this option when your app is fully developed and you have implemented the code to respond to launcher Intents for playback (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]. You can also use this option to verify content playback from your app.
*   [Testing Fire TV Launcher Integration with the Integration Test App][testing-launcher-integration-with-the-test-app]: Download and use the Test App provided by Amazon to test your app and verify that your app responds correctly to sign-in and playback intents. Use this option when you have not yet completed development on your app or have not yet implemented the code to respond to Intents from the launcher (see [Integrating Your App with the Fire TV Home Screen Launcher][launcher-integration]).

[1]: https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd
[2]: http://xmlsoft.org/xmllint.html

{% include links.html %}
