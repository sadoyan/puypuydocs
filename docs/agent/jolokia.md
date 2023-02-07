### **Jolokia**

Nowadays world's the most popular programming language is Java. There are mumereos servers running Java, and all they need to be monitored. 
While Agent have integraton with Java JMX, in some cases JMX cannot be enabled on Servlet Container, because of political or any other reason. 
Because of all these cases we have developed Servlet Containers monitoring module, based on Jolokia WAR agent. 

Jolokia WAR agent can be deployed on any Java Servlet Container like Tomcat, Jetty, Jboss etc ...
It will transform JMX mbeans to friendly Json format and expose it via standard HTTP interface. 

Jolkia developers describe Jolokia as:
```text
JMX-HTTP bridge giving an alternative to JSR-160 connectors. 
It is an agent based approach with support for many platforms. 
In addition to basic JMX operations it enhances JMX remoting 
with unique features like bulk requests and fine grained security policies.
```
So in general, Jolokia is a small **Open Source** application, which makes DevOps lives easy for monitoring any kind of Java based servers. 
Its can be dynamically attached to running Java process via Java Attache API as well, as statically included to CLASSPATH or installed as stand alone web application.
We use Jolokia in number of our checks

```text
check_jmx (Java Attach API)
check _hbase_rest (Java Attach API)
check_hbase_thrift (Java Attach API)
chech_cassandra (By including to CLASSPATH)
chech_elasticsearch (By including to CLASSPATH)
chech_kafka (By including to CLASSPATH)
check_jolokia (As stand alone web app)
``` 

Curent manual is about installing Jolokia WAR-Agent, which runs as stand alone web application at your Servlet Engine. 
Installation of Jolokia and `check_jolokia` is very easy :

In order to deploy Jolokia WAR-Agent, you should download it from _https://jolokia.org/download.html_ and place to webapps folder. 
Depending on your Servlet container, it will be auto deployed, or you shall deploy it manually in accordance to Container deployment rules. 
After the deployment of Jolokia to you Server,  you can enable `check_jolokia` module. 

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_jolokia ./
```

##### **Configure**


Depending on your setup and jolokia-version, the metrics will be exposed trough `http://HOST:PORT/jolokia-war-X.X.X/read`
Edit `conf/java.ini` with conducting changes in section Jolokia 
```ini
[Jolokia]
jolokia: http://127.0.0.1:8888/jolokia-war-1.3.5/read
```
##### **Provides**

As jolokia check is very generic, it will take only metrics provided by `java.lang` mbean.

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|jolokia_daemonthreadcount|Amount of running java daemon threads|gauge|None|
|jolokia_gc_collectioncount|Java garbage collections count|counter|None|
|jolokia_gc_collectiontime|Time spend on GC during last iteration |rate|Milliseconds|
|jolokia_heap_committed|Committed Java Heap Memory |gauge|Bytes|
|jolokia_heap_max|Max Java Heap Memory |gauge|Bytes|
|jolokia_heap_used|Used Java Heap Memory |gauge|Bytes|
|jolokia_lastgcinfo|Last GC duration |gauge|Milliseconds|
|jolokia_nonheap_committed|Committed Java Non Heap Memory |gauge|Bytes|
|jolokia_nonheap_max|Max Java Non Heap Memory |gauge|Bytes|
|jolokia_nonheap_used|Used Java Non Heap Memory |gauge|Bytes|
|jolokia_peakthreadcount|Peak amount of running Java Threads|gauge|None|
|jolokia_threadcount|Total amount of started threads |counter|None|

All metrics will be prefixed with `jolokia_` so you can easily find them in your dashboard application. 