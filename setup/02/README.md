# Module 2 Lab setup

## Prerequisites

You'll need the following to perform these deployment steps:

- A GitHub account to access the GitHub repository.

## Lab software requirements

Install the following on your lab computer:

- [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15)

## Create a Resource Group

In the Azure Portal, navigate to the Resource Groups overview page and create a new resource group. Give the group the following name: `techionista-dp203-m2`.

## Deploy Azure resources

Click the `Deploy to Azure` button below to start the deployment process.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F02%2Fmodule2-deployment.json%3Ftoken%3DAA2FKXRAGLJK2Q5PS7UV6QC7ZZAS2)

You should see next the `Custom deployment` screen where you need to provide the following (see [Pre-requisites for deployment](#pre-requisites-for-deployment) above for details):

- The resource group `techionista-dp203-m2` where the Synapse Analytics workspace will be deployed.
- A unique suffix used to generate unique names during deployment. Type a combination of your initials and the current date (for example `mdf1708`), but make sure your suffix does not exceed a **maximum** length of **9 characters**). Remember this suffix because you will need it later.
- The password for the SQL Administrator account. Type a unique password and remember it because you will need it during the labs.

Select `Review + create` to validate the settings.

![Synapse Analytics workspace deployment configuration](media/asaworkspace-deploy-configure.png)

Once the validation is passed, select `Create` to start the deployment. You should see next an indication of the deployment progress:

![Synapse Analytics workspace deployment progress](media/asaworkspace-deploy-progress.png)

Wait until the deployment completes successfully.
