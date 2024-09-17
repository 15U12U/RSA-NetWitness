# Useful commands to get information related to Log Source EPS/Ingestion
## Palo Alto Firewall
### View Log Data Retention and Size
```bash
show system logdb-quota
```
#### Sample Output
```bash
Quotas:
              system: 4.00%, 0.609 GB Expiration-period: 0 days
              config: 4.00%, 0.609 GB Expiration-period: 0 days
               alarm: 3.00%, 0.457 GB Expiration-period: 0 days
             appstat: 4.00%, 0.609 GB Expiration-period: 0 days
         hip-reports: 1.00%, 0.152 GB Expiration-period: 0 days
             traffic: 29.00%, 4.414 GB Expiration-period: 0 days
              threat: 15.00%, 2.283 GB Expiration-period: 0 days
               trsum: 7.00%, 1.065 GB Expiration-period: 0 days
         hourlytrsum: 3.00%, 0.457 GB Expiration-period: 0 days
          dailytrsum: 1.00%, 0.152 GB Expiration-period: 0 days
         weeklytrsum: 1.00%, 0.152 GB Expiration-period: 0 days
              urlsum: 2.00%, 0.304 GB Expiration-period: 0 days
        hourlyurlsum: 1.00%, 0.152 GB Expiration-period: 0 days
         dailyurlsum: 1.00%, 0.152 GB Expiration-period: 0 days
        weeklyurlsum: 0.75%, 0.114 GB Expiration-period: 0 days
               thsum: 2.00%, 0.304 GB Expiration-period: 0 days
         hourlythsum: 1.00%, 0.152 GB Expiration-period: 0 days
          dailythsum: 1.00%, 0.152 GB Expiration-period: 0 days
         weeklythsum: 1.00%, 0.152 GB Expiration-period: 0 days
              userid: 1.00%, 0.152 GB Expiration-period: 0 days
               iptag: 1.00%, 0.152 GB Expiration-period: 0 days
   application-pcaps: 1.00%, 0.152 GB Expiration-period: 0 days
             extpcap: 1.00%, 0.152 GB Expiration-period: 0 days
  debug-filter-pcaps: 1.00%, 0.152 GB Expiration-period: 0 days
            dlp-logs: 1.00%, 0.152 GB Expiration-period: 0 days
            hipmatch: 3.00%, 0.457 GB Expiration-period: 0 days
                 gtp: 2.00%, 0.304 GB Expiration-period: 0 days
              gtpsum: 1.00%, 0.152 GB Expiration-period: 0 days
        hourlygtpsum: 0.75%, 0.114 GB Expiration-period: 0 days
         dailygtpsum: 0.75%, 0.114 GB Expiration-period: 0 days
        weeklygtpsum: 0.75%, 0.114 GB Expiration-period: 0 days
                auth: 1.00%, 0.152 GB Expiration-period: 0 days
                sctp: 0.00%, 0.000 GB Expiration-period: 0 days
             sctpsum: 0.00%, 0.000 GB Expiration-period: 0 days
       hourlysctpsum: 0.00%, 0.000 GB Expiration-period: 0 days
        dailysctpsum: 0.00%, 0.000 GB Expiration-period: 0 days
       weeklysctpsum: 0.00%, 0.000 GB Expiration-period: 0 days
          decryption: 1.00%, 0.152 GB Expiration-period: 0 days
               desum: 1.00%, 0.152 GB Expiration-period: 0 days
         hourlydesum: 0.00%, 0.000 GB Expiration-period: 0 days
          dailydesum: 0.00%, 0.000 GB Expiration-period: 0 days
         weeklydesum: 0.00%, 0.000 GB Expiration-period: 0 days
       globalprotect: 1.00%, 0.152 GB Expiration-period: 0 days

Disk usage:
traffic: Logs and Indexes: 104M Current Retention: 21 days
threat: Logs and Indexes: 24K Current Retention: 0 days
system: Logs and Indexes: 17M Current Retention: 21 days
config: Logs and Indexes: 8.3M Current Retention: 21 days
alarm: Logs and Indexes: 20K Current Retention: 0 days
trsum: Logs and Indexes: 106M Current Retention: 21 days
hourlytrsum: Logs and Indexes: 97M Current Retention: 21 days
dailytrsum: Logs and Indexes: 5.2M Current Retention: 20 days
weeklytrsum: Logs and Indexes: 948K Current Retention: 18 days
thsum: Logs and Indexes: 204K Current Retention: 0 days
hourlythsum: Logs and Indexes: 268K Current Retention: 0 days
dailythsum: Logs and Indexes: 252K Current Retention: 0 days
weeklythsum: Logs and Indexes: 40K Current Retention: 0 days
appstatdb: Logs and Indexes: 2.2M Current Retention: 21 days
userid: Logs and Indexes: 16K Current Retention: 0 days
iptag: Logs and Indexes: 16K Current Retention: 0 days
hipmatch: Logs and Indexes: 20K Current Retention: 0 days
hip-reports: Logs and Indexes:  Current Retention: 0 days
extpcap: Logs and Indexes: 16K Current Retention: 0 days
urlsum: Logs and Indexes: 204K Current Retention: 0 days
hourlyurlsum: Logs and Indexes: 268K Current Retention: 0 days
dailyurlsum: Logs and Indexes: 252K Current Retention: 0 days
weeklyurlsum: Logs and Indexes: 40K Current Retention: 0 days
gtp: Logs and Indexes: 16K Current Retention: 0 days
gtpsum: Logs and Indexes: 200K Current Retention: 0 days
hourlygtpsum: Logs and Indexes: 268K Current Retention: 0 days
dailygtpsum: Logs and Indexes: 252K Current Retention: 0 days
weeklygtpsum: Logs and Indexes: 40K Current Retention: 0 days
auth: Logs and Indexes: 16K Current Retention: 0 days
sctp: Logs and Indexes: 16K Current Retention: 0 days
sctpsum: Logs and Indexes: 200K Current Retention: 0 days
hourlysctpsum: Logs and Indexes: 8.0K Current Retention: 0 days
dailysctpsum: Logs and Indexes: 8.0K Current Retention: 0 days
weeklysctpsum: Logs and Indexes: 8.0K Current Retention: 0 days
decryption: Logs and Indexes: 16K Current Retention: 0 days
desum: Logs and Indexes: 200K Current Retention: 0 days
hourlydesum: Logs and Indexes: 8.0K Current Retention: 0 days
dailydesum: Logs and Indexes: 8.0K Current Retention: 0 days
weeklydesum: Logs and Indexes: 8.0K Current Retention: 0 days
globalprotect: Logs and Indexes: 16K Current Retention: 0 days
application: Logs and Indexes: 12K Current Retention: 10 days
filters: Logs and Indexes: 4.0K Current Retention: 0 days
dlp: Logs and Indexes: 4.0K Current Retention: 0 days
hip_report_base: Logs and Indexes: 1.1M Current Retention: N/A
wildfire: Logs and Indexes: 40K Current Retention: N/A

Space reserved for cores:       0MB
```

