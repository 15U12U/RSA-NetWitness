#

## Data ingestion duplication avoidance [^1]

Use an ingest time transformation to filter out CEF messages from the Syslog stream to avoid duplication, as shown in the following query example.

```kql
source |
where ProcessName !contains "CEF"
```





[^1]: [Data ingestion duplication avoidance](https://learn.microsoft.com/en-us/azure/sentinel/cef-syslog-ama-overview?tabs=single#data-ingestion-duplication-avoidance)
