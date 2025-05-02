### **JMX**

All Java applications can expose metrics via JMX interface, some of them  do it by default and for others it is obligatory to be configured specially for that. 
While JMX is great way to get statistics from Java servers, it requires Java to read it. This means, that we run Java program for each attempt  to get stats or keep small Java program up and running all the time to read JMX, to convert to more common api and allow non java collector to get these stats.   

It is not effective and quite expensive to start and stop JVM each time, when we need to get statistics. Better way is to keep JVM running all the time, but in any case its another running JVM, which seems more expensive that we want to. 
 
To solve this problem we have decided to use Jolokia JVM agent, which can be attached to the running JVM as an agent and expose metrics via Json. 
`{AGENT_HOME}/lib` contains  of Jolokia JVM `agent.jar`version, which works perfectly for us, but if you need fresher or just another version of it, feel free to download JVM agent jar from https://jolokia.org/download.html and replace with it the existing `agent.jar` file.      
After downloading your prefered version of Jolokia JVM agent, make sure that you have renamed it to agent.jar 
 
Jolokia agent uses `Java Attach API` to attach agent.jar to running JVM and get statistics via HTTP/Json.
The trick is, that newest JVM's will not allow other users to attach agent to JVM. So if you are running for example Tomcat on behalf of system user `tomcat` (Hope you are not running Java as root !), agent.jar should be attached via tomcat user as well.  
To make this happen Agent must be able to execute java as user tomcat without password. Code below is an example of how `/etc/sudoers` will be configured :  
 
```text
puypuy    ALL = (tomcat) NOPASSWD: /opt/java/bin/java
```


Change `puypuy` , `tomcat` and  `/opt/java/bin/java` to actual values for your system. 


##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_jmx.py ./
```

##### **Configure**

To attach agent to running JVM, Agent needs to know the main class name, which is used to run your java application server. 
If you already do not know main class name, you can get it via command below.

```bash
cd {AGENT_HOME}
java -jar lib/agent.jar list
```

Output of it should be : 
```text
{PID} {MAIN_CLASS} {Something_else}

15425   org.apache.catalina.startup.Bootstrap start  
28240   org.apache.hadoop.hbase.rest.RESTServer start
14759   org.apache.hadoop.hbase.thrift.ThriftServer start
```

Above are examples of main classes for Tomcat, HBase Thrift and HBase Rest. 
Typically main class name should be in second column of output. Edit `conf/java.ini` and put correct fields in section `[JMX]`   

```ini
[JMX]
jmx: http://127.0.0.1:7777/puypuy/read
user: tomcat
class : org.apache.catalina.startup.Bootstrap
java : /opt/java/bin/java
```

##### **Restart** 

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

If configuration is correct, after restarting Agent daemon, you would see that your existing Java program magically started to listen `TCP:7777` and exposes JMX via HTTP/Json.
You can change port to whatever you prefer, but do not change context path ot IP address.  

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|jmx_daemonthreadcount|Amount of running java daemon threads|gauge|None|
|jmx_gc_collectioncount|Java garbage collections count|counter|None|
|jmx_gc_collectiontime|Time spend on GC during last iteration |rate|Milliseconds|
|jmx_heap_committed|Committed Java Heap Memory |gauge|Bytes|
|jmx_heap_max|Max Java Heap Memory |gauge|Bytes|
|jmx_heap_used|Used Java Heap Memory |gauge|Bytes|
|jmx_lastgcinfo|Last GC duration |gauge|Milliseconds|
|jmx_nonheap_committed|Committed Java Non Heap Memory |gauge|Bytes|
|jmx_nonheap_max|Max Java Non Heap Memory |gauge|Bytes|
|jmx_nonheap_used|Used Java Non Heap Memory |gauge|Bytes|
|jmx_peakthreadcount|Peak amount of running Java Threads|gauge|None|
|jmx_threadcount|Total amount of started threads |counter|None|
