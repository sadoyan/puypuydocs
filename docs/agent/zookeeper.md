**Apache Zookeeper** is a highly reliable distributed coordination system, which is build on ideas of Google's Chubby.
Lots of distributed systems (HBase, Kafka, etc .. ) use **Apache Zookeeper** coordinator service, 
so monitoring of **Zookeeper** service is very critical for these infrastructures. 

**Agent** natively supports **Zookeeper** metrics and all is needed to point **Agent** to right  **Zookeeper** host/port.

Configuration of `check_zookeeper` is stored in `conf/hadoop.ini` file. 
For most of setups it does not needs extra configuration, but if you run **Zookeeper** on different from default host/port, 
edit `conf/hadoop.ini` section `ZooKeeper` with setting of your **ZooKeeper** service: 

#### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_zookeeper.py ./
```

#### **Configure**

```ini
[ZooKeeper]
host: 127.0.0.1
port: 2181
```

#### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

#### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|zk_approximate_data_size|Approximate data size in bytes|gauge|Bytes|
|zk_avg_latency|Average latency of requests|gauge|Milliseconds|
|zk_ephemerals_count|Ephemeral items count|gauge|None|
|zk_max_latency|Requests max latency|gauge|Milliseconds|
|zk_min_latency|Requests min latency|gauge|Milliseconds|
|zk_open_file_descriptor_count|ZooKeeper daemon open files descriptors count |gauge|None|
|zk_outstanding_requests|Outstanding requests count|gauge|None|
|zk_packets_received|ZooKeeper packets received per second|rate|OPS|
|zk_packets_sent|ZooKeeper packets sent per second|rate|OPS|
|zk_watch_count|ZooKeeper watches count|gauge|None|
|zk_znode_count|ZooKeeper Znodes count |gauge|None|
