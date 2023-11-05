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
| :--------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------- |
| Onboard                | [Azure Connected Machine Onboarding](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-onboarding) / [Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#contributor) | Resource Group |
| Read / Modify / Delete | [Azure Connected Machine Resource Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-resource-administrator)                                                                                       | Resource Group |


[^1]: [Azure Arc-enable Servers - Prerequisites](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites)


-
