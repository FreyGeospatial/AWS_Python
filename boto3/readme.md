# Setting up AWS CLI

Note: Everything explained here assumes you have an AWS account with higher level privileges.

## Credentials
Simply calling `boto3.client('ssm')` or `boto3.client('dynamodb')` in Python will not work if boto3 client cannot find the aws credentials. This includes not just profiles or access keys/secrets, but also regions. To find out if you're already configured, use:
```
$ aws configure list
```
in the AWS CLI to see the stored (or missing) credentials. You can also access these credentials by opening file (no extension) `~/.aws/config`. On Windows, this would be `C:\Users\<username>\.aws\config`. You can find the tenant id, app id, username, default role arn, duration of login session, region, and potentially other user information needed to access AWS services from the CLI. You can manually edit this file to add missing information. It also contains default and named profile configurations.

The format would look something like this on Windows with Azure Active Directory: `aws-azure-login`:
```
[default]
azure_tenant_id=<id here>
azure_app_id_uri=<id here>
azure_default_username=<username here>
azure_default_role_arn=<role arn goes here>
azure_default_duration_hours=<integer>
azure_default_remember_me=<boolean>
region=<AWS region>
```

To sign into aws with Azure Active Directory, type `aws-azure-login` into Powershell.

On Mac, there are two files produced: `config` and `credentials` inside `~/.aws`. These files look like this, respectively:
```
[default]
region = <AWS region>
output = <default output format>
```
```
[default]
aws_access_key_id = <access key>
aws_secret_access_key = <secret key>
```

On MacOS, if after installing the AWS CLI (instructions not shown here) you have not yet created the above files, type `aws configure` into the terminal, and enter in the requested information. If you do not have an access or secret key, you must first create them in the IAM Management Console. After completing these steps, you should be able to access AWS programmatically via the `boto3` Python package.

### Named Profiles
A named profile is a collection of settings and credentials that you can use for a given AWS CLI command. You can store multiple named profiles in config or credentials files. A default profile may be specified when no profile is explicity referenced in a command, as seen above. The profiles (default and otherwise) are located in the config file.


# Setting up Services used in this Tutorial via CLI

## Running local version on DynamoDB, outside of cloud environment
You must have Java Runtime Environment (JRE) version 8.x or newer. Check current version by running `java -version` in the terminal. If it doesn't run, you don't have Java installed.


Download a .jar file containing the local version of DynamoDB from https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html. I unzipped the archive and placed it in my applications folder, but you can put it anywhere. Configure your credentials if you have not done so yet, though according to the docs, you don't need to input "real" credentials for this local instance. `cd` to the folder containing the .jar file and then run either:

MacOS Terminal:
 `java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb`. 

Powershell:
`java -D"java.library.path=./DynamoDBLocal_lib" -jar DynamoDBLocal.jar`

 To check that the AWS CLI can read from your local DynamoDB instance, run `aws dynamodb list-tables --endpoint-url http://localhost:8000`. The terminal should return in JSON format an empty list.

## S3 File Store

See the Jupyter Notebook contained in this repo for Python code utilizing boto3 for S3 implementation.

## CloudWatch

Nothing here yet

## Boto3 Overview:

Boto3 is a Python package allowing for interaction between the Python environment and AWS. It has two different ways of making service requests:
* Via **client** abstraction
* Via **resource** extraction

E.g., `boto3.client('DynamoDB')` or `boto3.resource('DynamoDB')`

All(?) AWS services are accessible via `client` but not all are accessible with `resource`. They are both abstractions used to accomplish the same or similar tasks, but may require different steps.

https://stackoverflow.com/questions/42809096/difference-in-boto3-between-resource-client-and-session






# Tutorials used in this repo:
* https://www.youtube.com/watch?v=qsPZL-0OIJg























