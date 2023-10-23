# FortiSIEM Useful Info and Commands

## Default 'root' Password - OS
| Username | Password      |
| :------- | :------------ | 
| root     | ProspectHills |

## Default 'admin' Password - Web UI
| Username | Password |
| :------- | :------- | 
| admin    | admin*1  |

## FortiSIEM Supervisor/Supervisor Follower/Worker/Collector/Manager Installation Script
```bash
/usr/local/bin/configFSM.sh
```

## FortiSIEM Binaries
```bash
/opt/phoenix/bin/
```
---
## View/Verify License Information
```bash
phLicenseTool --verify
phLicenseTool --show
phLicenseTool --support
```
---
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
---
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
---
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
---

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
