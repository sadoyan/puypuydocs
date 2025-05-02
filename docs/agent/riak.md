##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_riak.py ./
```

##### **Configure**

Configuration of **Riak** check is located at `bigdata.ini`. By default it will look fo **Riak** installation `localhost`. 
Please make sure to set correct IP address of **Riak** instance which you want to monitor.   

Restart Agent. `./puypuy.sh restart`
`check_riak` module should run without making changes in configuration, but according to your specific needs, you can edit `conf/bindata.ini` and make changes in section **Riak**

```ini
[Riak]
stats: http://127.0.0.1:8098/stats
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|riak_mem_allocated|Amount of memory alloted for Riak instance |current |Bytes|
|riak_memory_processes|Amount of memory allocated for Erlang processe|current |Bytes|
|riak_memory_processes_used|Amount of memory used by Erlang processe|current |Bytes|
|riak_node_gets|rate of GET commands executed on current node |rate|OPS|
|riak_node_puts|rate of PUT commands executed on current node |rate|OPS|
|riak_read_repairs|rate of READ commands executed on current node |rate|OPS|
|riak_sys_process_count|Riak system prcesses count|current |None|
|riak_vnode_gets|rate of GET commands executed on current v node |rate|OPS|
|riak_vnode_puts|rate of PUT commands executed on current v node |rate|OPS|
|riak_vnode_set_update|rate of UPDATES commands executed on current node |rate|OPS|
