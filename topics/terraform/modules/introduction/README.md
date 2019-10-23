# Introduction

### Overview

Terraform allows you to control your infrastructure on the cloud service provider the big providers like: AWS, GCP, Azure are supported. 
Your infrastructure can be described as code, allowing you to have a blueprint of your data-center that can be versioned like any other piece of code.

### Infrastructure as Code

Infrastructure as code means that we can use a high level or descriptive programming language to describe and manage infrastructure. 
There is a known issues with pipelines and that is environment drift, which means over time, an environment can end up in unique configuration that cannot be automatically recreated. 
When environments become inconsistent, deployments can be affected and testing can be made invalid. 
With infrastructure as code, the infrastructure configurations can be versioned and maintained, so if another environment needs to be created, you can be sure that you are using up to date configurations.

### Common use cases

* **Multi-Tier Applications** - It is very common to have applications with multiple tiers, each tier having different requirements and dependencies. 
With Terraform we are able to describe each tier of the application as a collection of resources so that the dependencies for each tier can be handled automatically.
* **Software Demos** - Although tools like Vagrant can be used to create environments for demos, vagrant can’t completely mimic production environments additionally depending on how large the infrastructure for the application is, it might be a challenging to run it on a laptop.
 Because configurations for vagrant can be distributed, demos can be run against the end user’s infrastructure with ease. 
 Parameters for the Terraform configurations can also be tweaked so that the software can be demoed at any scale.
* **Disposable Environments** - It is common to have a staging environment before deploying to production, which is a smaller clone of the production environment. 
Production environments over time can become more complex and require more effort to mimic on a smaller scale. 
With Terraform, the infrastructure will be kept as code and can easily applied to other new environments for testing and then be disposed. 
Spinning up and disposing environments this easily means that costs can also saved on environments that do not need to be operational 24/7 and only for a fraction of that time.
* **Multi-Cloud Deployments** - Terraform has the ability to configure infrastructure across more than one cloud service.
 This hybrid solution might be due to a customer wanting to take advantage of the features available on different cloud provider solutions. 
Another reason for multi-cloud deployments could be for extra fault tolerance.

### Tasks

We will now install terraform and check that the installation was successful.

### Windows

1. Navigate to https://www.terraform.io/downloads.html in a web browser and download Terraform for 64-bit windows
2. Extract the .zip file
3. Copy the terraform.exe file from where you decided to extract it to a new folder: `C:\tools\terraform\`
4. We now need to configure the PATH environment variable so that Terraform can be used easily on the command line.
    5. Press **Windows key + R** to open the **Run program**
    6. Type **SystemPropertiesAdvanced** and click **OK**
    <img align="left" src="https://imgur.com/6y4t3MX.jpg">
    7. Select **Environment Variables** button
    <img align="left" src="https://imgur.com/XihMpT9.jpg">
    8. Under *User Variables* for <your-username>, select **New…** and enter the Variable name: **TERRAFORM_HOME** and the Variable value: `C:\tools\terraform`, then click **OK** button
    <img align="left" src="https://imgur.com/EaIt6Jv.jpg">
    9. Under *User Variables* for <your-username>, select the variable called **Path** then click **Edit…** then enter **%TERRAFORM_HOME%**
    <img align="left" src="https://imgur.com/bkXxBsK.jpg">
    10. Click **OK** button on the *Environment Variable Windows* and close the *System Properties* window
    
### Linux

Follow the steps below, entering the commands into a terminal. 
1. Make sure your system is up to date:
    2. If you are using Debian/Ubuntu OS: `sudo apt update && sudo apt upgrade -y`
    3. If you are using CentOS/RHEL/Fedora: `sudo yum update -y`
4. Ensure the unzip and wget tools are installed:
    5. If you are using Debian/Ubuntu OS: `sudo apt install -y unzip wget`
    6. If you are using CentOS/RHEL/Fedora: `sudo yum install -y unzip wget`
7. Download the Terraform zip:
    8. Please not the version here will likely not be the latest, so please use the official download page to find out what the latest download link is: https://www.terraform.io/downloads.html. 
     You will likely need to download the 64-bit version.
    `wget https://releases.hashicorp.com/terraform/0.12.12/terraform_0.12.12_linux_amd64.zip`
9. Extract the Terraform zip archive: `unzip terraform_*_linux_*.zip`
10. Move the Terraform binary to the /usr/local/bin folder: `sudo mv terraform /usr/local/bin`
11. Remove the downloaded zip file: `rm terraform_*_linux_*.zip`

### Verify the Installation
You can verify that you have installed Terraform correctly by opening a command line or terminal and run the command below, the version of Terraform that you installed should be shown: `terraform --version`

### Creating a resource in AWS

We will now create a resource in AWS using Terraform.
First you need to find your `access_key` and `secret_key` in order to give terraform access to manage resources on AWS.
You can find them by following these steps:
1. Log in to your *AWS Management Console*
2. Click on your user name at the top right of the page
3. Click on the *Security Credentials* link from the drop-down menu
4. Find the *Access Credentials* section, and copy the latest *Access Key ID*, this is the `access_key` 
5. Click on the Show link in the same row, and copy the Secret Access Key, this is the `secret_key` 
    6. If there is no Secret Access Key, create a new one
7. Copy and save both in some text file but make sure to note down which is which. 
After saving both of them you should have them looking like this in your text file.
`
access_key = "AKIBIWX7DKIDGMCHPG4A"
secret_key = "3gSerUT5rreC989K5l4f3WcGZ0yUNaltaw4C8r/1"
`
