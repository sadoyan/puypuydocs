Apache Kafka is distributed stream processing engine, written in Java/Scala, initially developed by Linked in. Indeed it's a top level Apache foundation product.

 
Enablling Kafka checks has two parts: 

* Configure Kafka to expose metrics via Jolokia 
* Kafka checks and configure Agent with Jolokia access parameters. 

##### **Install**
At first download Jolokia 	jolokia-jvm-VERSION-agent.jar from https://jolokia.org/download.html  and copy it to All nodes of Kafka cluster.

```bash
cd /usr/share/java/
wget -O jolokia-agent.jar http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.3.6/jolokia-jvm-1.3.6-agent.jar
```

Now we need to edit `kafka-server-start.sh` and add `javaagent` JVM option: lets assume that you have installed Kafka at `/opt/kafka` directory. 

```bash
export  KAFKA_OPTS="$KAFKA_OPTS -javaagent:/usr/share/java/jolokia-agent.jar=config=/opt/kafka/config/jolokia/jolokia.properties"
```

**Jolokia**

Now we need to configure Jolokia: For packaged Kafka installations copy-paste of code below will do the trick. 
If you have tarball Kafka installation, just replace paths with actual location of your config files . 

```bash
mkdir /opt/kafka/config/jolokia
cat > /opt/kafka/config/jolokia/jolokia.policy <<-EOF
<?xml version="1.0" encoding="utf-8"?>
<restrict>
  <http>
    <method>get</method>
    <method>post</method>
  </http>
  <commands>
    <command>read</command>
    <command>list</command>
    <command>search</command>
  </commands>
</restrict>
EOF
```
```bash
cat > /opt/kafka/config/jolokia/jolokia.properties <<-EOF
host=0.0.0.0
port=7777
agentContext=/jolokia
backlog=100
policyLocation=file:///opt/kafka/config/jolokia/jolokia.policy
historyMaxEntries=10
debug=false
debugMaxEntries=100
maxDepth=15
maxCollectionSize=1000
maxObjects=0
EOF
```

Restart Kafka node. 

**Agent**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_kafka.py ./
```
Kafka check is enabled as any other check: so copy or symlink `check_kafka.py` check from `checks_available` to `checks_enabled`.

##### **Configure**

Edit `conf/mq.ini`, it already contains reasonable defaults to work out of the box, but if your Kafka and Jolokia are configured in other than described below way, just replace default parameters with desired ones. 

```ini
[Kafka]
jolokia: http://127.0.0.1:7777/jolokia/read
```

##### **Restart**

```bash
{AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

After several seconds you can start building your Kafka dashboards. Metrics will be prefixed with `kafka_` .

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|kafka_bytesinpersec|Amount of incoming bytes to Kafka node |rate|Bytes|
|kafka_bytesoutpersec|Amount of outgoing bytes to Kafka node |rate|Bytes|
|kafka_bytesrejectedpersec|Amount of rejected bytes to Kafka node |rate|Bytes|
|kafka_failedfetchrequestspersec|Amount of failed fetch requests to node |rate|OPS|
|kafka_failedproducerequestspersec|Amount of failed produce requests to node |rate|OPS|
|kafka_fetch|Fetches on current node |rate|OPS|
|kafka_fetchconsumer|Fetch consumers on current node |rate|OPS|
|kafka_fetchfollower|Fetch followers on current node |rate|OPS|
|kafka_gc_old_collectioncount|CMS or G1 Old gen garbage collections count|counter|None|
|kafka_gc_old_collectiontime|CMS or G1 Old gen garbage collections time|rate|Milliseconds|
|kafka_gc_old_lastgcinfo|CMS or G1 Old gen last garbage collections duration |gauge|Milliseconds|
|kafka_gc_young_collectioncount|ParNew or G1 Young gen garbage collections count|rate|Milliseconds|
|kafka_gc_young_collectiontime|ParNew or G1 Young gen garbage collections time|rate|Milliseconds|
|kafka_gc_young_lastgcinfo|ParNew or G1 Young gen last garbage collections duration |gauge|Milliseconds|
|kafka_groupcoordinator|Kafka group coordinators |gauge|None|
|kafka_heap_committed|Java Heap memory committed |gauge|Bytes|
|kafka_heap_used|Java Heap memory used|gauge|Bytes|
|kafka_joingroup|Kafka join groups|rate|OPS|
|kafka_leaderandisr|LeaderAndIsr requests performed ont his node |rate|OPS|
|kafka_messagesinpersec|Incomming messages rae to current node |rate|OPS|
|kafka_nonheap_committed|Java Non Heap memory committed |gauge|Bytes|
|kafka_nonheap_used|Java Non Heap memory used|gauge|Bytes|
|kafka_offsetcommit|Offset commits on current node |rate|OPS|
|kafka_offsetfetch|Offset fetched on current node|rate|OPS|
|kafka_offsets|Kafka Offsets |gauge|None|
|kafka_produce|Prodeucers per second on current node|rate|OPS|
|kafka_totalproducerequestspersec|Kafka total produced requess per second |rate|OPS|


