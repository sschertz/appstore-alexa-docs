---
title: smapi - Command Line Interface
sidebar: smapi
product: Skill Management API
permalink: smapis-cli
---

The ASK Command Line Interface (ASK CLI) is a tool for you to manage your Alexa skills and related AWS lambda functions.

## Prerequisites:

- [npm](https://www.npmjs.com/package/npm) package manager. To check if you have npm installed, at a command prompt type the following:

    `npm --version` 

    If installed you should get a response similar to the following:

    `2.15.1`

    If you don't have npm installed, the easiest way it is to install node.js (https://nodejs.org/en/download/).

- Credentials for an AWS account. To get these, [install the AWS CLI tool](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) and set up credentials to access the AWS account that contains the Lambda function code for your Alexa Skills.
Your credentials are stored in a directory named .aws in your home directory (UserProfile in Windows, or $Home in Unix).
For more information see, [Configuring the AWS Command Line Interface-Configuration and Credential Files](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files).


## Installation

To install the Alexa Skill Management CLI:

1. Clone the repository on GitHub (location here). 
2. Navigate to the installation directory (ASKCli by default) and run the following command:

`npm install -g`

## Commands 

A call to the ASK CLI tool uses the following format:

`$ ask <command> <subcommand> [options and parameters]`

The ASK CLI command parameter can be one of the following:

- [`init`](#init-command) - Initializes the ASK CLI with a specified developer account.
 - [`api`](#api-command) - Provides options to manage Alexa skills.
 - `clone [options]`(#clone) - Clones a Alexa skill project. Use this command when updating an existing skill.
- [`deploy`](#depoy) -  Deploys a skill to the developer portal.
- [`lambda`](#lambda) - Provides access to the AWS Lambda code associated with an Alexa Skill.
- `new [options]` Creates an Alexa skill project with the specified options.
- `help [cmd]`  Displays help for the specified command. If no command specified, displays the commands and options.

  Options:

- `-h, --help` - Provides the commands and options available.
- `-V, --version`- Outputs the version number.


### init command 

To manage skills associated with your Amazon developer account, you must run the init command.
`init` call format:

`$ ask init`

A browser window will open and with a sign on screen. You should sign in with your Amazon developer account credentials. When you have successfully signed in, you will be redirected to the command prompt.

When you return to the command prompt window, you will be promted to choose a vendor ID. Enter the number associated with the vendor ID.

```
$ Choose the vendor ID for the skills you want to manage
1) Amazon: M123456789
2) Woot: M987654321
```

If you manage Alexa skills for organizations, you have multiple vendor IDs associated with your developer account. To check your vendor ID for a particular organization, sign in to your developer account [developer.amazon.com](https://developer.amazon.com). Choose the organization you want to manage skills for in the dropdown in the upper right. Go to [https://developer.amazon.com/mycid.html](https://developer.amazon.com/mycid.html) to view the vendor ID for that organization.

{% include note.html content="You can change the organization whose skills you are managing by running the `$ ask init` command again and selecting a different vendor ID" %}

You should get a message that says Profile Vendor ID updated, which means you've succesfully initialized the tool.


### lambda command {#lambda}

The Lambda command enables you to retrieve and post code for an AWS Lambda function.

`lambda` call format::

``$ ask lambda <subcommand> [-f|--function <functionName>] [-d|--dest <destPath>]``

**Subcommands**

- [download](#download-subcommand)
- [upload](#upload-subcommand)

#### download subcommand

Downloads code for the specified lambda function to an optional specified destination.
`download` call format:

`$ ask lambda download [-f|--function <functionName>] [-d|--dest <destPath>]`

**Parameters**
<dl>
<dt>--function, -f</dt>

<dd>Optional. The Lambda function's name. If this option is not set, the download operation will display a list of Lambda functions for the configured account, and you can choose one from a numbered list.</dd>
</dl>

--dest, -d

:   Optional. Specifies the download destination for the Lambda function. If not set the Lambda is downloaded to the current working directory. 

#### upload subcommand 

Uploads files from the current directory or specified directory to a specified Lambda function. 
`upload` call format:

`$ ask lambda upload <-f|--function <functionName>> [-s|--src <sourcePath>]`

**Parameters**

--function, -f

:   Required. Specifies the target Lambda function name.
  

--src, -s

:   Optional. Specifies the source directory to upload from. If not specified, the contents of the current working directory are uploaded. 

#### log subcommand {#log}

Enables you to view the cloudwatch logs for the specified lambda function.
Format for a call:
`log` call format:

`$ ask lambda log <-f|--function <functionName>> [--start-time <startTime>] 
   [--end-time <endTime>] [--limit <limitNumber>] [--raw]`

**Parameters**

--function|-f

:   Required. The Lambda function name.
 

--start-time

:    Optional. The start time for the log time range you wish to view. 

--end-time

:    Optional. The end time for the log time range you wish to view.

--limit

:    Optional. The number of log entries to display. 

--raw

:    Optional. Displays the logs without color or formatting.


### api command {#api}

The Api command enables you to retreive the current or publish new interaction models for a skill. 
For detailed desciption of the interaction model description, see <interaction model>.
`api` call format:

`$ ask api <subcommand> [-s|--skill-id <skillId>] [-l|--locale <locale>]`

**Subcommands**

- [get-model](#get-model)
- [head-model](#head-model)
- [set-model](#set-model)
- [get-build-status](#get-build-status)
- [create-skill](#createskill)
- [get-skill](#get-skill)
- [create-skill](#createskill)
- [create-account-linking](#createaccountlinking)
- [get-account-linking](#getccountlinking)

#### get-model subcommand {#get-model}

Retrieves the model schema for the skill with the specified skillId and locale. This will update the model schema and the eTag for it.
`get-model` call format:

`$ ask api get-model [-s|--skill-id <skillId>] [-l|--locale <locale>]`

**Parameters**

--skill-id, -s

:    Skill Id for the target model.
     If not specified, skill-id will be retrieved from project config. 

--locale, -l

:    Locale for the target model. Valid values are en_US, en_GB or de_DE. If  not specified, you will be promted to enter the locale.    

#### head-model subcommand {#headModel}

Enables you to get the eTag for the model of the skill with the specified skillId and locale. 
`head-model` call format:

`$ ask api head-model [-s|--skill-id <skillId>] [-l|--locale <locale>]`

**Parameters**

--skill-id, -s

:    Skill ID for the target model.
     If not specified, the skill ID will be retrieved from project configuration file.. 

--locale, -l

:   Locale for the target model. Valid values are en_US, en_GB or de_DE. If not specified, you will be promted to enter the locale.   

#### set-model subcommand {#setModel}

Enables you to set the specified interaction model schema for the specified skill and locale.
`set-model` call format:

`$ ask api set-model [-s|--skill-id <skillId>] [-l|--locale <locale>]`


**Parameters**

--skill-id, -s

:    Skill Id for the target model.
    If not specified, skill-id will be retrieved from project config. 

--locale, -l

:    Locale for the target model. Valid values are en_US, en_GB or de_DE. If not specified, you will be promted to enter the locale.   

#### get-build-status subcommand {#getbuildstatus}

Gets the whether the specified model is built.
`get-build-status` call format:

`$ ask api get-build-status [-s|--skill-id <skillId>] [-l|--locale <locale>]`

--skill-id, -s

:   The identifier for the skill you are checking status for. If you don't specify a skill ID, the tool will attempt to retrieve the ID from a project configuration file relative to the current directory. 

--locale, -l

:    Required. Locale for the target model. Valid values are en_US, en_GB or de_DE. If not specified, you will be promted to enter the locale.  

#### create-skill subcommand {#createskill}

Creates a skill with an skill schema. Returns the skill ID and the skill status.
`create-skill` call format:

`$ ask api create-skill`


#### get-skill {getskill} subcommand

Gets the skill the specified skill ID. This call will update the skill schema in the file system.

`get-skill` call format:

`$ ask api get-skill [-s|--skill-id <skillId>] [--stage <stage>]`

**Parameters**

--skill-id, -s

:    Optional. Skill ID for the skill to get. If not specified, the CLI uses the name of the current directory.

--stage

:    Optional. The stage for the skill. If not specified, default is `development`. 

#### update {#update} subcommand

Updates the specified skill with the skill schema in the current directory.
`update-skill` call format:

`$ ask api update-skill [-s|--skill-id <skillId>]`

**Parameters**

--skill-id, -s

 :    Optional. Skill ID for the skill to update. If not specified, the CLI uses the name of the current directory.

#### create-account-linking subcommand

Adds account linking configuration details for the specified skill ID.
`create-account-linking` call format:

`$ ask api create-account-linking [-s|--skill-id <skillId>]`

**Parameters**

--skill-id, -s

:    Optional. Skill ID for the skill to add account linking information for. If not specified, adds account linking for the skill specified in the /.ask/config file. If an skill ID isn't specified and not present in the config file, the call will fail.

You will be prompted to enter the following values. For more details on these values, see [Linking an Alexa User with a User in Your System](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/linking-an-alexa-user-with-a-user-in-your-system):

- Authorization URL
- Client ID
- Scopes (comma separated list)
- Domains (comma separated list)
- Authorization Grant Type - Use arrow keys to choose either IMPLICIT or AUTH_CODE

#### get-account-linking subcommand {#getaccountlinking}

Gets the account linking configuration details and outputs them to the console.
`get-account-linking` call format:

`$ ask api get-account-linking [-s|--skill-id <skillId>]`

**Parameters**

--skill-id, -s

:    Skill ID for the skill to get the account linking information for. 

#### vendors subcommand {vendors}

Gets the currently configured vendor information used by the ASK CLI, if needed, refreshes the access token, and outputs the currently configured vendor in json format. If there is more than one vendor, you will be asked to choose one from a list to refresh the token.
`vendors` call format:

`$ ask api vendors`

Sample profile output. This is also stored in $HOME/.ask/cli_config:

```json
[
  {
    "id": "MYVENDORID1234567",
    "name": "Amazon",
    "roles": [
      "ROLE_ADMINISTRATOR"
    ]
  }
]
```

### new command {#new}

Creates a new skill project in the current directory, The parent directory for the skill will be the same as the skill name, and the parent directory will contain the other files and directories necessary to manage the skill with the ASK CLI. The skill is created with the default profile contained in the $HOME/.ask/cli_config file. 
`new` call format:

`ask new <-n|--skill-name <name>> [--profile <profile_name>]`

--skill-name, -n

:    Required. The skill name for the newly created skill project. 

### deploy command {#deploy}

Deploys a skill project. This command will create a skill if it doesn't exist, or update an existing skill if a skill with the same name already exists.
If the skill has never been deployed, [project_root]/.ask/config.json will not have skill_id and lambda_arn in the default deployment section. CLI will create a new skill and a new Lambda function named "ask-<Skill Type>-<Skill Name>" (with a new Role named "ask-lambda-<skill name>", attached to the policy arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole). Then the newly created skill ID and Lambda function ARN will be put into the config file. If they already exist, the CLI will update the existing skill and Lambda function resources.
`deploy` call format:

`ask deploy [--target <target>]`

**Parameters**

--target, -t

:   target indicates whether you want to deploy the lamda code for a skill, the skill project, or both. Accepted values for target are: "all", "lambda", "skill". Default target is "all" if not specified. 

### clone command {#clone}

Clones the specified skill project. Used to set up a new project from the latest deployed one, possibly for updates or modifications.
`clone` call format:

`ask clone <-s|--skill-id <skillId>> [--stage <stage>] [--profile <profile>]`

**Parameters**

--skill-id, -s

:    Skill ID for the target skill. 

--stage

:    The stage for the skill.
     If not set, the stage is "development" by default. 

--profile

:    Profile is a top-level context that will own credential tokens and other config variables. Profile information is stored in the $HOME/.ask/cli_config. 

