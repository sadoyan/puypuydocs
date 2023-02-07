**Apache ActiveMQ** is a popular and powerful open source messaging and Integration Patterns server.

It exposes its performance metrics via Jolokia agent out of the box.   
All you need is to configure **Agent** with appropriate parameters and start it.  

By Default **Apache ActiveMQ** sends metrics via `0.0.0.0:8161/api/jolokia/read` and uses `admin`:`admin` as user/pass. 
So if default configuration is not changed **Agent** will work without extra config. 

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_activemq.py ./
```

##### **Configure**
  
```ini
[ActiveMQ]
stats: http://127.0.0.1:8161/api/jolokia/read
brokername: localhost
user: admin
pass: admin
auth: True
```

If you have changed default configuration of **Apache ActiveMQ**, pleas make sure that all params in **Agent** config matches it. 
Especially on clustered installations, you will need to change `brokername: localhost` with appropriate BrokerName of your **ActiveMQ** instance. 

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|activemq_daemonthreadcount|Amount if  running Java daemon threads |current |None|
|activemq_heap_committed|Java heap committed memory |current |Bytes|
|activemq_heap_max|Java heap max memory|current |Bytes|
|activemq_heap_used|Java heap used memory |current |Bytes|
|activemq_memorypercentusage|ActiveMQ used memory percentage |current |Percent|
|activemq_nonheap_committed|Java non heap committed memory |current |Bytes|
|activemq_nonheap_max|Java non heap max memory|current |Bytes|
|activemq_nonheap_used|Java non heap used memory |current |Bytes|
|activemq_peakthreadcount|Peak amount of running Java threads |current |None|
|activemq_storepercentusage|Percentage os storage usage |current |Percent|
|activemq_threadcount|Amount of running Java non daemon threads |current |None|
|activemq_totalconnectionscount|Active connections count |current |None|
|activemq_totalconsumercount|Active consumers count |current |None|
|activemq_totaldequeuecount|Active dequeues count|current |None|
|activemq_totalenqueuecount|Active enqueues count |current |None|
|activemq_totalmessagecount|Current messages count |current |None|
|activemq_totalproducercount|Active producers count |current |None|
|activemq_{cms/g1_old}_collectioncount|CMS or G1 Old gen collections count |counter|None|
|activemq_{cms/g1_old}_collectiontime|CMS or G1 Old gen collections time |rate|Milliseconds|
|activemq_{cms/g1_old}_lastgcinfo|CMS or G1 Old gen last GC info|current |Milliseconds|
|activemq_{parneq/g1_young}_collectioncount|ParNew or G1 Young gen collections count |counter|None|
|activemq_{parneq/g1_young}_collectiontime|ParNew or G1 Young gen collections time |rate|Milliseconds|
|activemq_{parneq/g1_young}_lastgcinfo|ParNew or G1 Young gen last GC info|current |Milliseconds|
