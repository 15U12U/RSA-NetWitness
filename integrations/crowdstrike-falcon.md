# CrowdStrike Falcon SIEM Connector - Configuration Guide
## 1. Create an API Client for the CrowdStrike Falcon Streaming API

1.1. Log in to the [CrowdStrike Falcon](https://falcon.crowdstrike.com/) Web Console, and go to the _**Support > API Clients and Keys**_

1.2. Click _**'Create API client'**_ under _**OAuth2 API Clients**_

1.3. Enter the _**CLIENT NAME**_ and insert a _**DESCRIPTION**_ if required

1.4. Set the _**API SCOPES**_ as follows and Click _**'CREATE'**_

| Scope         | Read                    |
| :------------ | :---------------------: |
| Event streams | :ballot_box_with_check: |

> [!IMPORTANT]
> Copy the _**'Client ID'**_ and _**'Secret'**_ to a safe place for later use  

<br>

## 2. Download the CrowdStrike Falcon SIEM Connector Installation Package
2.1. Log in to the [CrowdStrike Falcon](https://falcon.crowdstrike.com/) Web Console, and go to the _**Support > Tool Downloads**_

2.2. Select the appropriate **'Falcon SIEM Connector'** installation package and Click on the :arrow_down: button under _**Actions**_

> [!NOTE]
> Supported OS (64-bit only)
> - CentOS/RHEL 7/8
> - Ubuntu 20

<br>

## 3. Install the Falcon SIEM Connector
3.1. Copy the Installation package downloaded in _**Step 2.2.**_ to the host that will be running the Falcon SIEM Connector

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

<br>

## 4. Configure the Falcon SIEM Connector
4.1. Select the appropriate config file based on the output format supported by your SIEM 
> Ex: Microsoft Sentinel supports "CEF" format. Therefore replace the original config file `/opt/crowdstrike/etc/cs.falconhoseclient.cfg` with the preset config file for CEF `/opt/crowdstrike/etc/cs.falconhoseclient.cef.cfg`

```bash
cp /opt/crowdstrike/etc/cs.falconhoseclient.cef.cfg /opt/crowdstrike/etc/cs.falconhoseclient.cfg
```

> [!NOTE]
> Supported Output types
> - JSON (default)
> - Syslog
> - Common Event Format (CEF)
> - Log Event Extended Format (LEEF)
>
> Preset Config files for each output type can be found in the following locations

| Output Type | Config File                                         |
| :---------- | :-------------------------------------------------- |
| JSON        | `/opt/crowdstrike/etc/cs.falconhoseclient.cfg`      |
| Syslog      | `/opt/crowdstrike/etc/cs.falconhoseclient.cfg`      |
| CEF         | `/opt/crowdstrike/etc/cs.falconhoseclient.cef.cfg`  |
| LEEF        | `/opt/crowdstrike/etc/cs.falconhoseclient.leef.cfg` |

4.2. Edit the `/opt/crowdstrike/etc/cs.falconhoseclient.cfg` config file and paste the _**'Client ID'**_ and _**'Secret'**_ generated in _**Step 1.4.**_ accordingly

```bash
[Settings]
# API Client ID
client_id = <client_id>
# API Client Secret
client_secret =  <secret>
```

4.3. Confirm the output format is set to _**'syslog'**_
```bash
[Settings]
# Output formats
# Supported formats are
#   1.syslog: will output syslog format with flat key=value pairs uses the mapping configuration below.
;             Use syslog format if CEF/LEEF output is required.
#   2.json: will output raw json format received from FalconHose API (default)
output_format = syslog
```

4.4. Configure Syslog forwarding
```bash
[Syslog]
send_to_syslog_server = true
host = <siem_log_collector_ip>/<siem_log_collector_hostname>
port = 514
protocol = tcp
```

> [!NOTE]
> Available options for Syslog Configuration

| Key                   | Value             |
| :-------------------- | :---------------: |
| send_to_syslog_server | `true` \| `false` |
| protocol              | `tcp` \| `udp`    |

4.5. Start the service
```bash
systemctl enable --now cs.falconhoseclientd.service
```

