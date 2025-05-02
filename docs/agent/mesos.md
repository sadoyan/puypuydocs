
![Mesos](../images/mesos.png)  

### **Mesos Master** 

Mesos master exposes metrics via HTTP/Json out of the box, so all you need is just to symlink `checks_available/check_mesos_master.py` to `checks_enabled` and restart Agent.

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_mesos_master.py ./
```

##### **Configure**

At most of cases there is no need to configure **Agent**, but if you have non default installation of Mesos, 
or if you need to monitor Mesos Master, which is not running locally for Agent node, 
edit `conf/bigdata.ini` and make your changes at Mesos-Master section. 

```ini
[Mesos-Master]
stats: http://127.0.0.1:5050/metrics/snapshot
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|mesos_allocator_mesos_allocation_run_ms|Duration of running mesos allocations |gauge|Milliseconds|
|mesos_allocator_mesos_allocation_run_ms_p99|99 Percentile Duration of running mesos allocations |gauge|Milliseconds|
|mesos_master_cpus_percent|Percent of in Use mesos master CPUs|gauge|Percent|
|mesos_master_cpus_revocable_percent|Mesos Master's CPUs revocable percentage|gauge|Percent|
|mesos_master_cpus_used|Amount used Mesos master CPUs|gauge|None|
|mesos_master_disk_percent|Mesos Master's dids usage in percent|gauge|Percent|
|mesos_master_disk_revocable_percent|Mesos Master disks revocable  in percent|gauge|Percent|
|mesos_master_disk_used|Meso Master disks usage in bytes |gauge|Bytes|
|mesos_master_event_queue_dispatches|Master's dispatched event queue |gauge|None|
|mesos_master_event_queue_http_requests|Master's HTTP requests to event queue |gauge|None|
|mesos_master_event_queue_messages|Message in Masters even queue |gauge|None|
|mesos_master_frameworks_active|Amount of current active frameworks|gauge|None|
|mesos_master_gpus_percent|Percent of in Use mesos master GPUs|gauge|Percent|
|mesos_master_gpus_used|Amount used Mesos master GPUs|gauge|None|
|mesos_master_mem_percent|Masters memory usage in percent|gauge|Percent|
|mesos_master_mem_used|Masters memory usage in bytes|gauge|Bytes|
|mesos_master_messages_kill_task|Mater kill tasks |gauge|None|
|mesos_master_messages_reregister_framework|Master registering frameworks |gauge|None|
|mesos_master_slaves_connected|Amount of connected slaves|gauge|None|
|mesos_master_tasks_dropped|Amount of dropped tasks |counter|None|
|mesos_master_tasks_error|Amount of tasks with errors |counter|None|
|mesos_master_tasks_failed|Amount of failed tasks |counter|None|
|mesos_master_tasks_finished|Amount of finished tasks |counter|None|
|mesos_master_tasks_gone|Amount of gone tasks |gauge|None|
|mesos_master_tasks_lost|Amount of lost tasks |gauge|None|
|mesos_master_tasks_running|Amount of running tasks |gauge|None|
|mesos_master_tasks_staging|Amount of staging tasks |gauge|None|
|mesos_master_tasks_starting|Amount of starting tasks |gauge|None|
|mesos_master_tasks_unreachable|Amount of unreachable tasks |gauge|None|
|mesos_registrar_state_fetch_ms|Registrars state fetched in milliseconds |gauge|Milliseconds|
|mesos_registrar_state_store_ms_p99|99 percentile  of registrars state fetched in milliseconds|gauge|Milliseconds|


### **Mesos Slave**

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_mesos_slave.py ./
```

##### **Configure**

At most of cases there is no need to configure **Agent**, but if you have non default installation of Mesos, 
or if you need to monitor Mesos Master, which is not running locally for Agent node, 
edit `conf/bigdata.ini` and make your changes at Mesos-Master section. 

```ini
[Mesos-Slave]
stats: http://127.0.0.1:5051/metrics/snapshot
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|mesos_slave_cpus_percent|Slaves CPU usage in percent|gauge|Percent|
|mesos_slave_cpus_revocable_used|Slaves CPU revocable usage |gauge|None|
|mesos_slave_cpus_used|Slave CPUs used |gauge|None|
|mesos_slave_disk_percent|Slave disks usage percentage|gauge|Percent|
|mesos_slave_disk_used|Slave disks usage bytes |gauge|Bytes|
|mesos_slave_executors_running|Amount of running executors |gauge|None|
|mesos_slave_executors_terminated|Amount of terminated executors |counter|None|
|mesos_slave_executors_terminating|Amount of terminating executors |gauge|None|
|mesos_slave_frameworks_active|Active frameworks on current node|gauge|None|
|mesos_slave_gpus_revocable_used|Slaves GPU revocable usage |gauge|None|
|mesos_slave_gpus_used|Slaves GPU usage in percent|gauge|Percent|
|mesos_slave_invalid_status_updates|Invalid status update |gauge|None|
|mesos_slave_mem_percent|Slave used memory percentage|gauge|Percent|
|mesos_slave_mem_revocable_percent|Slave memory revocable percentage|gauge|Percent|
|mesos_slave_mem_used|Slave used memory in bytes |gauge|Bytes|
|mesos_slave_recovery_errors|Recover errors |gauge|None|
|mesos_slave_tasks_failed|Amount of failed tasks on current node |gauge|None|
|mesos_slave_tasks_killing|Amount of failed killing on current node |gauge|None|
|mesos_slave_tasks_lost|Amount of lost killed on current node |gauge|None|
|mesos_slave_tasks_running|Amount of running tasks on current node |counter|None|


