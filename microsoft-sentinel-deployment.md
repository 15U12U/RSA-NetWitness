# Microsoft Sentinel Deployment Guide

## Onboarding Azure Arc-enable Servers
### 1. Prerequisites [^1]
#### 1.1. Azure Resource Providers
To use Azure Arc-enabled servers, the following [Azure resource providers](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types) must be registered in your subscription:
- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**
- **Microsoft.HybridConnectivity**
- **Microsoft.AzureArcData** (if you plan to Arc-enable SQL Servers)

##### 1.1.1. Azure Portal
```ps1
Subscriptions > "Select {Subscription Name}" > Resource providers > "Select {Resource provider}" > Register
```

##### 1.1.2. Azure PowerShell
```ps1
Connect-AzAccount
Set-AzContext -SubscriptionId [Subscription Name]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridConnectivity
Register-AzResourceProvider -ProviderNamespace Microsoft.AzureArcData
```

##### 1.1.3. Azure CLI
```ps1
az account set --subscription "{Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
az provider register --namespace 'Microsoft.HybridConnectivity'
az provider register --namespace 'Microsoft.AzureArcData'
```

#### 1.2. Permissions
You'll need the following Azure built-in roles for different aspects of managing connected machines:

| Action                 | Role                                                                                                                                                                                                                                                                    | Assignment     |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------- |
| Onboard            | [Azure Connected Machine Onboarding](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-onboarding)/[Contributor](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#contributor) | Subscription/Resource Group |
| Read/Modify/Delete | [Azure Connected Machine Resource Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-connected-machine-resource-administrator)                                                                                         | Subscription/Resource Group |

#### 1.3. Supported Operating Systems

> [!IMPORTANT]
> - Azure Arc supports the following Windows and Linux operating systems. Only x86-64 (64-bit) architectures are supported. The Azure Connected Machine agent does not run on x86 (32-bit) or ARM-based architectures.
> - The Azure Connected Machine agent can't currently be installed on systems hardened by the Center for Information Security (CIS) Benchmark.

- Amazon Linux 2 and 2023
- Azure Linux (CBL-Mariner) 1.0, 2.0
- Azure Stack HCI
- CentOS Linux 7 and 8
- Debian 10, 11, and 12
- Oracle Linux 7 and 8
- Red Hat Enterprise Linux (RHEL) 7, 8 and 9
- Rocky Linux 8
- SUSE Linux Enterprise Server (SLES) 12 SP3-SP5 and 15
- Ubuntu 16.04, 18.04, 20.04, and 22.04 LTS
- Windows 10, 11 (see [client operating system guidance](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites#client-operating-system-guidance))
- Windows IoT Enterprise
- Windows Server 2008 R2 SP1, 2012, 2012 R2, 2016, 2019, and 2022
  - Both Desktop and Server Core experiences are supported
  - Azure Editions are supported on Azure Stack HCI

#### 1.4. Software Requirements
##### 1.4.1. Windows
- Windows Server 2008 R2 SP1 requires PowerShell 4.0 or later. Microsoft recommends running the latest version, [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

##### 1.4.2. Linux
- systemd
- wget (to download the installation script)
- openssl
- gnupg (Debian-based systems, only)

### 2. Network Requirements [^2]
#### 2.1. URLs

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

#### 2.2. Proxy Configuration
On Windows, the Azure Connected Machine agent will first check the proxy.url agent configuration property (starting with agent version 1.13), then the system-wide HTTPS_PROXY environment variable to determine which proxy server to use. If both are empty, no proxy server is used, even if the default Windows system-wide proxy setting is configured.

Microsoft recommends using the agent-specific proxy configuration instead of the system environment variable.

> [!NOTE]  
> - Azure Arc-enabled servers doesn't support using proxy servers that require authentication, TLS (HTTPS) connections, or a [Log Analytics gateway](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/gateway) as a proxy for the Connected Machine agent.  
> - You do not need to restart any services when reconfiguring the proxy settings with the azcmagent config command.

##### 2.2.1. View Proxy Configuration
- **Agent Specific**
```bash
azcmagent show
```

##### 2.2.2. Setup Proxy Configuration
- **Agent Specific**
```bash
azcmagent config set proxy.url "http://ProxyServerFQDN:port"
```

- **System Specific (Windows)**
```ps1
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("HTTPS_PROXY", "http://ProxyServerFQDN:port", "Machine")
$env:HTTPS_PROXY = [System.Environment]::GetEnvironmentVariable("HTTPS_PROXY", "Machine")
# For the changes to take effect, the agent services need to be restarted after the proxy environment variable is set.
Restart-Service -Name himds, ExtensionService, GCArcService
```

- **System Specific (Linux)**
```bash
sudo /opt/azcmagent/bin/azcmagent_proxy add "http://ProxyServerFQDN:port"
```

##### 2.2.3. Verify Proxy Configuration
- **Agent Specific**
```bash
azcmagent config get proxy.url
```

##### 2.2.4. Remove Proxy Configuration
- **Agent Specific**
```bash
azcmagent config clear proxy.url
```

- **System Specific (Windows)**
```ps1
[Environment]::SetEnvironmentVariable("HTTPS_PROXY", $null, "Machine")
$env:HTTPS_PROXY = [System.Environment]::GetEnvironmentVariable("HTTPS_PROXY", "Machine")
# For the changes to take effect, the agent services need to be restarted after the proxy environment variable removed.
Restart-Service -Name himds, ExtensionService, GCArcService
```

- **System Specific (Linux)**
```bash
sudo /opt/azcmagent/bin/azcmagent_proxy remove
```

##### 2.2.5. Proxy bypass for Private Endpoints

| Proxy bypass value | Affected endpoints                                                                               |
| :----------------- | :----------------------------------------------------------------------------------------------- |
| **AAD**            | `login.windows.net`, `login.microsoftonline.com`, `pas.windows.net`                              |
| **ARM**            | `management.azure.com`                                                                           |
| **Arc**            | `his.arc.azure.com`, `guestconfiguration.azure.com` , `san-af-<location>-prod.azurewebsites.net` |

###### 2.2.5.1. Setup Proxy Bypass
```bash
azcmagent config set proxy.bypass "Arc"
azcmagent config set proxy.bypass "ARM,Arc"
```

###### 2.2.5.2. Remove Proxy Bypass
```bash
azcmagent config clear proxy.bypass
```


### 3. Troubleshooting [^3]
#### 3.1. Agent Verbose Log Location
##### 3.1.1. Windows
```ps1
%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log
```
##### 3.1.2. Linux
```bash
/var/opt/azcmagent/log/azcmagent.log
```

#### 3.2. Enable Agent Verbose Log
##### 3.2.1. Windows
```ps1
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```
##### 3.2.2. Linux
```bash
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```


[^1]: [Azure Arc-enable Servers - Prerequisites](https://learn.microsoft.com/en-us/azure/azure-arc/servers/prerequisites)
[^2]: [Azure Arc-enable Servers - Network Requirements](https://learn.microsoft.com/en-us/azure/azure-arc/servers/network-requirements)
[^3]: [Azure Arc-enable Servers - Troubleshooting](https://learn.microsoft.com/en-us/azure/azure-arc/servers/troubleshoot-agent-onboard)
