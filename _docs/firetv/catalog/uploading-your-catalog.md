---
title: Uploading Your Catalog to Amazon
permalink: uploading-your-catalog.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

This page describes how to validate and upload your Catalog Data Format (CDF) file to Amazon's AWS S3 bucket for catalog ingestion. Amazon strongly recommends validating your CDF file before attempting to upload; please include a validation step as part of your upload process.

* TOC
{:toc}

## Prerequisites

Before uploading your catalog to Amazon, you will need to set up your AWS account, and give your 12-digit AWS Account ID to your Amazon Fire TV representative. Your Fire TV representative will create your AWS S3 bucket for catalog ingestion and provide you with access to your bucket. Because Amazon creates the S3 bucket, the bucket will not appear in your AWS S3 console. However, you should be able to access the bucket via the Command Line Interface (CLI) to view the bucket contents and upload files to the bucket. The commands used to interact with your S3 bucket are given in the next sections of this page.

See [Setting Up Your AWS Account for Fire TV Catalog Integration][setting-up-your-aws-account-for-fire-tv-catalog-integration] for details.

## Validating Your CDF File

Amazon highly recommends validating your CDF file before uploading the file to AWS. Validating your file locally can help catch errors that would prevent the file from being successfully ingested and integrated with the Fire TV catalog. Validate your CDF file against the schema provided in Amazon's CDF XSD file: [CDF XSD file](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd).

Note that XML validation tools can only check that your XML is well-formed (for example, no broken or missing tags) and valid against the CDF schema (for example, no incorrectly nested elements). While XML validation should catch the most common errors in your CDF file, your CDF file could still contain errors that are not caught until the file is uploaded to AWS.

Use an XML validation tool to validate your CDF file. While Amazon does not provide an XML validation tool, you can easily obtain one at no cost from a variety of sources:

*   If you created or edited your CDF file using an IDE, your IDE might have a built-in XML validation tool.
*   On a Mac or Linux, use the [xmllint](http://xmlsoft.org/xmllint.html) utility. Your Mac or Linux computer should come with these utilities already installed as part of the OS.
*   On Windows, a number of free XML validation tools are available for download.

To use xmllint to validate your CDF file:

1.  Download the [CDF XSD file](https://s3.amazonaws.com/com.amazon.aftb.cdf/catalog.xsd) and copy or move this file to the same directory as the CDF file that you previously created.
2.  Open a terminal window (or a command window in Windows) to access your computer's command line interface.
3.  At the shell prompt, type the following command:

    ```
    $ xmllint --schema catalog.xsd --noout <cdf_file_name>.xml
    ```

    The `$` represents the shell prompt. Replace `<cdf_file_name>` with the actual name of your CDF file. The `--noout` option suppresses extra output as xmllint traverses your XML file. When finished, xmllint will report any errors that it found in your file or will report that your XML file is valid against the specified schema.

## Uploading Your CDF File

Your catalogs are contained in a `catalogs` folder inside the S3 bucket for your catalog bucket. Use any S3 tool to upload your catalog file to this bucket and folder.

To upload a CDF file to an S3 bucket:

1.  Type the following command, substituing your CDF file name for &lt;catalog_file_name.xml&gt; and S3 bucket name for &lt;s3_bucket_name&gt;. Note that the `--acl bucket-owner-full-control` option is required so that Amazon will be able to read the file that you upload upload, and thus ingest your catalog.

    ```
    $ aws s3api put-object --body <catalog_file_name.xml> --bucket <s3_bucket_name> --key catalogs/catalog.xml --region us-east-1 --acl bucket-owner-full-control
    ```

    {% include note.html content="Make sure that if you copy-and-paste this command into a terminal window that the `--` characters paste as double-dashes and not as an n-dash." %}

    This command successfully uploads files up to 2GB in size. If successful, the AWS CLI will display a set of tags for the VersionID, ETag, and Expiration for the file.

2.  Verify that you see your catalog file in your S3 bucket. Type the following command to list all files in your S3 bucket:

    ```
    $ aws s3 ls s3://<s3_bucket_name>/catalogs/ --region us-east-1
    ```

    You may upload multiple catalogs to the catalog bucket, but Amazon only uses the most recently uploaded catalog, regardless of its name. If you find a problem with your catalog, or need to update the data in the CDF file, just upload a new file.

### Example Workflow

The following example command uploads a catalog file named `my-catalog.xml` to an S3 bucket named `cdf-bucket`:

```
$ aws s3api put-object --body my-catalog.xml --bucket cdf-bucket --key catalogs/catalog.xml --region us-east-1 --acl bucket-owner-full-control
```

This command returns the following sample output:

```
{
    "VersionId": "m_QwgKPy9RJZsWperU_LEULD1waJE2He",
    "ETag": "\"e8c38d5258ad1f3b241ae2ce347e40bc\"",
    "Expiration": "expiry-date=\"Fri, 06 Jan 2017 00:00:00 GMT\", rule-id=\"Rule for the Entire Bucket\""
}
```

To verify that the my-catalog.xml file was successfully uploaded, use the following command:

```
$ aws s3 ls s3://cdf-bucket/catalogs/ --region us-east-1
```

This command returns a list of all catalog files currently found in the bucket:

```
2015-12-07 15:02:17      10236 my-catalog.xml
2015-12-01 15:10:28        166 other-catalog.xml
```

## Additional Resources for AWS

These links provide further information on AWS and Amazon S3.

*   [Amazon Web Services](http://aws.amazon.com/ "Amazon Web Services"): All AWS products
*   [Amazon Simple Storage Service (S3)](http://aws.amazon.com/s3/): Secure object storage
*   [Amazon Identity and Access Management (IAM)](http://aws.amazon.com/iam/): Define users and roles for your AWS account
*   [Start Developing with AWS](http://aws.amazon.com/developers/getting-started/): Documentation, SDKs, and sample apps for various languages and platforms
*   [AWS Command Line Interface (CLI)](http://aws.amazon.com/cli/): Simple access to AWS services
*   [Tools for Amazon Web Services](http://aws.amazon.com/tools/): All SDKs and sample code
*   [AWS SDK For Java](http://aws.amazon.com/sdk-for-java/): Integrate AWS with your Java apps
*   [Amazon S3 Client](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html): Java class for accessing Amazon S3
*   [Upload an Object Using the AWS SDK for Java](http://docs.aws.amazon.com/AmazonS3/latest/dev/UploadObjSingleOpJava.html): Sample code for uploading your data to Amazon S3

{% include links.html %}
