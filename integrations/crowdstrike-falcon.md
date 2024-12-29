# CrowdStrike Falcon SIEM Connector - Configuration Guide
## 1. Create an API Client for the CrowdStrike Falcon Streaming API
> Log in to the [CrowdStrike Falcon](https://falcon.crowdstrike.com/) Web Console, and go to the **Support** > **API Clients and Keys**

> Click **Create API client** under **OAuth2 API Clients**

> Enter the **CLIENT NAME** and insert a **DESCRIPTION** if required

> Set the **API SCOPES** as follows and Click **CREATE** 

| Scope         | Read                    |
| :------------ | :---------------------: |
| Alerts        | :ballot_box_with_check: |
| Detections    | :ballot_box_with_check: |
| Incidents     | :ballot_box_with_check: |
| Event streams | :ballot_box_with_check: |

> [!IMPORTANT]
> Copy the **Client ID** and **Secret** to a safe place for later use

## 2. Download the CrowdStrike Falcon SIEM Connector Installation Package
> Log in to the [CrowdStrike Falcon](https://falcon.crowdstrike.com/) Web Console, and go to the **Support** > **Tool Downloads**

> Select the appropriate 'Falcon SIEM Connector' installation package and Click on the :arrow_down: button under **Actions**

> [!NOTE]
> Supported OS (64-bit only)
> - CentOS/RHEL 7/8
> - Ubuntu 20

## 3. Install the Falcon SIEM Connector
> Copy the Installation package downloaded in the previous step to the host that will be running the Falcon SIEM Connector

### CentOS/RHEL
```bash
sudo rpm -Uvh [InstallationPackageName]
```

### Ubuntu
```bash
sudo dpkg -i [InstallationPackageName]
```

> [!NOTE]
> The Falcon SIEM Connector installs in the `/opt/crowdstrike/` directory by default




