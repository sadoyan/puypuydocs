![Lizardfs](../images/lizardfs.png)

LizardFS is a high performance distributed storage system for storing massive amount of data on commodity hardware.
PuyPuy Agent uses LizardFS's built in tools to expose statistics and send to PuyPuy servers. 
You just need to set hostname of target server and chose detalization of metrics.   

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_lizardfs.py ./
```

##### **Configure**

Configuration parameters of LizardFS check are in ``${OE_AGENT_HOME}/bonf/bigdata.ini`` file  

```ini
[LizardFS]
host: 127.0.0.1
port: 9421
chunkstats: True
diskstats: True
```
Host and port parameters points to master node. It's always a good idea to run PuyPuy Agent on target hosts, 
so for most of installtions these parameters can stay defaults. 

Other parameters enables and disabled detailed statistics of chunk servers, and disks|mountpoits 

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

**Basic Stats** 

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|lizardfs_memory_uage|Memory used by LizardFS|gauge|Bytes|
|lizardfs_total_space|Total space for LizardFS|gauge|Bytes|
|lizardfs_available_space|Available space of LizardFS|gauge|Bytes|
|lizardfs_trash_space|Space used by trashed files|gauge|Bytes|
|lizardfs_trash_files|Amount of trashed files|gauge|None|
|lizardfs_reserved_space|LizardFS reserved disk space|gauge|Bytes|
|lizardfs_reserved_files|Amount of files reserved for LizardFS|gauge|None|
|lizardfs_fs_objects|Amount of file system objects reserved for LizardFS|gauge|None|
|lizardfs_directories|Amount of directories in LizardFS|gauge|None|
|lizardfs_files|Amount of files in LizardFS|gauge|None|
|lizardfs_chunks|Amount of LizardFS chunks|gauge|None|
|lizardfs_chunk_copies|Amount of copies of LizardFS chunks|gauge|None|

**Chunserver Stats**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|lizardfs_chunksrv_total_space|Total space of particular chunk server|gauge|Bytes|
|lizardfs_chunksrv_used_space|Used space on particular chunk server|gauge|Bytes|
|lizardfs_chunksrv_chunks_mfr|Chunks marked for deletion on chunk server|gauge|Bytes|
|lizardfs_chunksrv_space_mfr|Space marked for deletion on chunk server|gauge|Bytes|
|lizardfs_chunksrv_errors|Errors on chunk server|gauge|None|

**Disks and mountpoints**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|lizardfs_disk_total_space|Total disk space of LizardFS by chunk server and mount point|gauge|Bytes|
|lizardfs_disk_used_space|Used disk space of LizardFS by chunk server and mount point|gauge|Bytes|
|lizardfs_disk_chunks|Amount of disk chunks by chunk server and mount point|gauge|Integer|
|lizardfs_read_bytes|Reads per minute by chunk server and mount point|rate|Bytes|
|lizardfs_write_bytes|Writes per minute by chunk server and mount point|rate|Bytes|
|lizardfs_max_read_time|Max read time by chunk server and mount point|curent |Microsecond|
|lizardfs_max_write_time|Max write time by chunk server and mount point|curent |Microsecond|
|lizardfs_max_fsync_time|Max file system sync time by chunk server and mount point|curent |Microsecond|
|lizardfs_read_ops|Read operations per minute by chunk server and mount point|rate|None|
|lizardfs_write_ops|Write operations pre minute by chunk server and mount point|rate|None|
|lizardfs_fsync_ops|File system sync operation by chunk server and mount point|rate|None|


There isn no necessity to run check_lizardfs.py on all nodes of LizardFS of cluster, 
statistics from all nodes can be collected from single location.  