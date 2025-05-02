![Cassandra](../images/cassandra.png)

Apache Cassandra is massively scalable Open-Source NoSQL database, 
initially created by Facebook for their messaging system and it is opensource and contributed to Apache Software Foundation.
Cassandra's native way of matrics collection is via JMX, 
but we prefered to use a great project, named Jolokia to collect metrics from Cassandra. 
As the matter of fact we use Jolokia very actively and we are very thankful them for creating such a great tool. 
Cassandra Jolokia integration is very easy and works perfectly, to grab JMX metrics via user friendly HTTP/Json interface. 
Agent have two modules for Cassandra: `check_cassandra.py` and `check_cassandra3.py`. 
first one is for collecting metrics from Cassandra 2.0xx and Cassandra 2.1xx, the second is for Cassandra 2.2 and upper. 

Enablling Cassandra checks has two parts: 

* Configure Caassandra to expose metrics via Jolokia 
* Enable one of cassandra checks and confiugre Agent with Jolokia access parameters. 


#### **Install**


```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_cassandra.py ./
```

or 

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_cassandra3.py ./
```

#### **Configure**

**Cassandra with Jolokia**

First download Jolokia 	jolokia-jvm-VESION-agent.jar from https://jolokia.org/download.html  and copy it to All nodes of Cassandra cluster.

```bash
cd /usr/share/java/
wget -O jolokia-agent.jar http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.3.6/jolokia-jvm-1.3.6-agent.jar
```

Now we need to edit `cassandra-env.sh` and add `javaagent` JVM option: 

```bash
JVM_OPTS="$JVM_OPTS -javaagent:/usr/share/java/jolokia-agent.jar=config=/etc/cassandra/jolokia/jolokia.properties"
```

Now we need to configure Jolokia: For packaged Cassandra installations copy-paste of code below will do the trick. 
If you have tarball Cassandra installation, just replace /etc/cassandra with actual location of your config files . 

```xml
mkdir /etc/cassandra/jolokia
cat > /etc/cassandra/jolokia/jolokia.policy <<-EOF
<?xml version="1.0" encoding="utf-8"?>

<restrict>
  <http>
    <method>get</method>
  </http>

  <commands>
    <command>read</command>
    <command>list</command>
  </commands>
</restrict>
EOF

