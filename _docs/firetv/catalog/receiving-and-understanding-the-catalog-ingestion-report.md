---
title: Receiving and Understanding the Fire TV Catalog Ingestion Report
permalink: receiving-and-understanding-the-catalog-ingestion-report.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

After you've [uploaded your catalog data][uploading-your-catalog] to the Amazon Simple Storage Service (S3) bucket for integration into the Amazon Fire TV universal browse and search, the catalog integration system generates an _ingestion report_ within four hours to tell you whether the catalog file could be used. The report gives an overall success or failure status, identifies any problems found in the file, and provides details about each problem.

{% include note.html content="The ingestion report system is a beta release. Opting in to receive ingestion status notification emails is strictly voluntary at this time. Until you feel comfortable making any necessary changes or fixes to your catalog file, your Amazon Business Developer Manager will notify you if there are problems and will help you understand and resolve them." %}

* TOC
{:toc}

## Retrieving Your Ingestion Report

The ingestion report, named _report.html_, is an HTML file placed in the same S3 bucket as your uploaded catalog file. You can opt to receive an email telling you that the new report is ready, providing a basic summary of its contents and giving you a command with which you can download the full report. 

### How to Receive Ingestion Status Notification Emails

To receive your ingestion report summary emails, send a email to [p11-catalog-subscriptions@amazon.com](mailto:p11-catalog-subscriptions@amazon.com) asking to be added to the distribution list for your catalog. Amazon will verify your request and add you to the list.

Success and failure emails are sent separately, so if you want to receive both, ask to be included on both lists. A report is generated once per upload or when the ingestion status changes.

### How to Use the Information in the Email

The email that you receive provides the ingestion's success or failure status. If the ingestion fails, the errors are listed in the full report (rather than in the email). The email also contains an Amazon Web Services (AWS) command to retrieve the full report.

To use that AWS command, you must first install the [Amazon Web Services Command Line Interface (CLI)](https://aws.amazon.com/cli/ "Amazon Web Services Command Line Interface") tool, after which you can use AWS commands in a normal command or terminal window. This ensures that your proprietary catalog information remains protected, because only someone with access rights to your S3 bucket can retrieve the report using that command.

Here's an example of the AWS command to retrieve the report. Copy the line from the email and paste it into your command or terminal window.

```bash
aws s3api get-object --bucket cdf-test --key reports/report.html --version-id FciuqMvVh2oWFv726L6Ytf8ECLbO6Kj0 report.html
```

When run successfully, this command downloads the report to your current folder. The downloaded file has the name \_report.html_. (If you're curious about what that command is doing, see the AWS CLI Command Reference [get-object](http://docs.aws.amazon.com/cli/latest/reference/s3api/get-object.html) page.)

In addition to the AWS CLI command, the email will also contain a link to access the report directly. This link is valid for 7 days. Note that anyone with this link can access the full report, so be aware of this when forwarding the email.

## What's in the Ingestion Report?

An ingestion report has three sections:

*   The summary, including the success or failure status
*   Errors, warnings, and suggestions
*   Counts of catalog entries added, removed, updated, or unchanged.

### The Summary

Here is an example of the summary section of an ingestion report:

{% include image.html caption="Example ingestion report summary section" file="catalog/catalog_IGSum" type="jpg" %}

The summary includes a unique ID given to each ingestion attempt, the catalog that was used, when the ingestion attempt was made, and the result. There are two possible results:

*   **Success**: No errors were found in the catalog, although there still may be actionable warnings and suggestions. The catalog information was integrated into Fire TV's universal browse and search.

    {% include note.html content="You should still look at the [Counts](#counts) section to ensure that the results are as you expected. For instance, you could accidentally delete a big portion of your catalog and still have a successful ingestion as long as there are no errors in what remains. Only by reading beyond the <i>Success</i> glyph to notice that the number of entries removed was unexpectedly high would you suspect that something wasn't right." %}

*   **Failure**: Errors prevented the catalog from being used. Your last successfully ingested catalog remains the active catalog. The [Errors, Warnings, and Suggestions](#ews) section of the report will give you specific information about what went wrong.

The ingestion in the example above failed due to nine errors. Many warnings and suggestions were also generated, but messages in these categories generally do not trigger a failure (image-related warnings and suggestions are the exception). Counts for added, removed, and updated are all 0 because the ingestion failed and the uploaded catalog was not used. The unchanged count of 0 indicates that there is no previous catalog to change.

Clicking on any of those boxes takes you to that section of the report.

### Errors, Warnings, and Suggestions

This portion of the report is where you'll find the information that you'll need to fix any problems. Clicking the Details button for a section expands that section to list its individual messages. At the end of each message is the number of entries that generated it. Clicking on the plus sign next to each message displays the IDs of the works that generated the message, and, in some cases, an additional message with more details. Here is an example with two expanded errors:

{% include image.html caption="Expanded errors showing details" file="catalog/catalog_IGFullyExpandedError.jpg" %}

Issues found in the Errors section cause an ingestion to fail; warnings and suggestions generally do not. Therefore, a successful ingestion report can still contain plenty of warnings and suggestions that you should examine.

### Counts

The last section in the report provides specifics on the changes to your catalog due to the ingestion. Clicking the Details button in each category again shows the IDs of the works in each. If you have a list of expected changes, you can compare it against these lists as a final check.

If the ingestion failed, then the uploaded catalog was not used so the Added, Removed, and Updated values will be 0\. The Unchanged value shows the number of catalog entries in your last successfully ingested catalog.

If the ingestion was successful, the Added, Removed, Updated, and Unchanged numbers should reflect your expected results. As mentioned above, the counts in this section can indicate that there's a problem with your catalog even when the ingestion is successful by all other measures.

## Acting on the Ingestion Report

What you do next depends on the ingestion status.

*   **Success**: As long as the counts of pages added, removed, updated, and unchanged are as you expected, there is no further action required. However, consider addressing any warnings and suggestions. See [catalog-data-format-ingestion-report-messages] for an explanation and course of action for each.
*   **Failure**: Refer to [catalog-data-format-ingestion-report-messages] for an explanation and course of action for each entry in the Error section. If you still have trouble understanding what needs to be done or how to do it, please contact your Amazon Business Developer Manager.

{% include links.html %}
