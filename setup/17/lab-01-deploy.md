# Module 17 Lab setup - step 2

## Lab software requirements

Install the following on your lab computer:

- [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=58494).

## Deploy Azure resources

Click the `Deploy to Azure` button below to start the deployment process.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F01%2Farm%2Fasaga-workspace-lab-01.json%3Ftoken%3DAA2FKXX5X2DEGNWNV6LNIT27ZYETC)

You should see next the `Custom deployment` screen where you need to provide the `techionista-dp203-m17` resource group where the Synapse Analytics workspace was deployed.

Select `Review + create` to validate the settings.

![Synapse Analytics workspace deployment configuration](media/lab-01-deploy-configure.png)

Once the validation is passed, select `Create` to start the deployment. You should see next an indication of the deployment progress:

![Synapse Analytics workspace deployment progress](media/lab-01-deploy-progress.png)

Wait until the deployment completes successfully before proceeding to the next step.

In the Azure Portal, navigate to the resource group you used to deploy the Synapse Analytics workspace (see [Pre-requisites for deployment](./asa-workspace-deploy.md#pre-requisites-for-deployment) for details) and start a Cloud Shell instance (see [Configure the Azure Cloud Shell](#configure-the-azure-cloud-shell) above for details).

Once the Cloud Shell instance becomes available, **run ```az login```** to make sure the correct account and subscription context are set:

![Cloud Shell login](media/cloudshell-setup-01.png)

Run the following command to make sure the Git repository has been correctly cloned:

```cmd
dir
```

Change your current directory using:

```cmd
cd asa/setup/17/automation
```

and then start the setup script using:

```powershell
.\lab-01-setup.ps1
```

Make sure the selected subscription is your Azure Pass:

![Cloud Shell select subscription](media/cloudshell-setup-03.png)

Enter the name of the `techionista-dp203-m17` resource group where you deployed the Synapse Analytics workspace:

![Cloud Shell select resource group](media/cloudshell-setup-04.png)

The setup script will now proceed to create all necessary Synapse Analytics artifacts in your environment.

The process should take a few minutes to finish. Once it completes successfully, you have completed the deployment of the lab.
