# FortiSIEM Troubleshooting
## 1. FortiSIEM DR Replication - "The 'DBSvr' service is not starting due to 100% disk utilization in '/cmdb'"
### 1.1. Find out the current WAL file:
```bash
pg_controldata /cmdb/data | grep "REDO WAL"
```
This command will print out the current `WAL` file that is being processed. Delete all `WAL` files that are _older_ than the current one.

### 1.2. Use `find` command to search and delete already processed `WAL` files (!!!! REPLACE `<CURRENT_WAL_FILE>` with the **current** WAL FILE NAME FOUND IN STEP 1.1. !!!!)
```bash
find -L /cmdb/data/pg_wal \
-not -path /cmdb/data/pg_wal/archive_status/* \
-not -samefile /cmdb/data/pg_wal/archive_status \
-not -newer /cmdb/data/pg_wal/<CURRENT_WAL_FILE> \
-not -samefile /cmdb/data/pg_wal/<CURRENT_WAL_FILE> \
-exec rm {} \;
```

