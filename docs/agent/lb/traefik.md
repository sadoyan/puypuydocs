![Traefik](../../images/traefik.png)

##### **Install** 

```commandline
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_traefik.py ./
```

##### **Configure**

Start Traefik with following parameters 

```
[entryPoints.metrics]
  address = ":8081"
[metrics]
  [metrics.prometheus]
    entryPoint = "metrics"
```

Edit ```conf/loadbalancer.ini``` and set Envoy URL to prometheus stats. 

```ini
[traefik]
metrics : http://127.0.0.1:8081/metrics
```  

Restart PuyPuy agent and Envoy. 

##### **Provides**

| Name  | Type | Description |
| ------------- | ------------- |------------- |
|traefik_entrypoint_http_duration_seconds_count|counter|How long it took to process the request on an entrypoint, partitioned by status code, protocol, and method.|
|traefik_entrypoint_http_duration_seconds_sum|gauge|How long it took to process the request on an entrypoint, partitioned by status code, protocol, and method.|
|traefik_go_gc_duration_seconds_count|counter|A summary of the GC invocation durations.|
|traefik_go_gc_duration_seconds_sum|gauge|A summary of the GC invocation durations.|
|traefik_process_cpu_seconds_total|counter|Total user and system CPU time spent in seconds.|
|traefik_process_max_fds|gauge|Maximum number of open file descriptors.|
|traefik_process_open_fds|gauge|Number of open file descriptors.|
|traefik_process_resident_memory_bytes|gauge|Resident memory size in bytes.|
|traefik_process_start_time_seconds|gauge|Start time of the process since unix epoch in seconds.|
|traefik_process_virtual_memory_bytes|gauge|Virtual memory size in bytes.|
|traefik_process_virtual_memory_max_bytes|gauge|Maximum amount of virtual memory available in bytes.|
