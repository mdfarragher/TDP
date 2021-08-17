
# Module 4 Lab setup

## Requirements

- Make sure the following resource providers are registered for your Azure Subscription.  

   - Microsoft.Sql
   - Microsoft.Synapse
   - Microsoft.StreamAnalytics
   - Microsoft.EventHub  

    See [further documentation](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types#azure-portal) for more information on registering resource providers on the Azure Portal.

- You'll also need a Power BI Trial, Pro or Premium account to host Power BI reports used for the lab in Module 16.

## Lab software requirements

Install the following on your lab computer:

- [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=58494). You'll need this for Module 16.

## Azure setup

The entire setup process will take from 1.5 to 2 hours to complete.

### Create a Resource Group

1. Log into the [Azure Portal](https://portal.azure.com) using your Azure Pass.

2. On the Azure Portal home screen, select the **Menu** button on the top-left corner **(1)**. Hover over **Resource groups (2)**, then select **+ Create (3)**.

    ![The Create button is highlighted.](media/new-resourcegroup.png "Create resource group")

3. On the **Create a resource group** screen, select your Azure Pass Subscription. For the Region, select `West Europe`. For Resource group, enter `techionista-dp203-m4` and select the **Review + Create** button.

    ![The Create a resource group form is displayed populated with Synapse-MCW as the resource group name.](media/bhol_resourcegroupform.png)

4. Select the **Create** button once validation has passed.

### Create the Deployment Virtual Machine

1. In the [Azure portal](https://portal.azure.com), type in "virtual machines" in the top search menu and then select **Virtual machines** from the results.

    ![In the Services search result list, Virtual machines is selected.](media/azure-create-vm-search.png "Virtual machines")

2. Select **+ Add** on the Virtual machines page and then select the **Virtual machine** option.

3. In the **Basics** tab, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Subscription                   | _select your Azure Pass subscription_              |
   | Resource group                 | _select `techionista-dp203-m4` (the name of the resource group you created in the previous task)_                      |
   | Virtual machine name           | _`synapse-lab-setup-vm`_      |
   | Region                         | _select `West Europe`_             |
   | Availability options           | _select `No infrastructure redundancy required`_   |
   | Image                          | _select `Windows 10 Pro, Version 1809 - Gen1`_     |
   | Azure Spot instance            | _set to `Unchecked`_                                      |
   | Size                           | _select `Standard_D8s_v3`_                         |
   | Username                       | _select `labuser`_                             |
   | Password                       | _enter a password you will remember_               |
   | Public inbound ports           | _select `Allow selected ports`_                    |
   | Select inbound ports           | _select `RDP (3389)`_                              |
   | Licensing                      | _select the option to confirm that you have an  eligible Windows 10 license with multi-tenant hosting rights._ |

   ![The form fields are completed with the previously described settings.](media/azure-create-vm-1.png "Create a virtual machine")

4. Select **Review + create**. On the review screen, select **Create**. After the deployment completes, select **Go to resource** to go to the virtual machine.

    ![The Go to resource option is selected.](media/azure-create-vm-2.png "Go to resource")

5. Select **Connect** from the actions menu and choose **RDP**.

    ![The option to connect to the virtual machine via RDP is selected.](media/azure-vm-connect.png "Connect via RDP")

6. On the **Connect** tab, select **Download RDP File**.

    ![Download the RDP file to connect to the Power BI virtual machine.](media/azure-vm-connect-2.png "Download RDP File")

7. Open the RDP file and select **Connect** to access the virtual machine. When prompted for credentials, enter `labuser` for the username and the password you chose.

    ![Connect to a remote host.](media/azure-vm-connect-3.png "Connect to a remote host")

    Click Yes to connect despite security certificate errors when prompted.

    ![The Yes button is highlighted.](media/rdp-connect-certificate.png "Remote Desktop Connection")

A new window will open and you'll see the desktop of the virtual machine you just created. Leave this window open while you continue with the next step.

### Deploy Azure Synapse Analytics

1. Deploy Azure Synapse Analytics with an Azure deployment template by clicking the button below:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F04%2Fartifacts%2Fenvironment-setup%2fautomation%2F00-asa-workspace-core.json" target="_blank"><img src="https://aka.ms/deploytoazurebutton" /></a>

2. On the **Custom deployment** form fill in the fields described below.

   - **Subscription**: Select your Azure Pass subscription.
   - **Resource group**: Select the `techionista-dp203-m4` resource group you previously created.
   - **Region**: Select `West Europe`.
   - **Unique suffix**: used to generate unique names during deployment. Type a combination of your initials and the current date (for example `mdf1708`), but make sure your suffix does not exceed a **maximum** length of **9 characters**). Also make sure you follow correct Azure [Resource naming](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming) conventions.
   - **SQL Administrator Login Password**: Provide a strong password for the SQLPool that will be created as part of your deployment. [Visit here](https://docs.microsoft.com/en-us/sql/relational-databases/security/password-policy?view=sql-server-ver15#password-complexity) to read about password rules in place. Your password will be needed during the next steps. Make sure you remember your password.

3. Select the **Review + create** button, then **Create**. The provisioning of your deployment resources will take approximately 13 minutes. 

**Wait** until provisioning successfully completes before continuing. You will need the resources in place before running the scripts below.

    > **Note**: You may experience a deployment step failing in regards to Role Assignment. This error may safely be ignored.

## Configure the Deployment Virtual Machine

The entire script will take between 1.5 and 2 hours to complete. Major steps include:

- Configure Synapse resources
- Download all data sets and files into the data lake (~15 mins)
- Execute the setup and execute the SQL pipeline (~30 mins)
- Execute the Cosmos DB pipeline (~25 mins)

### Software installation

Go back to the RDP window that shows the desktop of the **deployment virtual machine** you created earlier, and install these prerequisites before continuing.

Make sure you install these prerequisites on the deployment VM. They should **NOT** be installed on your laptop!

- Install VC Redist: <https://aka.ms/vs/15/release/vc_redist.x64.exe>
- Install MS ODBC Driver 17 for SQL Server: <https://www.microsoft.com/download/confirmation.aspx?id=56567>
- Install SQL CMD x64: <https://go.microsoft.com/fwlink/?linkid=2082790>
- Install [Git client](https://git-scm.com/downloads) accepting all the default options in the setup.
- [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/windows-powershell/install/installing-windows-powershell?view=powershell-7)

### Download artifacts and install PowerShell modules

Perform all of the following steps in your **deployment virtual machine**:

1. Open a PowerShell Window as an administrator, and run the following commands to download the artifacts

    ```powershell
    mkdir c:\labfiles

    cd c:\labfiles

    git clone https://github.com/solliancenet/microsoft-data-engineering-ilt-deploy.git data-engineering-ilt-deployment
    ```

* In the same PowerShell window, execute the following:

    ```powershell
    if (Get-Module -Name AzureRM -ListAvailable) {
        Write-Warning -Message 'Az module not installed. Having both the AzureRM and Az modules installed at the same time is not supported.'
        Uninstall-AzureRm -ea SilentlyContinue
        Install-Module -Name Az -AllowClobber -Scope CurrentUser
    } else {
        Install-Module -Name Az -AllowClobber -Scope CurrentUser
    }
    ```

    > [!Note]: You may be prompted to install NuGet providers, and receive a prompt that you are installing the module from an untrusted repository. Select **Yes** in both instances to proceed with the setup

* In the same PowerShell window, execute the following:

    ```powershell
    Install-Module -Name Az.CosmosDB -AllowClobber
    ```

    > [!Note]: If you receive a prompt that you are installing the module from an untrusted repository, select **Yes to All** to proceed with the setup.

* In the same PowerShell window, execute the following:

    ```powershell
    Install-Module -Name SqlServer -AllowClobber
    ```

* In the same PowerShell window, execute the following:

    ```powershell
    Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
    ```

**IMPORTANT**

* Once the last command has completed, **close the PowerShell window**.

### Execute setup scripts

Perform all of the steps below in your **deployment VM**:

* Open Windows PowerShell as an Administrator and execute the following:

    ```powershell
    Set-ExecutionPolicy Unrestricted
    ```

    > [!Note]: If you receive a prompt that you are installing the module from an untrusted repository, select **Yes to All** to proceed with the setup.

* In the same PowerShell window, execute the following:

    ```powershell
    Import-Module Az.CosmosDB
    ```

* In the same PowerShell window, execute the following:

    ```powershell
    cd c:\labfiles\data-engineering-ilt-deployment\setup\04\artifacts\environment-setup\automation\
    ```

* Execute `Connect-AzAccount` and sign in to your Microsoft user account when prompted.

    > [!WARNING]: You may receive the message "TenantId 'xxxxxx-xxxx-xxxx-xxxx' contains more than one active subscription. The first one will be selected for further use. You can ignore this at this point. When you execute the environment setup, you will choose the subscription in which you deployed the environment resources.

* Execute `az login` and sign in to your Microsoft user account when prompted.

    > If you receive the following error, and have already closed and re-opened the PowerShell window, you need to restart your computer and restart the steps in this task: `The term 'az' is not recognized as the name of a cmdlet, function, script file, or operable program`.

* Execute `.\01-environment-setup.ps1`

1. You will be prompted to setup your Azure PowerShell and Azure CLI context.

2. If you have more than one Azure Subscription, you will be prompted to enter the name of your desired Azure Subscription. You can copy and paste the value from the list to select one. For example:

    ![A subscription is copied and pasted into the text entry.](media/select-desired-subscription.png "Select desired subscription")

3. Enter the name of the resource group you created at the beginning of the environment setup. This is `techionista-dp203-m4`. This will make sure automation runs against the correct environment you provisioned in Azure.

    During the execution of the automation script you may be prompted to approve installations from PS-Gallery. Please approve to proceed with the automation.

    ![The Azure Cloud Shell window is displayed with a sample of the output from the preceding command.](media/untrusted-repo.png)

    > **NOTE** This script will take between 90 and 150 minutes to complete.

### Possible errors that you can safely ignore

You may encounter a few errors and warnings during the script execution. The errors below can safely be ignored:

1. The following error may occur when creating SQL users and adding role assignments in the dedicated SQL pool, and can safely be ignored: `Principal 'xxx@xxx.com' could not be created. Only connections established with Active Directory accounts can create other Active Directory users.`

    ![Error is displayed.](media/error-cannot-create-principal.png "Cannot create principal")

2. Toward the end of the script, you may see the following error. If you do, it can be safely ignored:

    ```PowerShell
    Starting PowerBI Artifact Provisioning
    Invoke-WebRequest : The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
    At C:\labfiles\data-engineering-ilt-deployment\setup\04\artifacts\environment-setup\solliance-synapse-automation\solliance-synapse-automation. char:15
    + ...   $result = Invoke-WebRequest -Uri $url -Method GET -ContentType "app ...
    +                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotImplemented: (:) [Invoke-WebRequest], NotSupportedException
        + FullyQualifiedErrorId : WebCmdletIEDomNotSupportedException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

    Cannot index into a null array.
    At C:\labfiles\data-engineering-ilt-deployment\setup\04\artifacts\environment-setup\solliance-synapse-automation\solliance-synapse-automation. char:5
    +     $homeCluster = $result.Headers["home-cluster-uri"]
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : InvalidOperation: (:) [], RuntimeException
        + FullyQualifiedErrorId : NullArray
    ```

### Pause SQL pool

> **Note:** If you are **not** planning on using the Synapse workspace environment right away, follow the steps in this task to pause the SQL pool. Otherwise, you will completely deplete your Azure Pass credit in a few days.

1. Navigate to the resource group into which you deployed this environment.

2. Select the **Dedicated SQL pool** (`SQLPool01`).

    ![The SQL pool is highlighted.](media/sql-pool.png "SQLPool01")

3. Select **|| Pause** to pause the pool.

    ![The pause button is highlighted.](media/sql-pool-pause.png "Pause the SQL pool")

### Delete lab setup VM

You no longer need the deployment virtual machine and can safely delete it now.

1. Open the VM in your Azure resource group, select **Delete**, then select **Yes** when prompted.

    ![The delete button is highlighted.](media/delete-vm.png "Delete VM")
