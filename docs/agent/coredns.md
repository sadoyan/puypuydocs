![coredns](../images/coredns.png)

Coredns is a fast and flexible, cloud native DNS server, written in Go programing language  
Configuration of coredns check is stored `k8s.ini` file in `conf` directory. 

##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_coredns.py ./
```

##### **Configure**

If you are using default installation of ethd service, no additional configuration is needed.
If you need to monitor etc in non default location, edit `conf/k8s.ini` section `coredns` and set `metrics` parameter with value matching your needs. 

Before enabling `check_coredns` module you should enable metrics plugin at Coredns. 
Snippet above is an example of how metrics interface can be enabled in coredns 

```json
{
    prometheus localhost:9253
}
```

When config file is changes , you should restart coredns daemon to apply new settings. 
The metrics path at coredns is fixed to `/metrics` so make sure  full URL of coredns metrics interface is written in `conf/k8s.ini` 
 

```ini
[coredns]
metrics : http://127.0.0.1:9253/metrics
```

##### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|coredns_go_gc_duration_seconds_count|A summary of the GC invocation durations.|counter|Seconds|
|coredns_go_gc_duration_seconds_sum|A summary of the GC invocation durations.|sum|Seconds|
|coredns_go_goroutines|Number of goroutines that currently exist.|gauge|None|
|coredns_go_memstats_alloc_bytes|Number of bytes allocated and still in use.|gauge|Bytes|
|coredns_go_memstats_alloc_bytes_total|Number of bytes allocated and still in use.|total|Bytes|
|coredns_go_memstats_buck_hash_sys_bytes|Number of bytes used by the profiling bucket hash table.|gauge|Bytes|
|coredns_go_memstats_frees_total|Total number of frees.|counter|None|
|coredns_go_memstats_gc_cpu_fraction|The fraction of this program's available CPU time used by the GC since the program started.|gauge|None|
|coredns_go_memstats_gc_sys_bytes|Number of bytes used for garbage collection system metadata.|gauge|Bytes|
|coredns_go_memstats_heap_alloc_bytes| Number of heap bytes allocated and still in use.|gauge|Bytes|
|coredns_go_memstats_heap_idle_bytes|Number of heap bytes waiting to be used.|gauge|Bytes|
|coredns_go_memstats_heap_inuse_bytes|Number of heap bytes that are in use.|gauge|Bytes|
|coredns_go_memstats_heap_objectsNumber of allocated objects.|gauge|integer|
|coredns_go_memstats_heap_released_bytes|Number of heap bytes released to OS.|gauge|Bytes|
|coredns_go_memstats_heap_sys_bytes|Number of heap bytes obtained from system.|gauge|Bytes|
|coredns_go_memstats_last_gc_time_seconds|Number of seconds since 1970 of last garbage collection.|gauge|Seconds|
|coredns_go_memstats_lookups_total|Total number of pointer lookups.|counter|None|
|coredns_go_memstats_mallocs_total|Total number of mallocs.|counter|None|
|coredns_go_memstats_mcache_inuse_bytes|Number of bytes in use by mcache structures.|gauge|Bytes|
|coredns_go_memstats_mcache_sys_bytes|Number of bytes used for mcache structures obtained from system.|gauge|Bytes|
|coredns_go_memstats_mspan_inuse_bytes|Number of bytes in use by mspan structures.|gauge|Bytes|
|coredns_go_memstats_mspan_sys_bytes|Number of bytes used for mspan structures obtained from system.|gauge|Bytes|
|coredns_go_memstats_next_gc_bytes|Number of heap bytes when next garbage collection will take place.|gauge|Bytes|
|coredns_go_memstats_other_sys_bytes|Number of bytes used for other system allocations.|gauge|Bytes|
|coredns_go_memstats_stack_inuse_bytes|Number of bytes in use by the stack allocator.|gauge|Bytes|
|coredns_go_memstats_stack_sys_bytes|Number of bytes obtained from system for stack allocator.|gauge|Bytes|
|coredns_go_memstats_sys_bytes|Number of bytes obtained from system.|gauge|Bytes|
|coredns_go_threads|Number of OS threads created.|gauge|None|
|coredns_health_request_duration_seconds_count|Time each request took.|counter|Seconds|
|coredns_health_request_duration_seconds_sum|Time each request took.|sum|Seconds|
|coredns_panic_count_total|A metrics that counts the number of panics.|counter|None|
|coredns_process_cpu_seconds_total|Total user and system CPU time spent in seconds.|counter|Seconds|
|coredns_process_max_fds|Maximum number of open file descriptors.|gauge|None|
|coredns_process_open_fds|Number of open file descriptors.|gauge|None|
|coredns_process_resident_memory_bytes|Resident memory size in bytes.|gauge|Bytes|
|coredns_process_start_time_seconds|Start time of the process since unix epoch in seconds.|gauge|Seconds|
|coredns_process_virtual_memory_bytes|Virtual memory size in bytes.|gauge|Bytes|
|coredns_process_virtual_memory_max_bytes|Maximum amount of virtual memory available in bytes.|gauge|Bytes|
