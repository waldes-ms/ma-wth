# Challenge 1: Deploy your first Azure Managed Application with Service Catalog

## Introduction

In this challenge, you will deploy a Managed Application in the Azure portal. You’ll do this using an Azure service called the “Service Catalog.”

## Description

1.	Clone this repository to your local machine.

    [A workshop for Azure Managed Applications](https://github.com/mabotova/ma-wth)

2.	Create a ZIP file called `app.zip` that contains these 2 files at the root of the ZIP.

- ma-wth / Student / Resources / Challenge 1 / createUiDefinition.json
- ma-wth / Student / Resources / Challenge 1 / mainTemplate.json

    **Note:** If you are using a Mac, it is important you don’t pick up any hidden files from your file system.

3. In Azure portal create a new resource group, storage account and a storage container. Upload the app.zip file to the storage container. Use the following valies for storage account properties:
    - Storage account name > Some unique value
    - Basics > Replication > Locally Redundant Storage (LRS)
    - Networking > Network Connectivity > Public endpoint (all networks)
    - For the rest > Leave the defauls

4. Create a **Service Catalog Managed Application Definition** in the same resource group. 

5. Navigate to Service Catalog Managed Application Definition blade and create a **Service Catalog Managed Application** from it in a separate resource group (look for "Deploy from definition" button).

6. Once the deployment is complete connect to the virtual machine via RDP. After verifing the remote connection, you may shut down the machine so you won’t be charged for VM uptime.

## Success Criteria

1. You have a resource group with a storage account holding a ZIP file with your application JSONs.
2. You have a resource group with your Managed Application deployed in Service Catalog.
3. You can successfully RDP into your new virtual machine.

## Learning Resources

- <https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal>
- <https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-azure-managed-applications-service-catalog/>