### **Nomad**

![Nomad](../images/nomad.png)

Nomad exposes metrics via HTTP/Json out of the box, so all you need is just to symlink `checks_available/check_hashicorp_nomad_py` to `checks_enabled` and restart Agent.

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_hashicorp_nomad_py ./
```

##### **Configure**

At most of cases there is no need to configure **Agent**, but if you have non default installation of Nomad, 
or if you need to monitor remote Nomad, edit `conf/hashicorp.ini` and make your changes at Hashicorp-Nomad section. 
Nomad also exposes metrics per running job, which fo some installations can create tens even hundreds os metrics. 
If you do not want to monitor these metrics set ```jobstats: False``` st config file.   

```ini
[Hashicorp-Nomad]
telemetery: http://127.0.0.1:4646/v1/metrics
jobstats: True
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|nomad_client_allocated_cpu|Total amount of CPU shares the scheduler has allocated to task|gauge|MHz|
|nomad_client_unallocated_cpu|Total amount of CPU shares free for the scheduler to allocate to tasks|gauge|MHz|
|nomad_client_allocated_memory|Total amount of memory the scheduler has allocated to tasks|gauge|Megabytes|
|nomad_client_unallocated_memory|Total amount of memory free for the scheduler to allocate to tasks|gauge|Megabytes|
|nomad_client_allocated_disk|Total amount of disk space the scheduler has allocated to tasks|gauge|Megabytes|
|nomad_client_unallocated_disk|Total amount of disk space free for the scheduler to allocate to tasks|gauge|Megabytes|
|nomad_client_allocated_network|Total amount of bandwidth the scheduler has allocated to tasks on the given device|gauge|Megabits|
|nomad_client_unallocated_network|Total amount of bandwidth free for the scheduler to allocate to tasks on the given device|gauge|Megabits|
|nomad_job_summary_queued|Number of queued allocations for a job|gauge|None|
|nomad_job_summary_complete|Number of complete allocations for a job|gauge|None|
|nomad_job_summary_failed|Number of failed allocations for a job|gauge|None|
|nomad_job_summary_running|Number of running allocations for a job|gauge|None|
|nomad_job_summary_starting|Number of starting allocations for a job|gauge|None|
|nomad_job_summary_lost|Number of lost allocations for a job|gauge|None|
|nomad_runtime_num_goroutines|Number of goroutines and general load pressure indicator|gauge|None|
|nomad_runtime_alloc_bytes|Memory utilization|gauge|Bytes|
|nomad_runtime_heap_objects|Number of objects on the heap. General memory pressure indicator|gauge|None|
|nomad_heartbeat_active|Number of active heartbeat timers. Each timer represents a Nomad Client connection|gauge|None|



### **Consul**

![Consul](../images/consul.png)

Consul exposes metrics via HTTP/Json out of the box, so all you need is just to symlink `checks_available/check_hashicorp_consul_py` to `checks_enabled` and restart Agent.

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_hashicorp_consul_py ./
```

##### **Configure**

At most of cases there is no need to configure **Agent**, but if you have non default installation of Consul, 
or if you need to monitor remote Consul, edit `conf/hashicorp.ini` and make your changes at Hashicorp-Consul section. 
Consul also exposes detailed metrics, with rates and counters, so if youwant to see these metrics set ```detailed: True``` for getting rated stats set ```getrates: True```

```ini
[Hashicorp-Consul]
telemetery: http://127.0.0.1:8500/v1/agent/metrics
detailed: True
getrates: True
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|consul_memberlist_tcp_connect|This metric counts the number of times an agent has initiated a push/pull sync with an other agent.|counter|integer|
|consul_memberlist_tcp_sent|This metric measures the total number of bytes sent by an agent through the TCP protocol.|counter|bytes|
|consul_memberlist_udp_received|This metric measures the total number of bytes received by an agent through the UDP protocol.|counter|bytes|
|consul_memberlist_udp_sent|This metric measures the total number of bytes sent by an agent through the UDP protocol.|counter|bytes|
|consul_rpc_accept_conn|This metric counts the number of RPC prorocol accepted connections|counter|integer|
|consul_rpc_request|This metric counts the number of RPC prorocol requests|counter|integer|
|consul_runtime_alloc_bytes|This metric measures runtime bytes allocated by Agent|counter|bytes|
|consul_runtime_free_count|This metric measures runtime bytes freed by Agent|counter|bytes|
|consul_runtime_heap_objects|This metric measures amount of objects in Agent's heap|counter|bytes|
|consul_runtime_sys_bytes|This metric measures runtime system bytes freed by Agent|counter|bytes|
|consul_runtime_total_gc_pause_ns|This metric measures Agent's GC pauses.|counter|nanosecond|
|consul_runtime_total_gc_runs|This metric measures Agent's GC runs.|counter|integer|
|consul_session_ttl_active|This metric measures active TTL sessions.|counter|integer|

