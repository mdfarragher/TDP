# Module 1 Lab 2 setup - step 1

## Prerequisites

You'll need the following to perform the deployment steps for the DP-203 training:

- A GitHub account to access the GitHub repository.
- A Power BI Pro subscription. In case you do not have a paid Power BI Pro subscription, you can get a 60 days trial by signing in to `https://powerbi.com` with your account and selecting `Try free`.
- A Power BI Pro workspace (for details about how to set up a workspace in Power BI, see [Create the new workspaces in Power BI](https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-power-bi)).

### Create a Resource Group

1. Log into the [Azure Portal](https://portal.azure.com) using your Azure Pass.

2. On the Azure Portal home screen, select the **Menu** button on the top-left corner **(1)**. Hover over **Resource groups (2)**, then select **+ Create (3)**.

    ![The Create button is highlighted.](../04/media/new-resourcegroup.png "Create resource group")

3. On the **Create a resource group** screen, select your Azure Pass Subscription. For the Region, select `West Europe`. For Resource group, enter `techionista-dp203-m1b` and select the **Review + Create** button.

    ![The Create a resource group form is displayed.](../04/media/bhol_resourcegroupform.png)

4. Select the **Create** button once validation has passed.

## Create a File Share

In the Azure Portal, navigate to the `techionista-dp203-m1b` resource group and create a new storage account. Provide the following information:

- **Subscription**: Choose your Azure Pass subscription.
- **Resource Group**: Select `techionista-dp203-m1b`. 
- **Region**: Select `West Europe`.

Leave all other fields at their default settings and create the storage account. 

When the storage account has been created, select `File shares` (under the `File service` settings group) and create a new file share named `cloud-shell-fileshare`.

## Configure Azure Cloud Shell

Select the Cloud Shell icon (located in the top right part of the page) and then select `PowerShell`:

![Cloud Shell configuration start](media/cloudshell-configure-01.png)

Select your Azure Pass subscription under `Subscription` if it's not already selected, and then select `Show advanced settings`:

![Cloud Shell configuration advanced settings](media/cloudshell-configure-02.png)

Provide values for the following fields:

- **Cloud Shell region**: Select `West Europe`.
- **Resource group**: select `Use existing` and then select `techionista-dp203-m1b` from the list.
- **Storage account**: select `Use existing` and then select the storage account you created above.
- **File share**: select `Use existing` and then select the `cloud-shell-fileshare` file share you created above.

Select `Attach storage` once all the values are in place.

![Cloud Shell configuration advanced settings values](media/cloudshell-configure-03.png)

Once configuration is complete, you should get an instance of Cloud Shell:

![Cloud Shell](media/cloudshell-configure-04.png)

## Deploy Synapse Analytics

Click the `Deploy to Azure` button below to start the deployment process.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F01%2Farm%2Fasaga-workspace-core.json%3Ftoken%3DAA2FKXRAGLJK2Q5PS7UV6QC7ZZAS2)

You should see next the `Custom deployment` screen where you need to provide the following:

- The resource group where the Synapse Analytics workspace will be deployed. Select `techionista-dp203-m1b`.
- A unique suffix used to generate unique names during deployment. Type a combination of your initials and the current date (for example `mdf1708`), but make sure your suffix does not exceed a **maximum** length of **9 characters**). Remember this suffix because you will need it later.
- The password for the SQL Administrator account. Type a unique password and remember it because you will need it during the labs.

Select `Review + create` to validate the settings.

![Synapse Analytics workspace deployment configuration](media/asaworkspace-deploy-configure.png)

Once the validation is passed, select `Create` to start the deployment. You should see next an indication of the deployment progress:

![Synapse Analytics workspace deployment progress](media/asaworkspace-deploy-progress.png)

Wait until the deployment completes successfully before proceeding to the next step.

## Tag your resource group with the unique suffix

In the Azure Portal, navigate to the `techionista-dp203-m1b` resource group.

Select the `Tags` section and add a new tag named `DeploymentId`. Copy the unique suffix you used earlier (for example: `mdf1708`) and paste it as the value of the tag. Then select `Apply` to save it.

![Synapse Analytics workspace resource group tagging](media/asaworkspace-deploy-tag.png)

The deployment of your Synapse Analytics workspace is now complete. Next, you will deploy the artifacts required by the labs into the newly created Synapse Analytics workspace.

## Clone a GitHub repo in Cloud Shell

In the Azure Portal, navigate to the `techionista-dp203-m1b` resource group and start a Cloud Shell instance.

Once the Cloud Shell instance becomes available, **run ```az login```** to make sure the correct account and subscription context are set:

![Cloud Shell login](media/cloudshell-setup-01.png)

Now run the following command:

```cmd
git clone https://github.com/solliancenet/microsoft-data-engineering-ilt-deploy asa
```

If GIT asks for credentials, provide your GitHub username and password.

Once the repository is successfully cloned, you should see a result similar to this:

![Cloud Shell git clone repository](media/cloudshell-setup-02.png)

Change your current directory using

```cmd
cd asa/setup/01/automation
```

and then start the setup script using

```powershell
.\environment-setup.ps1
```

Make sure the selected subscription is your Azure Pass:

![Cloud Shell select subscription](media/cloudshell-setup-03.png)

Enter the name of the `techionista-dp203-m1b` resource group where you deployed the Synapse Analytics workspace:

![Cloud Shell select resource group](media/cloudshell-setup-04.png)

The setup script will now proceed to create all necesary Synapse Analytics artifacts in your environment.

The process should take 5 to 10 minutes to finish. Wait until the setup script is finished before proceeding to the next steps.

## Connect Azure Synapse Analytics to Power BI

In the Azure Portal, navigate to your `techionista-dp203-m1b` resource group, open the Synapse workspace resource (which should be named `asagaworkspace<unque_suffix>` where `<unique_suffix>` is the one you specified when creating the workspace), and then open Synapse Studio.

In Synapse Studio, select the `Manage` hub on the left side, select `Linked Services`, and then select `+ New` to start creating a new linked service. Select `Connect to Power BI` to start configuring the linked service.

If the `Connect to Power BI` option does not show up, enter `Power BI` in the search box, select `Power BI` and then select `Continue`.

![Start configuring a new Power BI linked service](media/asaworkspace-deploy-pbi-linked-service-01.png)

In the `New linked service (Power BI)` dialog enter settings as follows:

- **Name**: enter `asagapowerbi<unique_suffix>` (where `<unique_suffix>` is the one you specified when creating the Synapse Analytics workspace).
- **Tenant**: ensure the correct tenant is selected (the one that contains your Azure AD account).
- **Workspace name**: select the Power BI workspace you want to use.

Select `Create` to create the Power BI linked service.

![Power BI linked service configuration](media/asaworkspace-deploy-pbi-linked-service-02.png)

After the linked service is successfully created, select the `Develop` hub on the left side and expand the `Power BI` section. You should see your Power BI workspace listed.

![Check Power BI linked service configuration](media/asaworkspace-deploy-pbi-linked-service-03.png)
