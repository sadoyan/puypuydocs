##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_foundationdb.py ./
```

##### **Configure**

Configuration of **FoundationDB** check is located at `bigdata.ini`. 
You only need to set right path for ```fdbcli``` binary, by default for packaged installation it is ```/usr/bin/fdbcli``` .
If you have compilled **FoundationDB** from source or used oher mechanism to install it, just locate ```fdbcli``` change default path in config file. 

Restart Agent. `./puypuy.sh restart`
`check_foundationdb` module should run without making changes in configuration, but according to your specific needs, you can edit `conf/bindata.ini` and make changes in section **FoundationDB**

```ini
[FoundationDB]
fdbclipath: /usr/bin/fdbcli
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|fdb_bytes_read|Bytes read from FoundationDB |rate|Bytes|
|fdb_bytes_written|Bytes written to FoundationDB |rate|Bytes|
|fdb_connected_clients | Connected clients |gauge|None|
|fdb_keys_read | FoundationDB read key per second | rate| OPS|
|fdb_latency_commit_seconds | Latency of Commits |gauge| Millisecond | 
|fdb_latency_read_seconds | Latency of Reads |gauge| Millisecond |
|fdb_least_operating_space_bytes_log_server| Least operating space bytes log server |gauge| Bytes| 
|fdb_least_operating_space_bytes_storage_server | Least operating space bytes Storage server |gauge| Bytes|
|fdb_memory_committed_bytes| FoundationDB committed memory |gauge| Bytes|
|fdb_memory_free_bytes| FoundationDB free memory |gauge| Bytes|
|fdb_memory_total_bytes| FoundationDB total memory |gauge| Bytes|
|fdb_moving_data | Data which is being moved | rate | Bytes| 
|fdb_operations_read_requests | Read requests per second | rate| OPS|
|fdb_operations_reads| Reads per second | rate| OPS|
|fdb_partitions_count |Amount of partitions |gauge|None|
|fdb_partitions_size_avg |Average size of partition |gauge| Bytes |
|fdb_total_disk_used_bytes | Used disk bytes |gauge| Bytes |
|fdb_total_kv_size_bytes| Size of key/values |gauge| Bytes |
|fdb_transactions_conflicted | Amount of conflicted transactions |gauge|None|
|fdb_transactions_started | Started transactions |gauge|None|
|fdb_worst_queue_bytes_log_server| Size of worst queue log server |gauge| Bytes|
|fdb_worst_queue_bytes_storage_server| Size of worst queue storage server |gauge| Bytes|
