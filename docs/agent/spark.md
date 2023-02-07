## **Apache Spark**

Apache Spark is a fast and general engine for large-scale data processing.

In order to gather Spark workers statistics, we need to download and enable **Jolokia JVM Agent**    

### **Jolokia** 

```bash
cd /usr/share/java/
wget -O jolokia-agent.jar http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.3.6/jolokia-jvm-1.3.6-agent.jar
```

### **Spark Master** 

As far, as **Jolokia JVM Agent** is downloaded, we should configure **Apache Spark**, to use it as JavaAgent for workers and expose metrics via HTTP/Json.
Edit `spark-env.sh`. It should be in `/usr/local/spark/conf`  and add following parameters : 
 
    export SPARK_MASTER_OPTS="$SPARK_MASTER_OPTS -javaagent:/usr/share/java/jolokia-agent.jar=config=/usr/local/spark/conf/jolokia-master.properties"
 
Now create `/usr/local/spark/conf/jolokia-master.properties` file with following content 
(Assuming that spark install folder is `/usr/local/spark`, if not change the pathe to one on which Spark is installed ): 

```ini
host=0.0.0.0
port=7777
agentContext=/jolokia
backlog=100

policyLocation=file:///usr/local/spark/conf/jolokia.policy
historyMaxEntries=10
debug=false
debugMaxEntries=100
maxDepth=15
maxCollectionSize=1000
maxObjects=0
```

Now we need to create `/usr/local/spark/conf/jolokia.policy` with following content : 
```xml
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
```

Configure Agent with following in `conf/bigdata.ini` file : 

```ini
[Spark-Master]
stats: http://127.0.0.1:7777/jolokia/read
```

Restart Spark master. 
  
### **Spark worker**

Edit `spark-env.sh`. It should be in `/usr/local/spark/conf`  and add following parameters : 

```bash
export SPARK_WORKER_OPTS="$SPARK_WORKER_OPTS -javaagent:/usr/share/java/jolokia-agent.jar=config=/usr/local/spark/conf/jolokia-worker.properties"
``` 

Now create `/usr/local/spark/conf/jolokia-worker.properties` file with following content 
(Assuming that spark install folder is `/usr/local/spark`, if not, change the path to one on which Spark is installed ): 
```ini
host=0.0.0.0
port=7778
agentContext=/jolokia
backlog=100

policyLocation=file:///usr/local/spark/conf/jolokia.policy
historyMaxEntries=10
debug=false
debugMaxEntries=100
maxDepth=15
maxCollectionSize=1000
maxObjects=0
```

Create `/usr/local/spark/conf/jolokia.policy` with following content : 
```xml
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
```

Configure Agent with following in `conf/bigdata.ini` file : 
```ini
[Spark-Worker]
stats: http://127.0.0.1:7778/jolokia/read
```

Restart Spark worker. 
