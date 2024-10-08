---
title: "Migrating Autopilot Hashes With Azure Tables"
date: "2024-10-08T11:45:27+11:00"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "sysadmin"
  - "technology"
  - "azure"
  - "intune"
  - "powershell"
  - "autopilot"
  - "tables"
coverImage:
draft: true
---

## The problem

As part of a project to consolidate 3 organizations into 1 new organization, we decided as part of the IT consolidation, end user would go through a reset and re-register process to migrate their device from either BYOD or old environment corporate (Intune) managed to new environment corporate (Intune) managed. This process would occur over a short window of time (such as a weekend) for all users.

While sounding simple enough on paper, there are some snags along the way that will need to be addressed. One big one being how do we migrate the AutoPilot v1 device hashes from the device's old Intune environment to the new one? Microsoft doesn't allow you to extract this enmasse from Intune currently. This leaves us with the only option to touch each device individually.

Sounds like a job requiring a bit of automation no?

## What are we working with?

One thing Microsoft do provide is a [ready-made PowerShell script](https://www.powershellgallery.com/packages/Get-WindowsAutopilotInfo/3.9) to extract the device hash from a device. Okay, this is a starting base to work with.

We also know that Intune likes to import Autopilot device hashes as a [particularly structured CSV file](https://learn.microsoft.com/en-us/autopilot/add-devices#ensure-that-the-csv-file-meets-requirements).

So we know how to output what we need and what are the import requirements, know we need to build out some scale in the process to collect the output and input in minimal steps (given the short window of time).

## Solutioning time

Since we are already dealing with PowerShell and data structured in table form, sounds like the perfect excuse to use Azure Tables! From a 10,000ft view, we need to build something that will look like the following:

<div style="background-color:white; padding: 20px">
{{< mermaid >}}
flowchart BT
subgraph Org1 [Old Organisation 1]
EU1[End user device 1] --> PS1[/Get-WindowsAutopilotInfo.ps1/]
end
PS1 --> DB[(Azure Table)]
subgraph Org2 ["`Old Organisation *n*`"]
EU2["`End user device *n*`"] --> PS2[/Get-WindowsAutopilotInfo.ps1/]
end
PS2 --> DB[(Azure Table)]
subgraph Org3 [New Organisation]
DB --> IA[Intune Autopilot]
end
{{< /mermaid >}}
</div>

## Build some infra

Okay, we have declared that Azure Tables shall provide the central storage of our device hashes for import into a new organisations Intune. Let's quickly build a Storage Account  associated Table and output a service level SAS key to be used by our device-level PowerShell to send up the device hash by flexing our Bicep skills.

Save the below as a ```.bicep``` file.

```bicep
@maxLength(24)
@minLength(3)
@description('Specifies the name of the Azure Storage account.')
param storageAccountName string = 'autopilot${uniqueString(resourceGroup().id, deployment().name)}'

@description('Specifies the name of the blob container.')
param tableName string = 'autopilotinfo'

param tableSasExpiry string = '2025-01-01T00:00:00Z'

@description('Specifies the location in which the Azure Storage resources should be deployed.')
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}

resource tableServices 'Microsoft.Storage/storageAccounts/tableServices@2023-05-01' = {
  parent: sa
  name: 'default'
}

resource table 'Microsoft.Storage/storageAccounts/tableServices/tables@2023-05-01' = {
  parent: tableServices
  name: tableName
}

var sasConfig = {
    canonicalizedResource: '/table/${sa.name}/${table.name}'
    signedPermission: 'rau'
    signedExpiry: tableSasExpiry
    signedProtocol: 'https'
    keyToSign: 'key2'
}

output sasToken string = sa.listServiceSas(sa.apiVersion, sasConfig).serviceSasToken
```

Create the necessary resouce group, for example

```bash
az group create --name Autopilot-RG --location "Australia East"
```

Now let's deploy our bicep file (in this example, the above bicep was saved as *deployaztable.bicep*)

```bash
az deployment group create \
  --name AutopilotTableDeployment \
  --resource-group Autopilot-RG \
  --template-file deployaztable.bicep \
  --output none
```
and note in the outputs section our service-level SAS key to hit the table with later.

```json
    "outputs": {
      "sasToken": {
        "type": "String",
        "value": "sv=2015-04-05&spr=https&se=2025-01-01T00%3A00%3A00.0000000Z&sp=rau&tn=autopilotinfo&sig=VG6pu3gpLNPMqPJGxopOntXX%2BgMWzQJdCRxqCr6EN4A%3D"
      }
    },
```

{{< alert "circle-info" >}}
The output gets a bit messed up, specifically, any time codes within the SAS key. Make sure to convert those **%3** in the ```se=``` section to **:**
{{< /alert >}}

## Extract the hash