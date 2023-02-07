New feature called **Hedged Reads** was been introduced in Hadoop 2.4. 
The main propose of this feature is to  help to speed up reads when is takes more time than specified threshold. 
So if read from block takes longer than desired , **HDFS** starts another parallel thread for reading data from another block replica. 
This process is called **Hedged Reads** .First returned response is used and the outstanding read is cancelled.This feature is not for solving systematic problems, 
but for situation when some reads occasionally takes a long time. The benefits of Hedged Reads are:

* Reads on secondary replica 
* Strongly consistent
* Works at HDFS level   

By default **Hedged Reads** are disabled and can be used only when HFiles are stored in **HDFS**.


Following is example of enabling and setting up **Hedged Reads** in ```hbase-site.xml```, there are no default values as by its desabled by default.  

Setting Thread pool size for **Hedged Reads** . 
```xml
<property>
        <name>dfs.client.hedged.read.threadpool.size</name>
        <value>20</value>
</property>
```
In accordance to this configuration **HBase HDFS** Client will spin up 20 threads to read data from another replica of read block.  

```xml
<property>
        <name>dfs.client.hedged.read.threshold.millis</name>
        <value>10</value>
</property>
```
Here we set desired threshold for reads in milliseconds. So in above example is read takes lokger that 10ms **Hedged Reads** are started

**Monitoring the Performance of Hedged Reads**

Following metrics for monitoring Hedged Reads are emitted by Hadoop at ```http://$REGIONSERVER:$PORT/jmx```. 

* **hedgedReadOps** : The number of hedged reads that have occurred
* **hedgeReadOpsWin** : The number of times the hedged read returned faster than the original read

**OddEye** agent will automatically detect if Hedged Reads are enabled and working and these metrics will appear.: 

* **hregion_node_hedgedreads**
* **hregion_node_hedgedreadwins**
