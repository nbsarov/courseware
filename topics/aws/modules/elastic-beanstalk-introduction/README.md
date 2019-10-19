<!--PROPS
{
    "prerequisites": [
        "aws/introduction"
		"aws/cli"
    ]
}
-->
# Elastic Beanstalk Introducion
<!--TOC_START-->
### Contents
- [Overview](#overview)
- [CLI Commands](#cli-commands)
- [Applications](#applications)
	- [Create](#create)
	- [Show Existing](#show-existing)
	- [Delete](#delete)
- [Versions](#versions)
	- [Create](#create-1)
		- [Sample Version](#sample-version)
		- [Version Using ZIP or WAR File](#version-using-zip-or-war-file)
	- [Show Existing Versions](#show-existing-versions)
		- [Show All Versions](#show-all-versions)
		- [Show Versions for an Application](#show-versions-for-an-application)
		- [Delete](#delete-1)
- [Environments](#environments)
	- [Create](#create-2)
- [Tasks](#tasks)

<!--TOC_END-->
## Overview
Elastic Beanstalk (EB) on AWS is Platform as a Service (PaaS) solution for deploying web applications and workloads.
EB can be used with little knowledge of infrastructure, allowing you to focus more on development.

EB has the potential to be used as a quick way to provision a test environment or be used for production solutions with the ability to create different versions to deploy and roll back versions of deployments.

Applications developed with any of the following are supported:
- Java
- .NET
- Nodejs
- Python
- PHP
- Ruby
- Go
- Docker

Common web servers can be also configure in EB:
- Apache
- Nginx
- Passenger
- IIS

## CLI Commands
EB can be easily used through the AWS console; a guided setup is provided.
Using the CLI will allow you gain a better understanding of how the different EB components work and the necessary configurations that you need to make.
Understanding the CLI commands will also allow you to script solutions for EB and any other resources in AWS for that matter.

More information for the commands that can be used for Elastic Beanstalk can be found [here](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-application.html).

## Applications
An application is the highest level of configurtation for EB.
You may have more than one application in EB if you would like to.
### Create
You will need to have a name for your application that isn't the same as any another applications in your EB, you may also provide a description for you application.

```bash
# aws elasticbeanstalk create-application --application-name [APPLICATION_NAME] --description "[DESCRIPTION]"
aws elasticbeanstalk create-application --application-name my-first-application --description "My first EB Application"
```

### Show Existing
Existing applications and information about them can be shown by using the `describe-applications` command.
```bash
# aws elasticbeanstalk describe-applications
aws elasticbeanstalk describe-applications
```
If you have a lot of applications then specific applications can be described using the `--application-names` option:
```bash
# aws elasticbeanstalk describe-applications --application-names "[APPLICATION_NAME]" "[APPLICATION_NAME]"
aws elasticbeanstalk describe-applications --application-names "app-1" "app-2"
```

### Delete
The application can be deleted by providing the unique name of it to the `delete-application` command:
```bash
# aws elasticbeanstalk delete-application --application-name [APPLICATION_NAME]
aws elasticbeanstalk delete-application --application-name my-first-application
```

## Versions
The way that deployments can be managed in EB is through versions of your applications.
### Create
There are some required and preffered options to include when creating an application version:
- `--application-name` (required): the name of the application that you are creating a version for
- `--description` (optional): description for the version
- `--source-bundle` (optional): the code to deploy, if ommited, a sample application will be set by AWS. The source bundle can be either from an AWS CodeCommit Git repository or a ZIP or WAR file stored in S3. 
- `--auto-create-application` (optional): if you haven't created an application yet then one will be created if you use this option and the application doesn't exist
#### Sample Version
Here a sample version will be created because a source bundle has not been configured:
```bash
# aws elasticbeanstalk create-application-version --application-name [APPLICATION_NAME] --version-label [VERSION_LABEL]
aws elasticbeanstalk delete-application --application-name my-first-application --version-label v1
```
#### Version Using ZIP or WAR File
The AWS sample applications are good for seeing how EB work however we will of course need to be able to deploy our own code to EB, which can be done using the `--source-bundle` option.
Details on configuring these source bundles can be found in another module.

We will be looking at using a ZIP file for a source bundle in this case which can be retrieved from and S3 bucket.
Note that the S3 bucket that you are using must be in the same region as the EB application.
```bash
# aws elasticbeanstalk create-application-version --application-name [APPLICATION_NAME] --version-label [VERSION_LABEL] --source-bundle S3Bucket="[BUCKET_NAME]",S3Key="[S3_KEY]"
aws elasticbeanstalk delete-application --application-name my-first-application --version-label v1 --source-bundle S3Bucket="my-bucket",S3Key="my-app-v1.zip"
```
### Show Existing Versions
Versions of the application that already existing can be viewed by using the `describe-application-versions` command.
#### Show All Versions
If you provide no arguments then all the versions for every application that you have will be shown.
```bash
# aws elasticbeanstalk describe-application-versions
aws elasticbeanstalk describe-application-versions
```
#### Show Versions for an Application
If there is a specific aplpication that you want to see the versions for the `--application-name` option can be provided to filter them out:
```bash
# aws elasticbeanstalk describe-application-versions --application-name my-application
aws elasticbeanstalk describe-application-versions --application-name my-application
```
#### Delete
A version can be deleted by providing the *application name* and *version label*:
```bash
# aws elasticbeanstalk delete-application-version --application-name my-application --version-label [VERSION_LABEL]
aws elasticbeanstalk delete-application-version --application-name my-application --version-label v1
```
## Environments
Environments are completely different instances of the application that are running.
This is of course ideal for testing new features and bug fixes before deploying any new versions to a production instance of the application that is in service.

### Create
An environment can be created with the `create-environment` command.
To create a new environment the following information is required:
- Environment Name (`--environment-name`)  
  The name for your environment has the following contraints:
  - 4-40 characters in length
  - Only container letters, numbers and hyphens
  - Unique within a region in your account - you can't have two environments called `dev` in London for example
- Application Name (`--application-name`)  
  This is the name of the application which has the version that you are wanting to deploy.
- Version Label (`--version-label`)  
  This is the specific version that is going to be deployed to the environment
```bash
# aws elasticbeanstalk create-environment --environment-name [ENVIRONMENT_NAME] --application-name [APPLICATION_NAME] --version-label [VERSION_LABEL]
aws elasticbeanstalk create-environment --environment-name my-env --application-name my-app --version-label v1
```
### View Existing 
Existing environments can be viewed using the `describe-environments` command, providing no options will return all envinronments:
```bash
# aws elasticbeanstalk describe-environments
aws elasticbeanstalk describe-environments
```
#### For an Application
You may want to only see the environments for a specific application, this can be done by passing the application name into the `--application-name` argument.
```bash
# aws elasticbeanstalk descibe-environments --application-name [APPLICATION_NAME]
aws elasticbeanstalk descibe-environments --application-name my-app
```
### Terminate Environment
Terminating an environment is effectively deleting it which is done by using the `terminate-environment` commmand.
When terminating an environment all you will need to provide is the *environment name*:
```bash
# aws elasticbeanstalk terminate-environment --environment-name [ENVIRONMENT_NAME]
aws elasticbeanstalk terminate-environment --environment-name [ENVIRONMENT_NAME]
```
## Tasks
Here we are going to deploy an Elastic Beanstalk sample application, which will entail creating:
- An *Application*
- A *Version* of the application to deploy
- An *Environment* to deploy the version of the application in
#### Prerequisites
- AWS CLI configured
### Create the Application
First off we are going to need an application, create one by runnig the command below:
```bash
aws elasticbeanstalk create-application --application-name sample-eb-app --description "Sample Elastic Beanstalk Application"
```
Now at anytime we can see details about the application:
```bash
aws elasticbeanstalk describe-applications
```
You should see an output like this:
```json
{
    "Applications": [
        {
            "ApplicationName": "sample-eb-app",
            "Description": "Sample Elastic Beanstalk Application",
            "DateCreated": "2019-10-19T10:05:53.895Z",
            "ConfigurationTemplates": [],
            "DateUpdated": "2019-10-19T10:05:53.895Z",
            "ResourceLifecycleConfig": {
                "VersionLifecycleConfig": {
                    "MaxCountRule": {
                        "DeleteSourceFromS3": false,
                        "Enabled": false,
                        "MaxCount": 200
                    },
                    "MaxAgeRule": {
                        "DeleteSourceFromS3": false,
                        "Enabled": false, 
                        "MaxAgeInDays": 180
                    }
                }
            },
            "ApplicationArn": "arn:aws:elasticbeanstalk:eu-west-2:847151757780:application/sample-eb-app"
        }
    ]
}
```
### Create an Application Version
The next thing that is needed is a version of the application which can be deployed, lets create one with a label of `v1` without a source bundle specified so that AWS deploys a sample application for us:
```bash
aws elasticbeanstalk create-application-version --application-name sample-eb-app --version-label v1
```
You should now be able to view information about your new application version at any point:
```bash
aws elasticbeanstalk describe-application-versions
```
You should see something like this:
```json
{
    "ApplicationVersions": [
        {
            "ApplicationName": "sample-eb-app",
            "Status": "UNPROCESSED",
            "VersionLabel": "v1",
            "ApplicationVersionArn": "arn:aws:elasticbeanstalk:eu-west-2:847151757780:applicationversion/sample-eb-app/v1",
            "DateCreated": "2019-10-19T10:11:07.826Z",
            "DateUpdated": "2019-10-19T10:11:07.826Z",
            "SourceBundle": {
                "S3Bucket": "elasticbeanstalk-eu-west-2",
                "S3Key": "GenericSampleApplication"
            }
        }
    ]
}
```
### Create an Environment
So this is the last step really, now that we have an application and an application version we can make an environment with the version deployed on it.
We'll call the environment `development` and provide the *application name* and *version label*:
```bash
aws elasticbeanstalk create-environment --environment-name development --application-name sample-eb-app --version-label v1
```
Great, now lets see information about our new environment:
```bash
aws elasticbeanstalk describe-environments
```
```json

```