**Detailed**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|consul_fsm.coordinate.batch.update|This measures the time it takes to apply the given batch coordinate update to the FSM.|gauge|millisecond|
|consul_http_get_v1_agent_checks|This metric gives the number Agen'ts check HTTP GET requests|rate\counter|integer|
|consul_http_get_v1_agent_metrics|This metric gives the number Agen'ts metric HTTP GET requests|rate\counter|integer|
|consul_http_get_v1_agent_self|This metric gives the number Agen'ts self HTTP GET requests|rate\counter|integer|
|consul_http_get_v1_agent_services|This metric gives the number Agen'ts service HTTP GET requests|rate\counter|integer|
|consul_memberlist_gossip|This metric gives the number of gossips (messages) broadcasted to a set of randomly selected nodes.|rate\counter|integer|
|consul_memberlist_probenode|This metric measures the time taken to perform a single round of failure detection on a select agent.|rate\counter|integer|
|consul_raft_fsm_apply|This metric gives the number of logs committed since the last interval.|rate\counter|integer|
|consul_raft_rpc_appendentries|This metric measures the time taken to process an append entries RPC call from an agent.|rate\counter|millisecond|
|consul_raft_rpc_appendentries_processlogs|This metric measures the time taken to add any outstanding logs for an agent, since the last appendEntries was invoked|rate\counter|millisecond|
|consul_raft_rpc_appendentries_storelogs|This metric measures the time taken to process the outstanding log entries of an agent.|rate\counter|millisecond|
|consul_raft_rpc_processheartbeat|This metric measures the time taken to process a heartbeat request.|rate\counter|millisecond|
|consul_runtime_gc_pause_ns|This metric measures the Golang GC runtime pauses.|rate\counter|millisecond|
|consul_serf_coordinate_adjustment_ms|This metric measures the coordinate adjustments in milliseconds.|rate\counter|millisecond|
|consul_serf_queue_event|This metric measures events in sef queue.|rate\counter|integer|
|consul_serf_queue_intent|This metric measures intents in sef queue.|rate\counter|integer|
|consul_serf_queue_query|This metric measures queries in sef queue.|rate\counter|integer|

### **Vault**

![Vault](../images/vault.png)

Vault exposes metrics via HTTP/Json out of the box, so all you need is just to symlink `checks_available/check_hashicorp_consul_py` to `checks_enabled` and restart Agent.

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_hashicorp_vault_py ./
```

##### **Configure**

At most of cases there is no need to change telemetry link, but if you have non default installation of Vault, 
or if you need to monitor remote Vault, edit `conf/hashicorp.ini` and make your changes at Hashicorp-Vault section. 
Also it is very important to set correct token in config file, or Vault will deny requests from PuyPuy Agent 
Vault also exposes detailed metrics, with rates and counters, so if youwant to see these metrics set ```detailed: True``` for getting rated stats set ```getrates: True```

```ini
[Hashicorp-Vault]
telemetery: http://127.0.0.1:8200/v1/sys/metrics
token: s.HqQb7CFNBT1wHWLHD0DrOx6P
detailed: True
getrates: False
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|vault_expire_num_leases|This metric gives the number of expired leases.|gauge|integer|
|vault_runtime_alloc_bytes|This metric shows amount of memory allocated by Vault process.|gauge|Bytes|
|vault_runtime_free_count|This metric shows Number of freed objects|gauge|integer|
|vault_runtime_heap_objects|This metric gives the number of objects in Vaulr process's heap.|gauge|integer|
|vault_runtime_malloc_count|This metric shows cumulative count of allocated heap objects|gauge|integer|
|vault_runtime_num_goroutines|Number of goroutines and general load pressure indicator|gauge|None|
|vault_runtime_sys_bytes|This includes what is being used by Vault's heap and what has been reclaimed but not given back to the operating system.|gauge|Bytes|
|vault_runtime_total_gc_pause_ns|This metric measures the Golang GC runtime pauses.|counter|millisecond|
|vault_runtime_total_gc_runs|This metric measures Agent's GC runs.|counter|integer|