### View Log Data Ingestion Rate
```bash
debug log-receiver statistics
```
#### Sample Output
```bash
Logging statistics
------------------------------ -----------
Log incoming rate:             88/sec
Log written rate:              82/sec
Corrupted packets:             0
Corrupted URL packets:         0
Corrupted HTTP HDR packets:    0
Corrupted HTTP HDR Insert packets: 0
Corrupted EMAIL HDR packets:   0
Logs discarded (queue full):   0
Traffic logs written:          362572715
GTP logs written:              0
Tunnel logs written:           0
Auth logs written:             0
Config logs written:           814
System logs written:           3481650
Alarm logs written:            0
Userid logs written:           7429392
SCTP logs written:             0
GlobalProtect logs written:    1087897
DECRYPTION logs written:       12323
URL logs written:              38111341
Wildfire logs written:         71803
Anti-virus logs written:       0
Maching Learning-virus logs written: 0
Wildfire Anti-virus logs written: 0
Spyware logs written:          2095
Spyware-DNS logs written:      1176
Attack logs written:           0
Vulnerability logs written:    1601153
Data logs written:             0
DLP logs written:              0
Non File DLP logs written:     0
URL Cloud logs written:        0
Fileext logs written:          1171503
Fileext logs URL not written:  946479
Fileext logs URL not written (timedout): 9
URL cache age out count:       0
URL cache full count:          0
URL cache key exist count:     2153047
URL cache wrt incomplete http hdrs count: 0
URL cache rcv http hdr before url count: 0
URL cache full drop count(url log not received): 0
URL cache age out drop count(url log not received): 0
Email hdr cache count:         0
Email hdr cache hit count:     0
HTTP hdr insertion received:   0
HTTP hdr insertion processed:  0
HTTP hdr insert no URL drop count: 0
HTTP hdr insert with invalid URL log: 0
HTTP hdr insert with values exceeded max allowed length: 0
Traffic alarms dropped due to sysd write failures: 0
Traffic alarms dropped due to global rate limiting: 0
Traffic alarms dropped due to each source rate limiting: 0
Traffic alarms generated count:  0
Netflow incoming count:        0
Log Forward count:             0
Log Forward discarded (queue full) count: 0
Log Forward discarded (send error) count: 0
Total logs not written due to disk unavailability: 0
Logs not written since disk became unavailable: 0
HIP Report logs received:      0
DPI Traffic logs written:      0
DPI Threat logs written:       0

Summary Statistics:
Num current entries in trsum:36097
Num cumulative entries in trsum:285888975
Num current entries in thsum:12089
Num cumulative entries in thsum:39722003
Num current entries in urlsum:0
Num cumulative entries in urlsum:0
Num current entries in gtpsum:0
Num cumulative entries in gtpsum:0
Num current entries in sctpsum:0
Num cumulative entries in sctpsum:0
Num current drop entries in trsum:0
Num cumulative drop entries in trsum:0
Num current drop entries in thsum:0
Num cumulative drop entries in thsum:0
Num current drop entries in urlsum:0
Num cumulative drop entries in urlsum:0
Num current drop entries in gtpsum:0
Num cumulative drop entries in gtpsum:0
Num current drop entries in sctpsum:0
Num cumulative drop entries in sctpsum:0
Num current drop entries in desum:0
Num cumulative drop entries in desum:0

External Forwarding stats:
      Type  Enqueue Count     Send Count     Drop Count    Queue Depth     Send Rate(last 1min)
    syslog              0              0              0              0                        0
      snmp              0              0              0              0                        0
     email              0              0              0              0                        0
       raw              0              0              0              0                        0
      http              0              0              0              0                        0
   autotag              0              0              0              0                        0
quarantine              0        
```
