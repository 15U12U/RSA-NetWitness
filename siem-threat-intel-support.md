# SIEM Threat Intel Support

| Threat Feed              	| FortiSIEM          | Wazuh              | Type                      | URL                                                               |
| :------------------------ | :----------------: | :----------------: |:-------------------------:| :---------------------------------------------------------------- |
| Cisco Talos              	| :heavy_check_mark: | :heavy_check_mark:	| IP                        | http://www.talosintelligence.com/documents/ip-blacklist           |
| AlienVault OTX           	| :heavy_check_mark: | :heavy_check_mark:	| IP/Domain/URL<br>IP<br>IP | https://otx.alienvault.com/taxii/discovery<br>https://reputation.alienvault.com/reputation.data<br>https://reputation.alienvault.com/reputation.generic |
| Abuse.ch                 	|    ✔<br>✔<br>❌<br>✔    	|   ✔<br>✔<br>❌<br>✔   	| Hash<br>IP<br>Hash<br>URL 	| https://bazaar.abuse.ch/export/csv/full/<br>https://feodotracker.abuse.ch/downloads/ipblocklist_recommended.txt<br>https://sslbl.abuse.ch/blacklist/sslblacklist.csv<br>https://urlhaus.abuse.ch/downloads/csv_online/ |
| IBM X-Force              	|     ✔     	|   ✔   	|                           	| ✔                                      |
| TOR Exit-Nodes           	|     ✔     	|   ✔   	|                           	| ✔                                      |
| Darklist                 	|     ✔     	|   ✔   	|                           	| ✔                                      |
| PhishTank                	|     ✔     	|   ✔   	| URL                       	| http://data.phishtank.com/data/online-valid.csv |
| Openphish                	|     ✔     	|   ✔   	| URL                       	| https://openphish.com/feed.txt |
| Blocklist                	|     ✔     	|   ✔   	| IP                        	| https://lists.blocklist.de/lists/all.txt |
| Vxvault                  	|     ❌     	|   ✔   	|                           	| ✔                                      |
| CI Bad Guys              	|     ❌     	|   ✔   	|                           	| ✔                                     |
| Greensnow Blacklist      	|     ❌     	|   ✔   	|                           	| ✔                                      |
| NoVirusThanks IPSpamList 	|     ❌     	|   ✔   	|                           	| ✔                                      |
| Binary Defense           	|     ✔     	|   ✔   	| IP                        	| https://www.binarydefense.com/banlist.txt |
| Bot Scout                	|     ✔     	|   ✔   	| IP                        	| https://raw.githubusercontent.com/firehol/blocklist-ipsets/master/botscout_1d.ipset |
| Bruteforceblocker        	|     ✔     	|   ✔   	| IP                        	| http://danger.rulez.sk/projects/bruteforceblocker/blist.php |




| External Lookup          	| FortiSIEM 	| Wazuh 	|  Type                       	| URL                                   	|
|--------------------------	|:---------:	|:-----:	|:--------------------------:|----------------|
| Cisco Talos              	|     ✔     	|   ✔   	|                           	| https://talosintelligence.com/reputation_center/lookup?search='ip/domain |
| AlienVault OTX           	|     ✔     	|   ✔   	|                           	| https://otx.alienvault.com/api/v1/indicators/IPv4/'ip'/general  |
| Abuse.ch                 	|     ✔     	|   ✔   	|                           	| ✔                                       |
| IBM X-Force              	|     ✔     	|   ✔   	|                           	| ✔                                        |
| TOR Exit-Nodes           	|     ✔     	|   ✔   	|                           	| ✔                                       |
| darklist.de              	|     ✔     	|   ✔   	|                           	| ✔                                       |
| PhishTank                	|     ✔     	|   ✔   	|                           	| ✔                                       |
| openphish                	|     ❌     	|   ✔   	|                           	| ✔                                       |
| blocklist.de             	|     ❌     	|   ✔   	|                           	| ✔                                       |
| vxvault.net              	|     ❌     	|   ✔   	|                           	| ✔                                       |
| CI Bad Guys              	|     ❌     	|   ✔   	|                           	| ✔                                       |


## FortiSIEM
### AlienVault OTX

| Parameter    | Value                      |
| :----------- | :------------------------- |
| URL          | https://otx.alienvault.com |
| Username     | <API-KEY>                  |
| Password     |                            |
| Plugin Class | <Plugin-Class>             |
| Data Format  | STIX-TAXII                 |
| Collection   | user_AlienVault            |
| Data Update  | Full                       |


#### Plugin Classes
- com.accelops.service.threatfeed.impl.OTXMalwareDomainUpdateService
- com.accelops.service.threatfeed.impl.OTXMalwareIPUpdateService
- com.accelops.service.threatfeed.impl.OTXMalwareUrlUpdateService
- com.accelops.service.threatfeed.impl.OTXMalwareHashUpdateService

