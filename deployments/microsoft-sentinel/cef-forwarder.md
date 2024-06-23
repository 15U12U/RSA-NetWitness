#

## Data ingestion duplication avoidance [^1]

```kql
source |
where ProcessName !contains "CEF"
```





[^1]: [Data ingestion duplication avoidance](https://learn.microsoft.com/en-us/azure/sentinel/cef-syslog-ama-overview?tabs=single#data-ingestion-duplication-avoidance)
