---
title: Setting Up Your AWS Account for Fire TV Catalog Integration
permalink: setting-up-your-aws-account-for-fire-tv-catalog-integration.html
sidebar: catalog
product: Fire TV Catalog
toc: false
github: true
---

Fire TV Catalog Integration uses Amazon Web Services (AWS) tools for uploading your catalog and managing the users who have access to the tool used to store your catalog. Before uploading your catalog file, you will need to set up your AWS account and exchange account information with your Amazon Fire TV representative.

* TOC
{:toc}

## Procedure Overview

Setting up your AWS account is a one-time process that you will need to complete before you can start uploading catalog files to Amazon. To set up and test your AWS account:

1.  Sign in or sign up for an AWS account.
2.  Send your 12-digit AWS Account ID (this is different than an IAM User ID) and your Canonical User ID to your Amazon Fire TV representative.
3.  Obtain the S3 bucket information for your app from your Amazon Fire TV representative.
4.  Grant permission to an Identity and Access Management (IAM) AWS user to upload files to your Fire TV S3 catalog bucket.
5.  Test your IAM user policy.
6.  Set up the AWS CLI (if necessary).
7.  Test the catalog upload using a sample CDF file with with the AWS Command Line Interface (CLI) to verify that you can upload catalog files.

## Step 1: Sign In or Sign Up for an AWS Account (New AWS Users)

To upload your catalog file to our service on Amazon S3, you must have an Amazon Web Services (AWS) account. If you have an AWS account, sign in to that account. If you do not already have an account with AWS, you will need to create one:

To create an AWS Account:

