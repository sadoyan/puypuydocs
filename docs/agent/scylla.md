![Scylla](../images/scylla.png)

Scylla is a massively scalable Open-Source NoSQL database. It's an implementation of Apache Cassandra using C++ instead of Java. 
Developers of Scylla describe it as : Scylla is a drop-in replacement for Apache Cassandra and DynamoDB.
Yet it also provides significant advantages over these other databases. Flip the cards to see similarities and differences. 
Scylla ships with native metrics exported, which runs by default 9180 port. So there is no need for special configuration of Scylla. 

Scylla config of PuyPuy agent is in `conf/bigdata.ini` file. If you run recommended Scylla installation, no need for making spacial configs for PuyPuy agent as well. 

#### **Install**


```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_scylla.py ./
```

#### **Configure**

Default config should be suitable for the most of the installations. 
If you are running Scylla's node_exporter other than default way, change the `stats` parameter to match your setup. 
```ini
[Scilla]
stats : http://127.0.0.1:9180/metrics
```

#### **Restart**

```commandline
${PUYPUY_HOME}/puypuy.sh restart
```

#### **Provides**

| Name | Description| Type | Unit |
| ------------- |----------------------------------------------------------------|-----------|-------|
|scylla_node_operation_mode| The operation mode of the current node.| gauge | None |
|scylla_reactor_utilization| CPU utilization| gauge | None |
|scylla_transport_requests_served| Counts a number of served requests | counter | None |
|scylla_storage_proxy_coordinator_write_latency_count| The general write latency histogram| histogram | None |
|scylla_storage_proxy_coordinator_write_latency_sum| The general write latency histogram| histogram | None |
|scylla_storage_proxy_coordinator_write_timeouts| Number of write request failed due to a timeout| counter | None |
|scylla_storage_proxy_coordinator_read_latency_count| The general read latency histogram | histogram | None |
|scylla_storage_proxy_coordinator_read_latency_sum| The general read latency histogram | histogram | None |
|scylla_storage_proxy_coordinator_read_timeouts| Number of read request failed due to a timeout | counter | None |
|scylla_cache_row_hits| Total number of rows needed by reads and found in cache| counter | None |
|scylla_cache_row_misses| Total number of rows needed by reads and missing in cache | counter | None |
|scylla_cache_bytes_total| Total size of memory for the cache | gauge | None |
|scylla_cache_bytes_used| urrent bytes used by the cache out of the total size of memory | gauge | Bytes |
|scylla_cache_concurrent_misses_same_key| Total number of operation with misses same key | counter | None |
|scylla_sstables_row_reads| Number of rows read| counter | None |
|scylla_sstables_range_partition_reads| Number of range tombstones written | counter | None |
|scylla_sstables_range_tombstone_writes| Cassandra Peak threads count | gauge | None |
|scylla_sstables_row_writes| Number of clustering rows written | counter | None |
|scylla_sstables_single_partition_reads| Number of single partition flat mutation reads | counter | None |
|scylla_sstables_sstable_partition_reads| Number of whole sstable flat mutation reads| counter | None |
|scylla_sstables_static_row_writes| Number of static rows written | counter | None |
|scylla_sstables_tombstone_writes| Number of tombstones written | counter | None |
|scylla_cql_inserts| The Number of CQL INSERT requests| gauge | None |
|scylla_cql_reads| The Number of CQL SELECT requests. | gauge | None |
|scylla_cql_deletes| The Number of CQL DELETE requests. | gauge | None |
|scylla_cql_updates| The Number of CQL UPDATE requests| gauge | None |
|scylla_cql_batches| The Number of CQL BATCH requests | gauge | None |
|scylla_cache_row_hits| Total number of rows needed by reads and found in cache| gauge | None |
|scylla_cache_row_misses| Total number of rows needed by reads and missing in cache | gauge | None |
|scylla_transport_requests_served|Counts a number of served requests.| gauge | None |
