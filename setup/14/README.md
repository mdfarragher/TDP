# Module 14 Lab setup

1. Click on the following button to open the ARM template in the Azure portal:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsolliancenet%2Fmicrosoft-data-engineering-ilt-deploy%2Fmain%2Fsetup%2F14%2fmodule-14-template.json" target="_blank"><img src="https://aka.ms/deploytoazurebutton" /></a>

2. On the **Custom deployment** form fill in the fields described below.

   - **Subscription**: Select your Azure Pass subscription.
   - **Resource group**: Create a new resource group named `techionista-dp203-m14`.
   - **Region**: Select `West Europe`.
   - **Unique suffix**: used to generate unique names during deployment. Type a combination of your initials and the current date (for example `mdf1708`), but make sure your suffix does not exceed a **maximum** length of **9 characters**). Also make sure you follow correct Azure [Resource naming](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming) conventions.
   - **SQL Administrator Login Password**: Provide a strong password for the SQLPool that will be created as part of your deployment. [Visit here](https://docs.microsoft.com/en-us/sql/relational-databases/security/password-policy?view=sql-server-ver15#password-complexity) to read about password rules in place.

   > **Important:** You will need your password during the lab. Save your password in Notepad or similar text editor so you can reference it during the lab.

3. Select the **Review + create** button, then **Create**. The provisioning of your deployment resources will take approximately 10 minutes. **Wait** until provisioning successfully completes.

    > **Note**: You may experience a deployment step failing in regards to Role Assignment. This error may safely be ignored.