cat > /etc/cassandra/jolokia/jolokia.properties <<-EOF
host=0.0.0.0
port=7777
agentContext=/jolokia
backlog=100
policyLocation=file:///etc/cassandra/jolokia/jolokia.policy
historyMaxEntries=10
debug=false
debugMaxEntries=100
maxDepth=15
maxCollectionSize=1000
maxObjects=0
EOF
```
Now you need to Cassandra daemon. 

**Agent**

Cassandra check is enabled as any other check, so copy or symlinc apropriate cassandra check from `checks_available` to `checks_enabled`.
Edit conf/bigdata.ini, it already contains reasonable defaults to work out of the box, 
but if your Cassandra and Jolokia are configured in different way, than described below, just replace default parameters with desired ones. 

```ini
[Cassandra]
jolokia: http://127.0.0.1:7777/jolokia/read
```

#### **Restart**

```commandline
${OE_AGENT_HOME}/puypuy.sh restart
```

#### **Provides**

**For older versions of Cassandra `check_cassandra.py`**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|cassa_{cms or g1_old}_collection_count|CMS or G1 old generations GC count|counter |None|
|cassa_{cms or g1_old}_collection_time|CMS or G1 old generations GC time|rate|Milliseconds |
|cassa_{cms or g1_old}_lastgcinfo|CMS or G1 old generations last GC info|gauge|Milliseconds |
|cassa_daemonthreadcount|Running Java daemon threads |gauge|None|
|cassa_heap_committed|Java Heap committed  |gauge|Bytes|
|cassa_heap_max|Java Heap Max|gauge|Bytes|
|cassa_heap_used|Java Heap Used |gauge|Bytes|
|cassa_keycachehits|Cassandra Key cache hits |rate|OPS|
|cassa_keycacherequests|Cassandra Key cache requests |rate|OPS|
|cassa_mutationstage|Cassandra mutations requests |rate|OPS|
|cassa_native_transport_requests|Cassandra native transport (CQL) requests |rate|OPS|
|cassa_nonheap_committed|Java non Heap committed  |gauge|Bytes|
|cassa_nonheap_max|Java non Heap Max|gauge|Bytes|
|cassa_nonheap_used|Java non Heap Used |gauge|Bytes|
|cassa_{parnew or g1_young}_collection_count|ParNew or G1 young generations GC count|counter |None|
|cassa_{parnew or g1_young}_collection_time|ParNew or G1 young generations GC time|rate|Milliseconds |
|cassa_{parnew or g1_young}_lastgcinfo|ParNew or G1 young generations GC lastgcinfo|gauge|Milliseconds |
|cassa_peakthreadcount|Peak amount of running daemon threads |gauge|None|
|cassa_pending_compactions|Cassandra pending compactions |gauge|None|
|cassa_readstage|Cassandra queries at read stage |rate|OPS|
|cassa_requestresponsestage|Cassandra requests and request/response stage |rate|OPS|
|cassa_threadcount|Running non daemon threads count|gauge|None|
|cassa_totalstartedthreadcount|Total started threads count |counter |None|

**For newer versions of Cassandra `check_cassandra3.py`**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|cassa_{cms or g1_old}_collection_count|CMS or G1 old generations GC count|counter|None|
|cassa_{cms or g1_old}_collection_time|CMS or G1 old generations GC time|rate|Milliseconds|
|cassa_{cms or g1_old}_lastgcinfo|CMS or G1 old generations last GC info|gauge|Milliseconds|
|cassa_compaction_pending|Cassandra pending compactions|gauge|None|
|cassa_cql_preparedstatementsexecuted|Cassandra prepared CQL statements execution|rate|OPS|
|cassa_cql_regularstatementsexecuted|Cassandra regular CQL statements execution|rate|OPS|
|cassa_daemonthreadcount|Running Java daemon threads count|gauge|None|
|cassa_heap_committed|Java Heap Used|gauge|Bytes|
|cassa_heap_max|Java Heap Max|gauge|Bytes|
|cassa_heap_used|Java Heap Used|gauge|Bytes|
|cassa_hits_keycache|Cassandra Key cache hits|rate|OPS|
|cassa_hits_rowcache|Cassandra Row cache hits|rate|OPS|
|cassa_nonheap_committed|Java non Heap Committed|gauge|Bytes|
|cassa_nonheap_max|Java non Heap Max|gauge|Bytes|
|cassa_nonheap_used|Java non Heap Used|gauge|Bytes|
|cassa_{parnew or g1_young}_collection_count|Parnew or G1 young generations GC count|counter|None|
|cassa_{parnew or g1_young}_collection_time|Parnew or G1 young generations GC time|rate|Milliseconds|
|cassa_{parnew or g1_young}_lastgcinfo|Parnew or G1 young generations Last GC info|gauge|Milliseconds|
|cassa_peakthreadcount|Cassandra Peak threads count|gauge|None|
|cassa_requests_keycache|Cassandra Key cashe requests |rate|OPS|
|cassa_requests_rowcache|Cassandra Row cashe requests|rate|OPS|
|cassa_threadcount|Running non daemon threads count|gauge|None|
|cassa_totalstartedthreadcount|Total started threads count|counter|None|
|cassa_latency_casread|Latency of CASRead Statements|gauge|Microseconds|
|cassa_latency_caswrite|Latency of CASWrite Statements|gauge|Microseconds|
|cassa_latency_rangeslice|Latency of RangeSlice Statements|gauge|Microseconds|
|cassa_latency_read|Latency of Read Statements|gauge|Microseconds|
|cassa_latency_viewwrite|Latency of ViewWrite Statements|gauge|Microseconds|
|cassa_latency_write|Latency of Write Statements|gauge|Microseconds|
