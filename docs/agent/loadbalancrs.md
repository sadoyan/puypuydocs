
### **HAProxy** 

![HAProxy](../images/haproxy.png)

**Install** 

```commandline
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_haproxy.py ./
```

**Configure**

Statistics for HAProxy can be enabled by adding following to haproxy.conf : 

```ini
listen stats
  bind :8888
  mode http
  stats enable
  stats hide-version
  stats realm Haproxy\ Statistics
  stats uri /haproxy?stats
  stats auth User:Pass
```
Then you need to configure Agent to start gathering statistics from Haproxy server. 

```ini
[HAProxy]
url: http://127.0.0.1/haproxy?stats;csv
user: User
pass: Pass
auth: True
upstream: MyApp1
```
Config parameter `upstream` is name of upstream server configured in Haproxy.

```ini
listen  MyApp1 192.168.0.1:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.10:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.20:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       
```
From above mentioned example it is obvious, that upstream name is MyApp1, so this name should be placed as upstream parameter in `HAProxy` section of `webservers.ini`
If you have more sophisticated HAProxy configuration with multiple upstreams and need to monitor several upstreams, you should write comma and separate names in upstream configuration of Agent: 
So if your HAProxy configuration looks like this : 

```yaml
listen  MyApp1 192.168.0.1:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.10:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.20:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       

listen  MyApp2 192.168.0.2:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.11:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.22:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       
```
Agent should be configured as follows: 

```ini
[HAProxy]
url: http://127.0.0.1/haproxy?stats;csv
user: User
pass: Pass
auth: True
```

**Restart**

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```

**Provides**

| Name  | Description | Type |
| ------------- | ------------- |------------- |
|haproxy_connrate|Connections per second|rate|
|nhaproxy_sessions|Current active sessions |gauge|


### **Envoy**

![Envoy](../images/envoy.png)

**Install** 

```commandline
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_envoy.py ./
```

**Configure**

Start Envoy with following parameters 

```yaml
admin:
  access_log_path: "/dev/null"
  address:
    socket_address:
      address: 127.0.0.1
      port_value: 8001
```

Edit ```conf/loadbalancer.ini``` and set Envoy URL to prometheus stats. 

```ini
[Envoy]
metrics : http://127.0.0.1:8001/stats/prometheus
```  

Restart OddEye agent and Envoy. 

**Provides**

| Name  | Type | Description |
| ------------- | ------------- |------------- |
|downstream_cx_total|counter|Total connections|
|downstream_cx_ssl_total|counter|Total TLS connections|
|downstream_cx_http1_total|counter|Total HTTP/1.1 connections|
|downstream_cx_websocket_total|counter|Total WebSocket connections|
|downstream_cx_http2_total|counter|Total HTTP/2 connections|
|downstream_cx_destroy|counter|Total connections destroyed|
|downstream_cx_destroy_remote|counter|Total connections destroyed due to remote close|
|downstream_cx_destroy_local|counter|Total connections destroyed due to local close|
|downstream_cx_destroy_active_rq|counter|Total connections destroyed with 1+ active request|
|downstream_cx_destroy_local_active_rq|counter|Total connections destroyed locally with 1+ active request|
|downstream_cx_destroy_remote_active_rq|counter|Total connections destroyed remotely with 1+ active request|
|downstream_cx_active|gauge|Total active connections|
|downstream_cx_ssl_active|gauge|Total active TLS connections|
|downstream_cx_http1_active|gauge|Total active HTTP/1.1 connections|
|downstream_cx_websocket_active|gauge|Total active WebSocket connections|
|downstream_cx_http2_active|gauge|Total active HTTP/2 connections|
|downstream_cx_protocol_error|counter|Total protocol errors|
|downstream_cx_length_ms|histogram|Connection length milliseconds|
|downstream_cx_rx_bytes_total|counter|Total bytes received|
|downstream_cx_rx_bytes_buffered|gauge|Total received bytes currently buffered|
|downstream_cx_tx_bytes_total|counter|Total bytes sent|
|downstream_cx_tx_bytes_buffered|gauge|Total sent bytes currently buffered|
|downstream_cx_drain_close|counter|Total connections closed due to draining|
|downstream_cx_idle_timeout|counter|Total connections closed due to idle timeout|
|downstream_flow_control_paused_reading_total|counter|Total number of times reads were disabled due to flow control|
|downstream_flow_control_resumed_reading_total|counter|Total number of times reads were enabled on the connection due to flow control|
|downstream_rq_total|counter|Total requests|
|downstream_rq_http1_total|counter|Total HTTP/1.1 requests|
|downstream_rq_http2_total|counter|Total HTTP/2 requests|
|downstream_rq_active|gauge|Total active requests|
|downstream_rq_response_before_rq_complete|counter|Total responses sent before the request was complete|
|downstream_rq_rx_reset|counter|Total request resets received|
|downstream_rq_tx_reset|counter|Total request resets sent|
|downstream_rq_non_relative_path|counter|Total requests with a non-relative HTTP path|
|downstream_rq_too_large|counter|Total requests resulting in a 413 due to buffering an overly large body|
|downstream_rq_1xx|counter|Total 1xx responses|
|downstream_rq_2xx|counter|Total 2xx responses|
|downstream_rq_3xx|counter|Total 3xx responses|
|downstream_rq_4xx|counter|Total 4xx responses|
|downstream_rq_5xx|counter|Total 5xx responses|
|downstream_rq_ws_on_non_ws_route|counter|Total WebSocket upgrade requests rejected by non WebSocket routes|
|downstream_rq_time|histogram|Request time milliseconds|
|rs_too_large|counter|Total response errors due to buffering an overly large body|

### **Traefik**

![Traefik](../images/traefik.png)

**Install** 

```commandline
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_traefik.py ./
```

**Configure**

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

Restart OddEye agent and Envoy. 

**Provides**

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
