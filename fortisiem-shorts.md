# FortiSIEM Useful Info and Commands

## Default Credentials 
### 'root' Password - OS
| Username | Password      |
| :------- | :------------ | 
| root     | ProspectHills |

### 'admin' Password - Web UI
| Username | Password |
| :------- | :------- | 
| admin    | admin*1  |
---

## FortiSIEM Supervisor/Supervisor Follower/Worker/Collector/Manager Installation Script
```bash
/usr/local/bin/configFSM.sh
```
### Install Log path
```bash
/usr/local/fresh-install/logs/ansible.log
```
---

## Proxy configuration for Internet Access
### Create `proxy.sh` file
```bash
vim /etc/profile.d/proxy.sh
```
### Add the following content and edit the values
```bash
PROXY_URL="<proxy-ip-or-hostname>:<proxy-port>"
export http_proxy="$PROXY_URL"
export https_proxy="$PROXY_URL"
export ftp_proxy="$PROXY_URL"
export no_proxy="127.0.0.1,localhost"
```

### Execute the script file in current shell environment
```bash
source /etc/profile.d/proxy.sh
```

### URLs to be permitted via proxy
```
https://os-pkgs.fortisiem.fortinet.com
https://os-pkgs-cdn.fortisiem.fortinet.com
https://os-pkgs-r8.fortisiem.fortinet.com
https://update.fortiguard.net
```
---

## FortiSIEM Binaries
```bash
/opt/phoenix/bin/
```

## View/Verify License Information
```bash
phLicenseTool --verify
phLicenseTool --show
phLicenseTool --support
```

## System Information
```bash
phshowVersion.sh
```

## PostgreSQL Log
```bash
cat /cmdb/data/pg_log/postgresql.log
```
---
## View FortiSIEM System Services Status
```bash
phstatus
```

## Start/Stop System Services
### All Services
```bash
phtools --start all
phtools --stop all
phxctl [start|stop|reboot]
```

### Specific Service
```bash
phtools --start ph<process-name>
phtools --stop ph<process-name>
```

#### Example
```bash
phtools --start phDataPurger
phtools --stop phDataPurger
```
---
## FortiSIEM Heath Check
```bash
get-fsm-health.py <super-ip> -u ec2-user -k <ssh-key> -n <no._of_nodes> -o fsm-aws.txt
get-fsm-health.py <super-ip> -u <user> -p <password> -n <no._of_nodes> -o fsm-remote.txt
get-fsm-health.py -local -o fsm-local.txt
```

## Register/Renew a FortiSIEM Collector Node
```bash
phProvisionCollector --add <user> '<password>' <Super IP or Hostname> <Organization> <CollectorName>
phProvisionCollector --update <user> '<password>' <Super IP or Hostname> <Organization> <CollectorName>
```
---

## Remove a FortiSIEM Collector Node
### Log into PostgreSQL Database
```bash
psql -U phoenix phoenixdb
```
### Update the Table
```psql
update ph_sys_collector set natural_id = '' where name = '<collector name>'
```
---

## Collect Back-end Logs for Fortinet Support
```bash
phziplogs <directory> <no._of_days>
```
#### Example
```bash
phziplogs /tmp/support-case 2
```
---

## Diagnostic CLI Commands
```bash
phtools --change-log DEBUG          # Change ph process logging level from INFO to DEBUG
killall -9 ph<process-name>         # Restart a ph process
killall -10 ph<process-name>        # Turn on DEBUG mode for a ph process
killall -10 ph<process-name>        # Turn off DEBUG mode for a ph process
```

## Rebuild App Server
```bash
cd /opt/phoenix/deployment/
./deploy-fresh.sh phoenix.ear --force
```
---

## Check SNMP Connectivity
### SNMPv1/v2C
```bash
snmpwalk -c <community_string> -v <version> <ip>
```
#### Example
```bash
snmpwalk -c public - v 2c 1.1.1.1
```
### SNMPv3

| Parameter     | Value                                |
| :------------ | :----------------------------------- |
| securityLevel | noAuthNoPriv / authNoPriv / authPriv |
| authProtocol  | MD5 / SHA                            |
| privProtocol  | AES / DES                            |

```bash
snmpwalk -v3 -u <user> -l <securityLevel> -a <authProtocol> -A '<authKey>' -x <privProtocol> -X '<privKey>'
```

#### Example
```bash
snmpwalk -v3 -u snmpv3user -l authPriv -a SHA -A 'SHAPassword' -x AES -X 'AESPassword'
```
---
## Check WMI Connectivity
```bash
checkWMIMonitorability <ip> <domain> <user> '<password>' <output_file>
```
#### Example
```bash
checkWMIMonitorability 1.1.1.1 pandora wmiuser 'WMIPassword' wmitest.txt
```
---
## Check WMI Permissions and Access
```bash
wmic -U <domain>/<user> //<ip> <query>
```
#### Example
```bash
wmic -U pandora/wmiuser //1.1.1.1 "select Domain from Win32_ComputerSystem"
```
---

## FortiSIEM Collector Event Buffer
Collectors can buffer events in case events cannot be uploaded fast enough to Worker(s) or Supervisor nodes or they are unavailable for a period of time. Events are stored in compressed format in the following location `/opt/phoenix/cache/parser/events` before being sent to Worker(s) or Supervisor nodes. By default, a maximum of 10K files are stored and each file has a maximum uncompressed file size of 10MB.

### Increase Collector Event Buffer Size
When the number of files reaches the limit, events are dropped by the Collector. To change this limit, modify the following line in `/opt/phoenix/config/phoenix_config.txt`.
```
[BEGIN phEventPackager]
max_num_event_files=10000
```
In case your Collectors are forwarding events to a 3rd party server, there is a similar limit of 10K files of events stored in `/opt/phoenix/cache/parser/fwd`. When this limit is reached, newer events are not forwarded. To change this limit, modify the following line in `/opt/phoenix/config/phoenix_config.txt`.
```
[BEGIN phEventForwarder]
max_num_fwd_files=10000
```
> [!NOTE]
> You may need to increase the size of `/opt` disk if you significantly increase the number of files stored. For configuration information on how to increase your `/opt` disk size, see Collector with Different OPT Disk Sizes in the ESX Install Guide.
