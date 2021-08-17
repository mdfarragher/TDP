# Module 1 Lab 1 setup

To complete this lab, you will need to deploy an Azure Databricks workspace in your Azure subscription.

## Deploy Azure Databricks

1. Click the following button to open the deployment template in the Azure Portal.

   [![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.databricks%2Fdatabricks-workspace%2Fazuredeploy.json)

1. Provide the required values to create your Azure Databricks workspace:

   - **Subscription**: Choose your Azure Pass subscription.
   - **Resource Group**: Select **Create new** and provide the name `techionista-dp203-m1a` for the new resource group. 
   - **Region**: Select `West Europe`.
   - **Workspace Name**: Enter a unique name for your workspace.
   - **Enable No Public Ip**: Set to `false`.
   - **Pricing Tier**: Ensure `premium` is selected.
   - **Location**: Leave this at the default `[resourceGroup()location]` value.

\
   ![The form is configured as described.](media/databricks-arm-template.png "Deploy an Azure Databricks Workspace")

1. Select **Review + create**.
1. Select **Create**.
1. The workspace creation takes a few minutes. During workspace creation, the portal displays the **Submitting deployment for Azure Databricks** tile. You may need to scroll right on your dashboard to see the tile. There is also a progress bar displayed near the top of the screen. You can watch either area for progress.

### Create a cluster

1. When your Azure Databricks workspace creation is complete, select the link to go to the resource.
1. Select **Launch Workspace** to open your Databricks workspace in a new tab.
1. In the left-hand menu of your Databricks workspace, select **Clusters**.
1. Select **Create Cluster** to add a new cluster.

    ![The create cluster page](media/create-a-cluster.png)

1. Enter a name for your cluster, such as `Test Cluster`.
1. Select the **Databricks RuntimeVersion**. We recommend the latest runtime and **Scala 2.12**.
1. Select the default values for the cluster configuration.
1. Select **Create Cluster**.
