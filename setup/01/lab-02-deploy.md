# Module 1 Lab 2 setup - step 2

To complete this lab, you will need to load some test data into the Azure Synapse Analytics workspace you set up earlier.

## Perform Custom Deploy

Click the following button to open the deployment template in the Azure portal.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F01%2Farm%2Fasaga-workspace-lab-02.json%3Ftoken%3DAA2FKXURSMJAZDFEIRRDZVK7Z3WVC)

You should see next the `Custom deployment` screen where you need to provide the following information:

   - **Resource Group**: Provide the same resource group you used earlier to install Azure Synapse Analytics. 
   - **Region**: Select `West Europe`.

Select `Review + create` to validate the settings.

![Synapse Analytics workspace deployment configuration](media/lab-02-deploy-configure.png)

Once the validation is passed, select `Create` to start the deployment. You should see next an indication of the deployment progress:

![Synapse Analytics workspace deployment progress](media/lab-02-deploy-progress.png)

Wait until the deployment completes successfully before proceeding to the next step.

## Load Data Into Azure Synapse Analytics

In the Azure Portal, navigate to the resource group you used to deploy the Synapse Analytics workspace and start a Cloud Shell instance.

Once the Cloud Shell instance becomes available, **run ```az login```** to make sure the correct account and subscription context are set:

![Cloud Shell login](media/cloudshell-setup-01.png)

Run the following command to make sure the Git repository has been correctly cloned:

```cmd
dir
```

Change your current directory using

```cmd
cd asa/setup/01/automation
```

and then start the setup script using

```powershell
.\lab-02-setup.ps1
```

Make sure the selected subscription is your Azure Pass:

![Cloud Shell select subscription](media/cloudshell-setup-03.png)

Enter the name of the resource group where you deployed the Synapse Analytics workspace:

![Cloud Shell select resource group](media/cloudshell-setup-04.png)

The setup script will now proceed to create all necessary Synapse Analytics artifacts in your environment.

The process should take a few minutes to finish. Once it completes successfully, you have completed the deployment of the lab.