1.  Go to the AWS home page: [http://aws.amazon.com/](http://aws.amazon.com/)
2.  Click the **Sign in to the Console** button, which takes you to the login screen.
3.  Type your email address or mobile telephone number.
4.  Choose the **I am a new user option**.
5.  Click the **Sign in using our secure server** button.
6.  Follow the prompts to create your account.

## Step 2: Send Your 12-Digit AWS Account ID and Your Canonical User ID to Your Amazon Representative

For the Fire TV service to grant access to the S3 catalog bucket that Amazon creates for you, send the 12-digit Account ID of your AWS account to your Amazon Fire TV representative using a secure method.

Note that your AWS Account ID is different from an Access Key ID or Secret Access Key, which are specific to IAM users.

To find your AWS Account ID:

*   For account owners (root account users), the AWS account ID is on the [Security Credentials page](https://console.aws.amazon.com/iam/home#security_credential), under the **Account Identifiers** tab.

    {% include image.html file="firetv/catalog/images/catalog_AccountID_root" type="png" %}

*   For IAM or federated users, the AWS account ID is shown in the [Support Center](https://console.aws.amazon.com/support/home#/support/home), in the upper right corner. (If you have not yet set up your IAM users, the next section describes how to do so. Note that you will need the name of the S3 bucket from your Amazon Fire TV representative to set up the security policy for your IAM users.)

    {% include image.html file="firetv/catalog/images/catalog_AccountID" type="png" %}

Additionally, make sure to send your Canonical User ID to your Amazon Fire TV representative. This ID is located just below the AWS account ID for account owners:

{% include image.html file="firetv/catalog/images/catalog_Canonical_ID" type="png" %}

## Step 3: Obtain the S3 bucket information for your app from your Amazon Fire TV representative

Once you send your AWS Account ID to your Amazon Fire TV representative, your representative will send you an S3 bucket name. This catalog bucket is unique to your account and may only be accessed by your account. No other Catalog Integration partner can see or access your S3 catalog bucket.

## Step 4: Grant permission to an Identity and Access Management (IAM) AWS user to upload files to your Fire TV S3 catalog bucket

Configure an Identity and Access Management (IAM) AWS user with at least the "S3 PutObject" permission to all of S3 or at least your Fire TV S3 catalog bucket. This permission will allow that user to upload files to your S3 bucket.

Although you can use your AWS root credentials to access Amazon S3 and your catalog bucket, the AWS best practice is to create separate Identity and Access Management (IAM) users and use those credentials to interact with AWS. For example, you can create an IAM user that has full administrator access to S3 but to no other AWS services or create a limited-access IAM user who can only upload catalog data but not access any other part of the account. Set up an IAM user for your AWS account for each person in your organization who will need access to your S3 bucket. Having separate IAM users for different members of your organization will allow you to enable only the necessary permissions for each user.

### Creating a new IAM User

To create a new IAM user:

1.  Go to the main AWS console: [https://console.aws.amazon.com/](https://console.aws.amazon.com/console/home?region=us-east-1).
2.  Under **Security & Identity**, click **Identity and Access Management** to display the Welcome to Identity and Access Management page.
3.  Under **Security Status**, click **Create individual IAM users** to expand that menu item, then click **Manage Users** to view the list of IAM users associated with your AWS account.
4.  Click the **Create New Users** button to create your new users:
    1.  Under **Enter User Names**, type your choice of user names for up to five new users.
    2.  Check the **Generate an access key for each user** box.
    3.  Click **Create**.
5.  As prompted, download or otherwise save the security credentials for the new user.

The security credentials contain two access keys: an Access Key ID and a Secret Access Key. The secret access key is only available when you create an IAM user, although you can generate a new one at any time. Find the access key ID on the [IAM Users page](https://console.aws.amazon.com/iam/home#users) under the Security Credentials tab.

Any user who is going to access the Catalog Integration S3 bucket will need the access keys for that IAM user.

### Giving IAM Users Access to Your Amazon S3 Bucket

If you have already set up IAM users, you will need to give them access to your S3 bucket. Before you start this procedure, you will need the name of your S3 bucket as provided by your Amazon Fire TV representative.

To give existing IAM users access to your Amazon S3 bucket:

1.  Go to the main AWS console:  [https://console.aws.amazon.com/](https://console.aws.amazon.com/console/home?region=us-east-1).
2.  Under **Security & Identity**, click **Identity and Access Management** to display the Welcome to Identity and Access Management page.
3.  On the left pane, click **Users** to view the list of IAM users associated with your account.
4.  From the list of users, select the user who you would like to grant access to your S3 bucket to.
5.  On the user's details page, click the **Permissions** tab to set up the security policy for the user. Two different setups are commonly used for permissions:
    *   **Option 1**: If you are comfortable with the user having full access to all S3 functionality for the account, apply the "AmazonS3FullAccess" policy:
        1.  On the **Permissions** tab, under **Managed Policies**, click **Attach Policy** to go to the Attach Policy screen.
        2.  In the Filter box, type "AmazonS3FullAccess" to show the AmazonS3FullAccess policy.
        3.  Check the box next to the AmazonS3FullAccess policy and click **Attach Policy**.
    *   **Option 2**: Set up a new custom policy for your bucket, which restricts that user access to only your catalog bucket:
        1.  On the **Permissions** tab, go to **Inline Policies**, and click the link to create a new inline policy, which will take you to a Set Permissions page.
        2.  On the Set Permissions page, select **Custom Policy**, and click **Select** to go to the Review Policy page.
        3.  On the **Review Policy** page, enter the following information:
            *   **Policy Name**: Type a unique name for the policy. This name can be anything that you choose.
            *   **Policy Document**: Copy and paste the following line into the Policy Document field. Substitute the actual name of your S3 bucket for the &lt;Bucket_Name&gt; placeholder.

            ```
            {"Statement":[{"Effect":"Allow","Action":"s3:*","Resource":"arn:aws:s3:::*<Bucket_Name>"}]}
            ```
        4.  Click **Validate Policy**.
You will see a status message either telling you that the policy is valid or an error message with instructions to fix the policy.
        5.  Click **Apply Policy** to apply the policy to the user.

## Step 5: Test Your IAM User Policy

To make sure that you have correctly set up your IAM users and their security policies, you can use AWS's Policy Simulator tool. To verify a policy using the Policy Simulator:

1.  In your web browser, navigate to the Policy Simulator: [https://policysim.aws.amazon.com/home/index.jsp?#](https://policysim.aws.amazon.com/home/index.jsp)
2.  On the left pane, select the user whose policy you are verifying.
3.  On the right pane, select **S3** from the **Select Service** drop-down list.
4.  From the **Select action** drop-down list, select **PutObject**.
5.  Click the toggle arrow just to the left of the **Amazon S3** entry in the **Service** column to expand the simulation settings.
6.  Copy and paste "arn:aws:s3:::<Bucket_Name>/catalogs/*" into the **ARN** field, replacing the <Bucket_Name> placeholder with the name of your S3 catalog bucket.
7.  Click **Run Simulation** to run the simulation.

If the policy is valid, the simulator will display an "allowed" message; otherwise, it will display "denied" with an explanation of the issue.

## Step 6: Set up the AWS Command Line Interface (CLI) (if necessary)

To interact with Amazon S3 and your catalog bucket, you can use any available tools for interacting with Amazon S3, including the following:

*   The AWS Command Line Interface (CLI). See [Getting Set Up with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) for information.
*   One of the [AWS SDK](https://aws.amazon.com/tools/)s to implement catalog upload with the programming language of your choice. For example, the [AWS SDK for Java](http://aws.amazon.com/sdk-for-java/) provides an [S3 Client](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html), with [sample code](http://docs.aws.amazon.com/AmazonS3/latest/dev/UploadObjSingleOpJava.html). See [Start Developing with Amazon Web Services](http://aws.amazon.com/developers/getting-started/?nc1=h_d_gs) for more information on the AWS developer tools.

Any S3 access tool you use, including your own implementation, must be initially configured with the Access Key ID and Secret Access Key for your IAM user. For example, to configure the AWS CLI, use the `aws configure` command. The following example shows both the `aws configure` command syntax and the prompts that follow as you configure the CLI:

```bash
$ aws configure
AWS Access Key ID [None]: AAAAAAAAAAAAAEXAMPLE
AWS Secret Access Key [None]: aAaaaAAaaAAA/A1AAAAA/aAaAaaAAEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

See [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for information on configuring the AWS CLI.

{% include links.html %}