**Detailed**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|vault_audit_log_request|Duration of time taken by all audit log requests across all audit log devices|counter\rate|milliseconds|
|vault_audit_log_response|Duration of time taken by audit log responses across all audit log devices|counter\rate|milliseconds|
|vault_barrier_get|Duration of time taken by GET operations at the barrier|counter\rate|milliseconds|
|vault_barrier_put|Duration of time taken by PUT operations at the barrier|counter\rate|milliseconds|
|vault_barrier_delete|Duration of time taken by DELETE operations at the barrier|counter\rate|milliseconds|
|vault_barrier_list|Duration of time taken by LIST operations at the barrier|counter\rate|milliseconds|
|vault_core_check_token|Duration of time taken by token checks handled by Vault core|counter\rate|milliseconds|
|vault_core_fetch_acl_and_token|Duration of time taken by ACL and corresponding token entry fetches handled by Vault core|counter\rate|milliseconds|
|vault_core_handle_request|Duration of time taken by requests handled by Vault core|counter\rate|milliseconds|
|vault_policy_get_policy|Time taken to GET a policy|counter\rate|milliseconds|
|vault_policy_list_policy|Time taken to LIST a policy|counter\rate|milliseconds|
|vault_policy_set_policy|Time taken to SET a policy|counter\rate|milliseconds|
|vault_policy_delete_policy|Time taken to DELETE a policy|counter\rate|milliseconds|
|vault_token_lookup|The time taken to look up a token|counter\rate|milliseconds|
|vault_rollback_attempt_auth_token_|Time taken to perform a rollback operation for the token auth method|counter\rate|milliseconds|
|vault_rollback_attempt_auth_ldap_|Time taken to perform a rollback operation for the LDAP auth method|counter\rate|milliseconds|
|vault_rollback_attempt_cubbyhole_|Time taken to perform a rollback operation for the Cubbyhole secret backend|counter\rate|milliseconds|
|vault_rollback_attempt_identity_|Time taken to perform a rollback operation for the Identity backend|counter\rate|milliseconds|
|vault_rollback_attempt_secret_|Time taken to perform a rollback operation for the K/V secret backend|counter\rate|milliseconds|
|vault_rollback_attempt_sys_|Time taken to perform a rollback operation for the system backend|counter\rate|milliseconds|
|vault_route_read_sys_|Time taken to perform a route rollback operation for the system backend|counter\rate|milliseconds|
|vault_route_rollback_auth_token_|Time taken to perform a route rollback operation for the token auth method|counter\rate|milliseconds|
|vault_route_rollback_cubbyhole_|Time taken to perform a route rollback operation for the Cubbyhole secret backend|counter\rate|milliseconds|
|vault_route_rollback_identity_|Time taken to perform a route rollback operation for the Identity|counter\rate|milliseconds|
|vault_route_rollback_secret_|Time taken to perform a route rollback operation for the K/V secret backend|counter\rate|milliseconds|
|vault_route_rollback_sys_|Time taken to perform a route rollback operation for the system backend|counter\rate|milliseconds|
|vault_route_rollback_ldap_|Time taken to perform a route rollback operation for the LDAP auth method|counter\rate|milliseconds|
