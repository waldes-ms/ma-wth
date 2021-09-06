# Challenge 2: Modify the application

## Overview

In this exercise, you will change the type of VM from Windows Server to Ubuntu, add a storage account to your application, and redeploy the application.

## Modify the ARM template

1.	Open the ARM template (mainTemplate.json) for editing.
2.	In the parameters section add the following 2 parameters. This will be populated by outputs of the `createUiDefinition.json`, which you will modify later.

```json
    "storageAccountPrefix": {
        "type": "string",
        "metadata": {
            "description": "Assign a prefix for the Storage account"
        }
    },
    "storageAccountType": {
        "type": "string",
        "metadata": {
            "description": "Assign Storage account type"
        }
    }
```

3.	In the variables section, change the `osType` variable to create an Ubuntu image instead of Windows. To do so, make the osType variable JSON look like the following.

```json
    "osType": {
        "imageOffer": "UbuntuServer",
        "imageSku": "18.04-LTS",
        "imagePublisher": "Canonical"
    }
```

4.	Insert the below JSON as the last resource to be created in the ARM template. This JSON specifies that the ARM template will create a plain storage account.

```json
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountPrefix')]",
            "apiVersion": "2021-04-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {
                "allowBlobPublicAccess": true
            }
        }
```

## Modify createUIDefinition.json

Here you will update the `createUiDefinition.json` file to create an installation experience that prompts the user for details needed to create an Azure Storage account. The values gathered will be input into the ARM template as parameters at install time.

> __Note:__ At any point in editing the `createUiDefinition.json` file, consider using the [Create UI Definition](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) sandbox in the Azure portal to check your sytax and functionality.

1.	Open the UI Definition file (createUiDefinition.json) for editing.
2.	Add the following JSON as the last member of the steps array.

```json
            {
                "name": "storageBlade",
                "label": "Storage",
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccount",
                        "type": "Microsoft.Storage.StorageAccountSelector",
                        "label": "Storage account",
                        "toolTip": "Click 'Create New' to set the unique name and properties of the new Storage account",
                        "defaultValue": {
                            "name": "storageaccount01",
                            "type": "Standard_LRS"
                        },
                         "options": {
                            "hideExisting": true
                        },
                        "visible": true
                    }
                ]
            }

```

3.	Add the following 2 lines to the end of the outputs section. These are the parameter names you included in the ARM template earlier.

```json
            "storageAccountName": "[steps('storageBlade').storageAccount.name]",
            "storageAccountType": "[steps('storageBlade').storageAccount.type]"
```

## Use the ARM template test toolkit

In this section, you will download a tool used in testing ARM templates and use it to test your mainTemplate.json and createUiDefinition.json files.

1.	Follow the instructions in the linked article below, using `../ma-wth/Student/Resources/Challenge 1` as the target folder.

    [ARM template test toolkit - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/test-toolkit)

2.	Fix any errors reported by the tool.

## Publish the new application

In this last exercise, you will package your new application and publish it. After that, you’ll check to see that the VM you deployed is up and running.

## Create the AMA package

1.	Delete the old app.zip file
2.	ZIP the 2 files into a new file named app.zip
3.	Delete the old app.zip from blob storage and upload the new one

## Create a Service Catalog offer

Create a new Service Catalog Managed Application definition, just as you did before. Use a different name for this definition.

## Install the application

Install the app just as you did before, this time using a different name for the application.

## Check that the machine is running

After the installation completes, the VM you created will be left in a running state. 

Since it is an Ubuntu server, you would need to SSH into the box to manage it. You are free to SSH into the machine if you know how, but you can also just see in the menu bar on the VM’s portal page that it is running.

**Shutdown the machine**.

## Additional Resources

1.	[Azure Managed Applications documentation](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/)
2.	[Publish service catalog managed app - Azure Managed Applications](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/publish-service-catalog-app?tabs=azure-powershell)
3.	[Use Azure portal to deploy service catalog app - Azure Managed Applications](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/deploy-service-catalog-quickstart)

