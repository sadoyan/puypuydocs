![etcd](../images/etcd.png)

Etcd is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. 
Configuration of etcd check is stored `k8s.ini` file in `conf` directory. 

##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_etcd.py ./
```

##### **Configure**

If you are using default installation of etcd service, no additional configuration is needed.
If you need to monitor etc in non default location, edit `conf/k8s.ini` section `etcd` and set `metrics` parameter with value matching your needs. 

```ini
[etcd]
metrics : http://127.0.0.1:2379/metrics
```

##### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|etcd_debugging_mvcc_db_compaction_pause_duration_milliseconds_count|DB compaction pause duration.|count|Milliseconds|
|etcd_debugging_mvcc_db_compaction_pause_duration_milliseconds_sum|DB compaction pause duration.|counter|Milliseconds|
|etcd_debugging_mvcc_db_compaction_total_duration_milliseconds_count|DB compaction total duration.|count|Milliseconds|
|etcd_debugging_mvcc_db_compaction_total_duration_milliseconds_sum|DB compaction total duration.|counter|Milliseconds|
|etcd_debugging_mvcc_index_compaction_pause_duration_milliseconds_count|Index compaction pause duration.|count|Milliseconds|
|etcd_debugging_mvcc_index_compaction_pause_duration_milliseconds_sum|Index compaction pause duration.|counter|Milliseconds|
|etcd_debugging_snap_save_marshalling_duration_seconds_count|The marshalling cost distributions of save called by snapshot.|count|Seconds|
|etcd_debugging_snap_save_marshalling_duration_seconds_sum|The marshalling cost distributions of save called by snapshot.|counter|Seconds|
|etcd_debugging_snap_save_total_duration_seconds_count|The total latency distributions of save called by snapshot.|count|Seconds|
|etcd_debugging_snap_save_total_duration_seconds_sum|The total latency distributions of save called by snapshot.|counter|Seconds|
|etcd_disk_backend_commit_duration_seconds_count|The latency distributions of commit called by backend.|count|Seconds|
|etcd_disk_backend_commit_duration_seconds_sum|The latency distributions of commit called by backend.|counter|Seconds|
|etcd_disk_backend_defrag_duration_seconds_count|The latency distribution of backend defragmentation.|count|Seconds|
|etcd_disk_backend_defrag_duration_seconds_sum|The latency distribution of backend defragmentation.|counter|Seconds|
|etcd_disk_backend_snapshot_duration_seconds_count|The latency distribution of backend snapshots.|count|Seconds|
|etcd_disk_backend_snapshot_duration_seconds_sum|The latency distribution of backend snapshots.|counter|Seconds|
|etcd_disk_wal_fsync_duration_seconds_count|The latency distributions of fsync called by wal.|count|Seconds|
|etcd_disk_wal_fsync_duration_seconds_sum|The latency distributions of fsync called by wal.|counter|Seconds|
|etcd_go_gc_duration_seconds_count|A summary of the GC invocation durations|count|Seconds|
|etcd_go_gc_duration_seconds_sum|A summary of the GC invocation durations|counter|Seconds|
|etcd_mvcc_hash_duration_seconds_count|The latency distribution of storage hash operation.|count|Seconds|
|etcd_mvcc_hash_duration_seconds_sum|The latency distribution of storage hash operation.|counter|Seconds|
|etcd_mvcc_hash_rev_duration_seconds_count|The latency distribution of storage hash by revision operation.|count|Seconds|
|etcd_mvcc_hash_rev_duration_seconds_sum|The latency distribution of storage hash by revision operation.|counter|Seconds|
|etcd_process_cpu_seconds_total|Total user and system CPU time spent in seconds|counter|Seconds|
|etcd_process_max_fds|Maximum number of open file descriptors|gauge|None|
|etcd_process_open_fds|Number of open file descriptors|gauge|None|
|etcd_process_resident_memory_bytes|Resident memory size in bytes.|gauge|Bytes|
|etcd_process_start_time_seconds|Start time of the process since unix epoch in seconds.|gauge|Seconds|
|etcd_process_virtual_memory_bytes|Virtual memory size in bytes.|gauge|Bytes|
|etcd_snap_db_fsync_duration_seconds_count|The latency distributions of fsyncing .snap.db file|count|Seconds|
|etcd_snap_db_fsync_duration_seconds_sum|The latency distributions of fsyncing .snap.db file|counter|Seconds|
|etcd_snap_db_save_total_duration_seconds_count|The total latency distributions of v3 snapshot save|count|Seconds|
|etcd_snap_db_save_total_duration_seconds_sum|The total latency distributions of v3 snapshot save|count|Seconds|
