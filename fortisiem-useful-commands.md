# FortiSIEM Useful Commands

## View/Verify License Information
```bash
phLicenseTool --verify
phLicenseTool --show
phLicenseTool --support
```

## System Information
### /opt/phoenix/bin/phShowVersion.sh
```bash
phShowVersion.sh
```

## View FortiSIEM System Services Status
```bash
phstatus
```

## Start/Stop System Services
### All Services
```bash
phtools --start all
phtools --stop all
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

## FortiSIEM Heath Check
### /opt/phoenix/bin/get-fsm-health.py
```bash
get-fsm-health.py <super-ip> -u ec2-user -k <ssh-key> -n <no._of_nodes> -o fsm-aws.txt
get-fsm-health.py <super-ip> -u <user> -p <password> -n <no._of_nodes> -o fsm-remote.txt
get-fsm-health.py -local -o fsm-local.txt
```


## Collect Back-end Logs for Fortinet Support
### /opt/phoenix/bin/phziplogs
```bash
phziplogs <directory> <no._of_days>
```
#### Example
```bash
phziplogs /tmp/support-case 2
```

## Diagnostic CLI Commands
```bash
phtools --stop all                  # Stop the ph processes
phtools --start all                 # Start the ph processes
phtools --change-log DEBUG          # Change ph process logging level from INFO to DEBUG
killall -9 ph<process-name>         # Restart a ph process
killall -10 ph<process-name>        # Turn on DEBUG mode for a ph process
killall -10 ph<process-name>        # Turn off DEBUG mode for a ph process
```


---------------------------------------------------------------------------------------------------------------------
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

## Check WMI Connectivity
### /opt/phoenix/bin/checkWMIMonitorability
```bash
checkWMIMonitorability <ip> <domain> <user> '<password>' <output_file>
```
#### Example
```bash
checkWMIMonitorability 1.1.1.1 pandora wmiuser 'WMIPassword' wmitest.txt
```

## Check WMI Permissions and Access
### /opt/phoenix/bin/wmic
```bash
wmic -U <domain>/<user> //<ip> <query>
```
#### Example
```bash
wmic -U pandora/wmiuser //1.1.1.1 "select Domain from Win32_ComputerSystem"
```
