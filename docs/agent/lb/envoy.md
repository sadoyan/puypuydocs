![Envoy](../../images/envoy.png)

##### **Install**

```commandline
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_envoy.py ./
```

##### **Configure**

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

Restart PuyPuy agent and Envoy. 

##### **Provides**

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

