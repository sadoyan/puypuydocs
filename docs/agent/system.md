**Linux OS Monitoring**
---------

#### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/{check_cpustats.py,check_disks.py,check_load_average.py,check_memory.py,check_network_bytes.py} ./
```

#### **Configure**

For most of installations, our defaults works well, but you can edit `conf/system.ini` if you need fine tuning. 
For each of system check you van enable or disable static alerts and set appropriate thresholds. 

```ini
[Load Average]
static_enabled: True
high: 95
severe : 100

[Disk Stats]
static_enabled: True
high : 90
severe : 95

[Memory Stats]
static_enabled: True
high : 90
severe : 95

[CPU Stats]
static_enabled: True
percore_stats: False
high: 90
severe: 95

[Network Stats]
localhost = False
rated = True
```

#### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **CPU**

System CPU statistics are collected via check_cpustats.py module: It reads /proc/stat file for CPU related information. 
The Check does not require any additional dependencies, and it should provide following metrics: 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|cpu_idle|percent of free CPU resources|gauge|Percent|
|cpu_iowait|CPU percent spent on waiting for I/O operation|gauge|Percent|
|cpu_irq|CPU percent spent on handling hardware interrupts |gauge|Percent|
|cpu_load|Total CPU/Core load percent|gauge|Percent|
|cpu_nice|CPU percent of processing user level processes with positive nice |gauge|Percent|
|cpu_softirq|CPU percent spent on handling soft interrupts |gauge|Percent|
|cpu_system|CPU percent of processing system level processes|gauge|Percent|
|cpu_user|CPU percent of processing user level processes|gauge|Percent|


##### **Memory**

Collected via check_memory.py module, it takes memory related information from /proc/meminfo file, and provides following metrics: 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|mem_active|Memory that is being used by a particular process|gauge|Bytes|
|mem_available|Not active / free memory |gauge|Bytes|
|mem_buffers|The total amount of memory used for critical system buffers|gauge|Bytes|
|mem_cached|Amount of cached data. Free’d if there is not enough free memory in the system.|gauge|Bytes|
|mem_inactive|Memory that was allocated to a process that is no longer running|gauge|Bytes|
|mem_swapcached|Amount of swapped memory|gauge|Bytes|
|mem_total|Total amount of memory |gauge|Bytes|
|mem_used_percent|Aggregated from metrics above, total memory usage percentage |gauge|Percent|


##### **Disk**

Collected via check_disks.py module, this check uses several resources, to provide statistics about disk IO and Space usage. 
Read/write statistics are taken from /sys/block/{DISK_NAME}/stat files, also we use Linux df command, to get information about space usage. 
IO statistics are taken from /proc/diskstats. For each disk we take following metrics: 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|drive_bytes_available|Amounts of unused bytes|gauge|Bytes|
|drive_bytes_used|Amounts of used bytes|gauge|Bytes|
|drive_io_percent_used|Percent of used IO resources  per second of disk drive |gauge|Percent|
|drive_percent_used|Percentage of disk space usage |gauge|Percent|
|drive_reads|Read operations per second performed on disk drive |rate|Bytes|
|drive_writes|Write operations per second performed on disk drive |rate|Bytes|


##### **Network**

Collected via check_network_bytes.py module, It collects metrics about all installed interfaces by reading /sys/class/net/{NIC}/statistics/rx_bytes file. 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|bytes_rx|Amounts of received bytes|rate|Bytes|
|bytes_tx|Amounts of sent bytes|rate|Bytes|

##### **IP Conntrack**

Enable this module only if you use connection tracking. 
This check makes sense, if you have router like system, 
or if by some reason  nf_conntrack kernel module is loaded. 
It reads /proc/sys/net/ipv4/netfilter/ip_conntrack_max|ip_conntrack_count files and provides : 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|conntrack_max|Maximum configured Conntack value|gauge|None|
|conntrack_cur|Current IP Conntack value|gauge|None|

##### **Load Average**

This is one of the most important metrics in Linux (Maybe even the most). 
System load average shows ammount of processes, waiting in queue for CPU. 
It is calculater by 1,5 and 15 minute averages.In most of live systems it must have a value, 
which is less than total amount of CPUs detected by system. 
For example of your Server is equiped with 2x Quad cores CPUs, 
with enabled hyper trading Linux will see 16 CPUs, so Load Average should be below 16. Our check provides: 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|sys_load_1|System load average for last minute|gauge|None|
|sys_load_5|System load average for last 5 minutes|gauge|None|
|sys_load_15|System load average for last 15 minutes|gauge|None|


As regular checks and also triggers special WARNING, when value of sys_load_1 is more that 90%  of amount of CPU cores and ERROR,
 when its equal or more that 100%. This behavior can be changed by editing check_load_average.py and changing : 

```ini
warn_level = 90
crit_level = 100
```

to desired values. However these values are quite reasonable so, use it without modifications, if you are not for 100% sure that you need to change it 

##### **TCP Connections**

This check provides status of TCP connections to systems.  It parses `/proc/net/tcp` and provides following metrics. 

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|tcp_close|TCP connections with CLOSED state|gauge|None|
|tcp_close_wait|The remote side has been shut down and is now waiting for the socket to close|gauge|None|
|tcp_closing|TCP connections in  closing state |gauge|None|
|tcp_established|The socket has a connection established|gauge|None|
|tcp_fin_wait1|The socket is closed, and the connection is now shutting down.|gauge|None|
|tcp_last_ack|TCP connections in  last ack state |gauge|None|
|tcp_listen|TCP listening sockets|gauge|None|
|tcp_max_states|TCP connections in  max state |gauge|None|
|tcp_new_syn_recv|TCP connections in  new syn recv state |gauge|None|
|tcp_syn_recv|TCP connections in  syn recvstate |gauge|None|
|tcp_syn_sent|TCP connections in  syn recv state |gauge|None|
|tcp_time_wait|TCP connections in  time wait state |gauge|None|

On some very heavy loaded systems, this check may become expensive, 
please make sure its suits your system resources before enabling it on systems with 20k+ TCP ESTABLISHED connections.   

##### **BTRFS check**

BTRFS check can be very useful in combinations with regular Drive IO checks on systems which uses BTRFS file system. 
Its monitors BTRFS volumes and checks for volume errors. 
It also contains `special` check which will send manually defined `ERROR` and `WARNING` messages if values of checked parameters are above Zero.
Manual Error handling can be enabled or disable by setting up `enable_special` variable at the top of script. Its accepts `True` or `False` parameters, defaults is `True`.  

**Provides**

```text
btrfs_dev_{NAME}_corruption_errs
btrfs_dev_{NAME}_flush_io_errs
btrfs_dev_{NAME}_generation_errs
btrfs_dev_{NAME}_read_io_errs
btrfs_dev_{NAME}_write_io_errs
```
