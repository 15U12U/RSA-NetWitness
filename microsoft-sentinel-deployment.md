# Microsoft Sentinel Deployment Guide

## Onboarding Azure Arc-enable Servers
### Prerequisites [^1]
#### Azure resource providers
To use Azure Arc-enabled servers, the following [Azure resource providers](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types) must be registered in your subscription:
- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**
- **Microsoft.HybridConnectivity**
- **Microsoft.AzureArcData** (if you plan to Arc-enable SQL Servers)

##### Azure Portal
```ps1
Subscriptions > "Select {Subscription Name}" > Resource providers > "Select {Resource provider}" > Register
```

##### Azure PowerShell
```ps1
Connect-AzAccount
Set-AzContext -SubscriptionId [Subscription Name]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridConnectivity
Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData
```

##### Azure CLI
```ps1
az account set --subscription "{Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
az provider register --namespace 'Microsoft.HybridConnectivity'
az provider register --namespace 'Microsoft.AzureArcData'
```

#### Permissions
You'll need the following Azure built-in roles for different aspects of managing connected machines:

| Action                 | Role                                                                                                                                                                                                                                                                    | Assignment     |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------- |
| Onboard            | [Azure Connected Machine Onboarding](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-onboarding)/[Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#contributor) | Subscription/Resource Group |
| Read/Modify/Delete | [Azure Connected Machine Resource Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-resource-administrator)                                                                                         | Subscription/Resource Group |


### Network Requirements [^2]
#### URLs

| Agent resource                                                               | Description                                                          | When required                                                                                                                                  | Endpoint used with private link                                      |
| :--------------------------------------------------------------------------- | :------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------- |
| `aka.ms`                                                                     | Used to resolve the download script during installation              | At installation time, only                                                                                                                     | Public                                                               |
| `download.microsoft.com`                                                     | Used to download the Windows installation package                    | At installation time, only                                                                                                                     | Public                                                               |
| `packages.microsoft.com`                                                     | Used to download the Linux installation package                      | At installation time, only                                                                                                                     | Public                                                               |
| `login.windows.net`                                                          | Microsoft Entra ID                                                   | Always                                                                                                                                         | Public                                                               |
| `login.microsoftonline.com`                                                  | Microsoft Entra ID                                                   | Always                                                                                                                                         | Public                                                               |
| `pas.windows.net`                                                            | Microsoft Entra ID                                                   | Always                                                                                                                                         | Public                                                               |
| `management.azure.com`                                                       | Azure Resource Manager - to create or delete the Arc server resource | When connecting or disconnecting a server, only                                                                                                | Public, unless a resource management private link is also configured |
| `*.his.arc.azure.com`                                                        | Metadata and hybrid identity services                                | Always                                                                                                                                         | Private                                                              |
| `*.guestconfiguration.azure.com`                                             | Extension management and guest configuration services                | Always                                                                                                                                         | Private                                                              |
| `guestnotificationservice.azure.com`, `*.guestnotificationservice.azure.com` | Notification service for extension and connectivity scenarios        | Always                                                                                                                                         | Public                                                               |
| `azgn*.servicebus.windows.net`                                               | Notification service for extension and connectivity scenarios        | Always                                                                                                                                         | Public                                                               |
| `*.servicebus.windows.net`                                                   | For Windows Admin Center and SSH scenarios                           | If using SSH or Windows Admin Center from Azure                                                                                                | Public                                                               |
| `*.waconazure.com`                                                           | For Windows Admin Center connectivity                                | If using Windows Admin Center                                                                                                                  | Public                                                               |
| `*.blob.core.windows.net`                                                    | Download source for Azure Arc-enabled servers extensions             | Always, except when using private endpoints                                                                                                    | Not used when private link is configured                             |
| `dc.services.visualstudio.com`                                               | Agent telemetry                                                      | Optional, not used in agent versions 1.24+                                                                                                     | Public                                                               |
| `san-af-<region>-prod.azurewebsites.net`                                     | Azure Arc data processing service                                    | For Azure Arc-enabled SQL Server. The Azure Extension for SQL Server uploads inventory and billing information to the data processing service. | Public                                                               |

[^1]: [Azure Arc-enable Servers - Prerequisites](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites)


-
